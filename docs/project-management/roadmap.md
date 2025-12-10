# Basic Roadmap 
NOTE: THIS IS TEMPLATE FOR NOW, (in-progress!)


Simple, overall view of the process.
# Phase 0 - Tutorial
- Learn and familiarize yourself with superset.
- Build dota league insights, which are intuitive to you and all the data is already present.

# Phase 1 Roadmap — PwR-Only Superset + Data Model

## 0. Goals for Phase 1

* ✅ Use **PwR as the only tenant** (hardcoded org assumptions are fine).
* ✅ Turn **raw Discord voice logs** into:

  * `presence` (point events)
  * `voice_sessions` (durations)
  * `events` (what we planned to run)
  * `event_attendance` (who actually showed, for how long)
* ✅ Expose a **small set of clean Superset datasets**:

  * Member activity & time in voice
  * Event performance & attendance
  * Early retention/engagement metrics (simple first, more advanced later)
* ✅ No multi-tenancy yet, but design schemas so `org_id` can be bolted on later.

---

## 1. Data Sources and Staging

### 1.1. Define Raw Voice Logs Staging Table

Assuming you’re already logging join/leave data from the bot, standardize it into a **single staging table**:

```sql
-- schema: pwr_raw
CREATE TABLE pwr_raw.discord_voice_events (
    id BIGSERIAL PRIMARY KEY,
    discord_user_id BIGINT NOT NULL,
    discord_guild_id BIGINT NOT NULL,
    discord_channel_id BIGINT NOT NULL,
    event_type TEXT NOT NULL,           -- 'join', 'leave', maybe 'move'
    event_ts TIMESTAMPTZ NOT NULL,      -- when the event occurred
    raw_payload JSONB NOT NULL          -- original Loki/log payload, optional
);
```

**Tasks**

* [ ] Make the bot write *all* voice events to this table (ideally via a queue → worker → Postgres).
* [ ] Backfill historical data if you have it in logs (optional but nice).
* [ ] Ensure timestamps are in UTC consistently.

### 1.2. Member Dimension Table

You’ll need a reasonably stable **dimension for members**:

```sql
-- schema: pwr_core
CREATE TABLE pwr_core.members (
    member_id BIGSERIAL PRIMARY KEY,
    discord_user_id BIGINT UNIQUE NOT NULL,
    first_seen_ts TIMESTAMPTZ NOT NULL,
    last_seen_ts TIMESTAMPTZ,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    display_name TEXT,
    username TEXT,
    join_date TIMESTAMPTZ,             -- to PwR/club
    leave_date TIMESTAMPTZ             -- nullable
);
```

**Tasks**

* [ ] Build a sync job from Discord to populate this table (on first sighting of user in voice or other events).
* [ ] Update `last_seen_ts` whenever new activity is observed.
* [ ] Start capturing `join_date` and `leave_date` when membership logic triggers.

---

## 2. Reconstructing Presence & Sessions

### 2.1. Logical Presence Model

Define a **logical model** of presence:

* Presence is a set of **voice sessions**:

  * `session_start_ts`
  * `session_end_ts`
  * `duration_seconds`
  * `member_id`
  * `guild_id`
  * `channel_id`

### 2.2. Build `voice_sessions` Table

```sql
-- schema: pwr_core
CREATE TABLE pwr_core.voice_sessions (
    session_id BIGSERIAL PRIMARY KEY,
    member_id BIGINT NOT NULL REFERENCES pwr_core.members(member_id),
    discord_guild_id BIGINT NOT NULL,
    discord_channel_id BIGINT NOT NULL,
    session_start_ts TIMESTAMPTZ NOT NULL,
    session_end_ts TIMESTAMPTZ,
    duration_seconds INTEGER,
    is_completed BOOLEAN NOT NULL DEFAULT FALSE
);
```

**Tasks**

* [ ] Implement a **session builder** job:

  * Consumes `pwr_raw.discord_voice_events` ordered by `(discord_user_id, event_ts)`.
  * For each `join`, open a session.
  * For each corresponding `leave` (or move), close the current session, set `duration_seconds`.
  * For bot restarts/crashes: add a safety rule (e.g., auto-close very old open sessions at some cutoff).
* [ ] Schedule this job:

  * Either realtime inside bot worker or periodic ETL (e.g., every minute).

### 2.3. Daily Aggregates (Optional in Phase 1, but recommended)

Create a **daily summary** table for Superset performance:

```sql
-- schema: pwr_agg
CREATE TABLE pwr_agg.member_voice_daily (
    activity_date DATE NOT NULL,
    member_id BIGINT NOT NULL REFERENCES pwr_core.members(member_id),
    total_sessions INT NOT NULL,
    total_voice_seconds BIGINT NOT NULL,
    PRIMARY KEY (activity_date, member_id)
);
```

**Tasks**

* [ ] Nightly job that:

  * Aggregates `voice_sessions` into this table.
  * Updates only the last N days to handle late updates.

---

## 3. Events & Event Attendance

Even in Phase 1, assume **events are “real objects”** in your warehouse.

### 3.1. `events` Table

```sql
-- schema: pwr_core
CREATE TABLE pwr_core.events (
    event_id BIGSERIAL PRIMARY KEY,
    external_event_id TEXT,          -- e.g., Discord event ID, if used
    name TEXT NOT NULL,
    description TEXT,
    event_type TEXT,                 -- 'scrim', 'inhouse', 'meeting', etc.
    scheduled_start_ts TIMESTAMPTZ NOT NULL,
    scheduled_end_ts TIMESTAMPTZ,
    actual_start_ts TIMESTAMPTZ,
    actual_end_ts TIMESTAMPTZ,
    host_member_id BIGINT REFERENCES pwr_core.members(member_id),
    status TEXT NOT NULL DEFAULT 'scheduled' -- 'scheduled','completed','cancelled'
);
```

**Tasks**

* [ ] Hook your event-creation flow (bot or portal) to write to this table.
* [ ] Ensure actual start/end times are recorded (manually or via bot commands).

### 3.2. `event_attendance` Table

This is where presence + events meet.

```sql
-- schema: pwr_core
CREATE TABLE pwr_core.event_attendance (
    event_id BIGINT NOT NULL REFERENCES pwr_core.events(event_id),
    member_id BIGINT NOT NULL REFERENCES pwr_core.members(member_id),
    first_join_ts TIMESTAMPTZ NOT NULL,
    last_leave_ts TIMESTAMPTZ NOT NULL,
    total_voice_seconds INTEGER NOT NULL,
    joined_on_time BOOLEAN,
    PRIMARY KEY (event_id, member_id)
);
```

**Tasks**

* [ ] Define logic: “A member is counted as attending if they are in any event-associated voice channel between actual_start_ts and actual_end_ts.”
* [ ] Implement ETL that:

  * Looks at `voice_sessions` overlapping an event’s timeframe.
  * Sums `duration_seconds` per `(event_id, member_id)`.
  * Sets `joined_on_time` by comparing first join vs `scheduled_start_ts` + grace period.
* [ ] Run ETL for events after they complete (e.g. Celery job triggered on event completion).

---

## 4. Early Engagement / Retention / Churn Foundations

Don’t overcomplicate in Phase 1. Build **just enough** for future churn models.

### 4.1. Engagement Scores (Simple)

Create an aggregate table:

```sql
-- schema: pwr_agg
CREATE TABLE pwr_agg.member_engagement_weekly (
    week_start_date DATE NOT NULL,
    member_id BIGINT NOT NULL REFERENCES pwr_core.members(member_id),
    total_voice_seconds BIGINT NOT NULL,
    events_attended INT NOT NULL,
    distinct_event_types INT NOT NULL,
    PRIMARY KEY (week_start_date, member_id)
);
```

**Tasks**

* [ ] Weekly job to aggregate:

  * Sum of voice_seconds (from `member_voice_daily`).
  * Count of events attended and distinct event types (from `event_attendance`).
* [ ] This becomes your base “engagement” dataset.

### 4.2. Cohort & Retention Seeds

You don’t need full cohort analysis yet, but seed the data model:

* Ensure `join_date` on `pwr_core.members` is correct.
* Start calculating a simple **“active this month?”** flag.

```sql
-- schema: pwr_agg
CREATE TABLE pwr_agg.member_monthly_status (
    month_start_date DATE NOT NULL,
    member_id BIGINT NOT NULL REFERENCES pwr_core.members(member_id),
    was_active BOOLEAN NOT NULL,
    PRIMARY KEY (month_start_date, member_id)
);
```

**Tasks**

* [ ] Monthly job:

  * `was_active = TRUE` if member had any voice_sessions OR event_attendance during that month.
* [ ] This will be the raw material for:

  * Retention curves
  * Basic churn metrics (active last N months → inactive now)

---

## 5. Superset: Minimum Viable Setup for PwR

### 5.1. Superset Deployment

**Tasks**

* [ ] Deploy Superset pointing at your Postgres warehouse.
* [ ] Create a single admin user (you).
* [ ] Configure basic security, HTTPS, and authentication.

### 5.2. Register Core Datasets

In Superset, define datasets for:

1. `pwr_agg.member_voice_daily`
2. `pwr_core.events`
3. `pwr_core.event_attendance`
4. `pwr_agg.member_engagement_weekly`
5. `pwr_agg.member_monthly_status`
6. `pwr_core.members` (read-only dimension dataset)

Ensure each dataset:

* Has good column labels / descriptions.
* Uses appropriate temporal column for time-series charts.

### 5.3. Build 3–4 Core Dashboards

Keep it **tight, opinionated, and actually useful**:

1. **Voice Activity Overview**

   * Total voice hours per day/week
   * Top members by voice time
   * Heatmap by hour-of-day x day-of-week

2. **Event Performance**

   * Events over time
   * Attendance per event
   * On-time vs late arrivals
   * Average time spent per attendee

3. **Member Engagement**

   * Weekly engagement per member (voice seconds + events attended)
   * Distribution of engagement levels
   * Identify “rising” and “falling” engagement

4. **Early Retention Snapshot**

   * Active vs inactive by month
   * New members this month vs previous months
   * Simple retention for cohorts (by join month, using `member_monthly_status`)

---

## 6. Acceptance Criteria for Phase 1 Completion

You can call Phase 1 “done” when:

* [ ] Every voice join/leave is stored in `pwr_raw.discord_voice_events`.
* [ ] Every member who appears in logs is represented in `pwr_core.members`.
* [ ] `pwr_core.voice_sessions` is correctly reconstructing sessions, with sane durations.
* [ ] `pwr_core.events` is populated for all “real” PwR events you care about.
* [ ] `pwr_core.event_attendance` shows who actually attended which events, with durations.
* [ ] `pwr_agg.member_voice_daily`, `member_engagement_weekly`, and `member_monthly_status` are populated regularly.
* [ ] Superset has at least:

  * 3–4 core dashboards
  * You can answer from Superset alone:

    * “Who are our most engaged members right now?”
    * “Which events are most successful?”
    * “How is overall engagement trending over time?”

Once you’re at that point, **multi-tenancy and customer-facing stuff become “just” packaging and isolation**, not foundational data-model work.

---

If you want, I can next:

* Propose **specific SQL for the ETL jobs** (sessions, attendance, engagement).
* Or design the **Superset datasets + example charts** in more concrete detail (e.g., “here’s how to build the voice activity heatmap”).

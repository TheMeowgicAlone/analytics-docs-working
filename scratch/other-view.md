# Member & Event Insights: High-Level Requirements

## 1. Purpose

Design and implement an analytics capability for the Discord community that:

* Measures health of the *overall community* (behavioral data across the server).
* Measures *event performance* (attendance, churn, popularity).
* Measures *platform/bot usage* (especially profile commands).
* Creates a foundation for future segmentation and “member archetype” insights. 

This doc is meant to guide engineering on what to build, not how to implement it.

---

## 2. Scope

System should cover:

1. **Members**

   * Join/leave behavior.
   * Activity in voice and events.
   * Usage of key bot commands.
2. **Events**

   * Creation, scheduling, and completion.
   * Attendance and drop-off.
3. **Platform / Bot**

   * Command usage, especially profile-related commands.
   * Basic reliability metrics (success/failure of logging).

Out of scope for now: ML-based recommendations, complex predictive models.

---

## 3. Core Data Model Requirements

Engineering should ensure we can persist, query, and aggregate at least the following entities.

### 3.1 Member / Discord User

A canonical “Member” record, keyed by Discord user ID, with:

* Discord user ID (primary key).
* Join timestamp (server).
* Leave / kick / ban timestamps (if applicable).
* Optional mappings (e.g., Steam/Dota/other platform IDs).
* Flags for:

  * Is current member (vs alumni).
  * Roles / tiers (e.g., staff, host, volunteer, regular member).

### 3.2 Member Activity Session

We need to distinguish *presence* from mere membership.

* Session type: `voice`, `event_voice`, `text_only` (if tracked).
* Start / end timestamps.
* Duration (derived).
* Event association (if part of a scheduled event).
* Voice channel ID(s) used.

Input sources:

* Discord voice state updates (join/leave).
* Any existing voice logs (already implemented).

Goal: We must be able to answer “Did this member join a voice call this week?” reliably.

### 3.3 Event

A structured “Event” entity for all organized activities:

* Event ID (internal).
* Discord message/ID link if created via bot.
* Title, type (e.g., inhouse, meeting, one-off), and host(s).
* Scheduled start / end time.
* Status: planned, completed, cancelled.

### 3.4 Event Attendance

Separate attendance facts:

* Event ID.
* Member ID.
* RSVP status (if available): interested/going/not responded.
* Actual attendance:

  * Joined? (boolean)
  * Join timestamp.
  * Leave timestamp.
  * Total time attended.

We should be able to:

* Aggregate attendance per event.
* Track repeat attendance for a member across events.
* Determine drop-off (members who stop attending over time).

### 3.5 Command Invocation

We need a general model for command usage, with explicit focus on:

* `/show_profile`
* `/show_my_profile`

Fields:

* Command name.
* Subcommand/options (serialized).
* Member ID (invoker).
* Timestamp.
* Outcome: success, failure, error type.
* Latency (optional but preferred).

This should be generic enough to extend to other high-value commands later.

---

## 4. Instrumentation Requirements

### 4.1 Discord Bot

The bot must emit events (either directly to the DB or via a queue/log pipeline) for:

1. **Member lifecycle**

   * Member joins server.
   * Member leaves / is removed.

2. **Voice presence**

   * Voice join / leave events consolidated into sessions.
   * Clearly mark whether a session is associated with a scheduled event.

3. **Event lifecycle**

   * Event created / updated / cancelled.
   * Event started / ended.

4. **Attendance**

   * First join of an event (per member).
   * Last leave of an event (per member).
   * Optional: mark “no-shows” if RSVP exists.

5. **Command usage**

   * Log every invocation of `/show_profile` and `/show_my_profile`.
   * Extendable to other commands if needed.

### 4.2 Data Durability & Idempotency

* Logging must be at-least-once; duplicates should be safely handled at ingestion.
* All logged events must be replayable into the warehouse (append-only raw log + transformed tables).

---

## 5. KPI Framework

We want KPIs at three layers:

1. **Community health KPIs** (server-wide behavior).
2. **Event performance KPIs**.
3. **Platform/bot usage KPIs**.
4. **Data quality KPIs** (to ensure insights are trustworthy). 

Formulas can be defined later; for now we need the *ability* to compute them.

### 5.1 Community Health KPIs

Engineering must support computation of:

1. **Weekly Active Members (WAM)**

   * Number and % of members who had at least one qualifying activity in the last 7 days.
   * Qualifying activity: joined voice, attended an event, or ran a tracked bot command.

2. **Voice Activity Rate**

   * % of members who joined at least one voice channel per week.
   * Average voice session duration per member per week.

3. **Inactivity / Attrition Rate**

   * Members who were active in the previous period but had **no** activity in the current period.
   * Separate:

     * *Soft inactivity*: still in server but behaviorally inactive.
     * *Hard churn*: left server / kicked / banned.

4. **Member Lifecycle Segments**
   Using contextual categories from the whiteboard: fresh/new/core, alumni, etc. 
   Engineering must make it possible to classify members into segments such as:

   * **Fresh**: joined within last N days.
   * **New**: joined >N days ago but limited activity.
   * **Core**: high frequency of attendance/voice sessions.
   * **Alumni**: no activity for N days or left the server.

   Exact thresholds can be tuned later; data must support flexible cohorting.

5. **Member Retention / Churn**

   * Cohort-based retention (e.g., “% of members still active 4, 8, 12 weeks after joining”).
   * Overall monthly churn rate (hard + soft defined separately).

### 5.2 Event Performance KPIs

From the “Events – Insights” section of the whiteboard: attendance, churn, popularity. 

Engineering must support:

1. **Event Attendance Rate**

   * For each event:

     * Unique attendees.
     * Attendance vs invitee list or server size.

2. **Event Churn**

   * For recurring events:

     * % of last event’s attendees who attend the next one.
     * % of regulars who drop off after N events.

3. **Event Popularity**

   * Average and peak attendance per event.
   * Waitlist / overflow (if implemented later).
   * Attendance by event type (to compare formats).

4. **Time-in-Event**

   * Average time each attendee spends in event voice channels.

5. **Host/Organizer Load**

   * Events per host per period.
   * Total attendee-hours per host.

### 5.3 Platform / Bot Usage KPIs

Based on “Platform Usage Insights – Bot CLI usage” from the whiteboard. 

1. **Profile Command Adoption**

   * % of active members who ran `/show_profile` or `/show_my_profile` in a given period.
   * Number of invocations per day/week.
   * Distribution of unique users vs repeat users.

2. **Command Usage Penetration**

   * For key commands (starting with the profile commands), track:

     * Unique users.
     * Total invocations.
     * Errors / failed invocations.

3. **Bot Engagement Funnel**

   * New member → first command usage → first event attendance.
   * Engineering must make linking these steps possible at the member level.

### 5.4 Data Quality & Operational KPIs

Reflecting the “Data Insights” section of the whiteboard (status of members by tier, captain insights, data quality segment). 

We need the ability to compute:

1. **ID Coverage**

   * % of active members with mapped external accounts (e.g., Steam, Dota).
2. **Event Data Completeness**

   * % of events with:

     * Proper start/end timestamps.
     * Attendance records.
3. **Member Classification Coverage**

   * % of members successfully classified into lifecycle segments.

---

## 6. Reporting & Access Requirements

1. **Data Store**

   * Metrics must be queryable from the analytics DB (Postgres or warehouse).
   * Tables should support cohort analysis (by join date, event type, etc.).

2. **Dashboards**

   * Provide stable views suitable for:

     * Leadership overview (community health).
     * Event organizers (event performance).
     * Engineering (bot usage, logging health).

3. **Granularity**

   * Daily grain for raw events.
   * Aggregated weekly and monthly views for most KPIs.

---

## 7. Non-Functional Requirements

* **Performance**

  * Event logging must not materially impact bot responsiveness.
* **Reliability**

  * Degraded analytics collection must not break core bot functionality.
* **Privacy**

  * All analytics must respect Discord ToS and internal privacy guidelines.
  * PII minimized; use IDs wherever possible.
* **Extensibility**

  * Easy to add new commands, event types, or platforms without redesigning schema.

---

## 8. Open Design Questions (for follow-up)

Engineering + product/analytics should explicitly design:

1. Exact thresholds for lifecycle segments (fresh/new/core/alumni).
2. Definition of “qualifying activity” for activity/retention metrics.
3. How RSVP/intention is captured (if at all) for better attendance KPIs.
4. Preferred pipeline pattern (direct DB writes vs queue + ETL).

---

This requirements list should be enough for your engineering lead to:

* Design the tracking schema and ingestion approach.
* Confirm feasibility of the proposed KPIs.
* Identify any additional data they need from the bot or other systems.


Here’s a concise stakeholder-level roadmap pulled out of your brainstorm, organized into phases and split by what’s currently unblocked vs blocked. 

---

# PwR Insights Roadmap (Stakeholder Overview)

## Cross-cutting Concept: Member Context & Cohorts

Before and across all phases we standardize “who is who” in the club so every dashboard speaks the same language:

* **Member context labels** (initially simple, can evolve):

  * Fresh member (first ~14 days)
  * New member (not yet promoted / fully onboarded)
  * Core Dota member (frequent Dota activity / many events)
  * Alumni / inactive (no meaningful activity for a threshold window)
* **Unified identity** between:

  * Discord user
  * Steam / Dota account(s)
  * Club membership status

These labels become the backbone for cohort views later (both Dota and Discord-only).

---
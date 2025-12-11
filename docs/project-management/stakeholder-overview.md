## Phase 1 – Dota Program Insights

**Goal:** Start with intuitive, game-domain analytics. It's intuitive, simpler, and will allow for learning while still creating positive value.

### Scope

0. **Workflow Establishment**
   * At the meta level, we need to nail down initial style guidelines, communication methods, tooling, etc.

1. **Core Dota Participation**

   * Matches played by member, by league/“ticket,” (match-type)
   * Time-based views (last 14 days, 30 days, season-to-date).

2. **Winrate & Performance Analytics**

   * Overall winrates per member, per team, per captain.
   * Winrates by ticket/league, by lineup, by side/position where available.
   * Outlier detection

3. **Captain Insights**

   * Overview to assist promotion team in selecting skilled captains.
   * Consider/re-evaluate methods for captain evaluation.
   * Simple captain comparison dashboards.

4. **League Health and Insights**

   * Descriptive statistics analyzing winrate deviation.
   * Matches played per ticket in the last month.
   * Retired tickets should be at zero.

5. **Steam / Account Health**

   * Unaccounted for steam accounts which need to be tracked down.
   * Smurf tracker (listing member and multiple accounts).

6. **Planning: Select necessary data for Python Engineers to complete for Phase 2**
   * Review overall design for phase 2.
   * Determine what data processing must be done.
   * Python Engineer will implement the processing.
   * Information such as discord roles, etc, are all able to be provided.
   * Ensure background data required for segmentation is available.


**Outputs (high-level):**

* A **Dota Players** dashboard, tracking participation, winrates, trends
* A **Captain Selection** dashboard, enabling promotion team to select captains.
* A **League Health** dashboard, plotting players in various methods to determine potential cheaters.
* A **An Initial Database Health** dashboard, highlighting missing information & todos for moderation/engineers.
* A **PRD-Lite** document, detailing what exact data is needed and any constraints, this will be deployed in time for the beginning of Phase 2.

---

## Phase 2 – Discord-Only Analytics & Behavioral Cohorts

**Goal:** Use Discord activity alone to understand community health, segment members, and analyze overall community churn/retention.

### Scope

1. **Discord Activity Foundations**

   * Voice presence and sessions (time in voice, by member and channel).
   * Text activity: messages by channel, thread, time-of-day.
   * Simple engagement metrics:
     * Active days, voice hours, channel diversity.

2. **Behavior-Based Cohorts (Discord-only)**

   * Cohorts defined purely from server behavior, ie no dota/event connections yet.
     * Fresh member (first ~14 days)
     * Inactive Member (never became core, went inactive)
     * New member (not yet promoted to Club Member)
     * Club Member (was promoted, meets no other criteria)
     * Contributor (contributor rank)
     * Staff (staff rank) 
     * Former Core Member
    * Future Cohorts (included for completeness)
     * Core Dota member (frequent Dota activity / many events)
     * Dota Regular (sustained attendance of a particular event)

3. **Discord Engagement Insights**

   * Heatmaps of when the server is alive (day/time).
   * Member engagement trends:

     * Who is ramping up vs cooling off.
     * Which cohorts are growing/shrinking.

   * Early retention views:

     * Fresh members who “convert” into core cohorts.
     * Members whose activity has fallen off.

4. **Pre-Churn Signals (Discord-only)**

   * Simple signals like:

     * Drop in active days / voice hours compared to personal baseline.
     * Members who haven’t been seen in N days.
   * No formal model yet; focus on surfacing candidate “at-risk” lists.

5. **Funnel Rates**

   * Long term rates of:
     * Going active
     * Core members going inactive
     * 

     * Drop in active days / voice hours compared to personal baseline.
     * Members who haven’t been seen in N days.
   * No formal model yet; focus on surfacing candidate “at-risk” lists.


5. **Event Planning Insights**

   * What time of day/day of week/etc that members are most active.
   * Which members are most active, when (ie segregate by cohort).

**Outputs (high-level):**

* A **Discord Activity & Health** dashboard.
* A **Event Planning** dashboard.
* A **Promotion Team Quick-view** dashboard, listing fresh members and recently inactive members.
* A **Behavioral Cohorts** dashboard,
* A **Risk / Inactivity Watchlist** view based on simple heuristics.

---

## Phase 3 – Club Event Insights (Blocked on `club_events` / event system)

**Status:** Concept defined, implementation *blocked* until event system is ready.

**Future Scope (once unblocked):**

* Attendance and participation per event.
* Event popularity (by type, time, host).
* Member behavior around events (joining vs dropping off after events).
* Early relationship between event participation and churn/retention.

---

## Phase 4 – Platform Usage & Tooling Analytics (Blocked for now)

**Status:** Concept defined, *blocked* until you export the right data (bot usage, Superset usage, etc.).

**Future Scope (once unblocked):**

* Bot usage analytics:

  * Which commands are used, by whom, and how often.
* Portal / dashboard usage:

  * Which dashboards and filters stakeholders actually use.
* Feedback loop:

  * Use this to prioritize which insights to refine or automate.

---

This should give you a clear, phased story to share with stakeholders and the student:
start with **Dota analytics (Phase 1)**, layer in **Discord-only behavioral insights (Phase 2)**, and keep **events and platform analytics** clearly marked as future phases that are conceptually defined but currently blocked.

# Stakeholder Overview

Phased roadmap for SFG analytics workstreams. Use this to align stakeholders on current focus, future phases, and dependencies.

## Phase 1 - Dota Program Insights

**Goal:** Start with intuitive, game-domain analytics. It's simpler and allows learning while still creating positive value.

### Scope

1. **Workflow establishment:** Nail down initial style guidelines, communication methods, and tooling.
2. **Core Dota participation:**
   - Matches played by member and by league/ticket (match type).
   - Time-based views (last 14 days, 30 days, season-to-date).
3. **Win rate and performance analytics:**
   - Overall win rates per member, per team, and per captain.
   - Win rates by ticket/league, lineup, and side/position where available.
   - Outlier detection.
4. **Captain insights:**
   - Overview to assist the promotion team in selecting skilled captains.
   - Re-evaluate methods for captain evaluation.
   - Simple captain comparison dashboards.
5. **League health and insights:**
   - Descriptive statistics analyzing win rate deviation.
   - Matches played per ticket in the last month; retired tickets should be zero.
6. **Steam / account health:**
   - Unaccounted-for Steam accounts that need to be tracked down.
   - Smurf tracker listing members and multiple accounts.
7. **Planning for Phase 2 data needs:**
   - Review overall design for Phase 2 and identify required data processing.
   - Determine what data processing must be done; Python engineers will implement it.
   - Confirm supporting information (Discord roles, etc.) is available.
   - Ensure background data required for segmentation is available.

### Outputs

- A **Dota Players** dashboard tracking participation, win rates, and trends.
- A **Captain Selection** dashboard enabling the promotion team to select captains.
- A **League Health** dashboard plotting players in various methods to determine potential cheaters.
- An **Initial Database Health** dashboard highlighting missing information and todos for moderation/engineers.
- A **PRD-Lite** document detailing required data and constraints, ready before Phase 2.

---

## Phase 2 - Discord-Only Analytics & Behavioral Cohorts

**Goal:** Use Discord activity alone to understand community health, segment members, and analyze overall community churn/retention.

### Scope

1. **Discord activity foundations:**
   - Voice presence and sessions (time in voice by member and channel).
   - Text activity: messages by channel, thread, and time of day.
   - Engagement baselines: active days, voice hours, channel diversity.
2. **Behavior-based cohorts (Discord-only):**
   - Cohorts defined purely from server behavior (no Dota/event connections yet):
     - Fresh member (first ~14 days).
     - Inactive member (never became core, went inactive).
     - New member (not yet promoted to Club Member).
     - Club Member (promoted; meets no other criteria).
     - Contributor (contributor rank).
     - Staff (staff rank).
     - Former Core Member.
   - Future cohorts (included for completeness):
     - Core Dota member (frequent Dota activity / many events).
     - Dota Regular (sustained attendance of a particular event).
3. **Discord engagement insights:**
   - Heatmaps of when the server is alive (day/time).
   - Early retention views:
     - Fresh members who convert into core cohorts.
     - Members whose activity has fallen off.
4. **Pre-churn signals (Discord-only):**
   - Drops in active days or voice hours relative to personal baselines.
   - Members unseen for N days; highlight candidate at-risk lists (no formal model yet).
   - Special highlights on staff/contributors.
5. **Funnel rates:**
   - Long-term rates for going active, graduating to Club/Contributor, and lapsing from core segments.
6. **Event planning insights:**
   - Time-of-day/day-of-week activity patterns.
   - Most active members by cohort and time window to guide scheduling.

### Outputs

- A **Discord Activity & Health** dashboard.
- An **Event Planning** dashboard.
- A **Promotion Team Quick-view** dashboard listing fresh members and recently inactive members.
- A **Behavioral Cohorts** dashboard.
- A **Risk / Inactivity Watchlist** view based on simple heuristics.
- A **Contributor / Staff Engagement** dashboard monitoring staff members whose interest is waning.

---

## Phase 3 - Club Event Insights (Blocked on `club_events` / event system)

**Status:** Concept defined; implementation is blocked until the event system is ready.

**Future scope (once unblocked):**

- Attendance and participation per event.
- Event popularity (by type, time, host).
- Member behavior around events (joining vs dropping off after events).
- Early relationship between event participation and churn/retention.

---

## Phase 4 - Platform Usage & Tooling Analytics (Blocked for now)

**Status:** Concept defined; blocked until the right data is exported (bot usage, Superset usage, etc.).

**Future scope (once unblocked):**

- Bot usage analytics:
  - Which commands are used, by whom, and how often.
- Portal / dashboard usage:
  - Which dashboards and filters stakeholders actually use.
- Feedback loop:
  - Use this to prioritize which insights to refine or automate.

---

This should give you a clear, phased story to share with stakeholders and the student: start with **Dota analytics (Phase 1)**, layer in **Discord-only behavioral insights (Phase 2)**, and keep **events and platform analytics** clearly marked as future phases that are conceptually defined but currently blocked.

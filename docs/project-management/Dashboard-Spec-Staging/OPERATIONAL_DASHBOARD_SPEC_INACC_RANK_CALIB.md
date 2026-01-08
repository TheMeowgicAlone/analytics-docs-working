# Set to New File name: Operational Dashboard Spec — <Slice Name>

## 1) Deliverable (What This Slice Produces)

The dashboard shows an overall view of a player level performance, indicated by winrate.

---

## 2) Source of Truth (Do Not Re-define Here)
- Decision Brief: https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/decision-briefs/inaccurate-account-calibration.md
- Signal Design / Metric Definitions: https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/Decision/SIGNAL_DESIGNS.md
- Glossary (optional): -

**This document is the final build blueprint:** layout, default state, interaction model, caveats/guardrails, and acceptance criteria.

---

## 3) Scope
**In scope:** <what this dashboard slice covers operationally>  
This dashboard covers the initial signals of an inaccurate player rank calibration
**Out of scope (Non-goals):** <explicit exclusions to prevent scope creep>
The dashboard only covers winrate based signals, other indicators of player performance breakdown will be included in a future dashboard
---

## 4) Unit of Analysis (Grain)
**One row/item represents:**
Dot - Player
Filters/Segments : event(ticket), role, 
 (account / player / match / case / event / member)  
**Primary time windows supported:**
Initial pass: lifetime, 6months for segments 
 <e.g., 7d / 30d / 60d / season>  
**Eligibility rule (data sufficiency):** <minimum sample size or completeness required to interpret>
At least 30 games played in lifetime, further segments require 10 games played minimum
---

## 5) Acceptance Criteria (Dashboard-Level)
Define what “good” looks like for the dashboard artifact (not the business decision):
The dashboard should show a clear indicator of outlier players based on winrate.

- **Time-to-triage:** 
reviewer can identify potential entities in below 5mins
- **Self-sufficiency:** 
A reviewer can process entities through the dashboard and ascertain whether the entity should be flagged for further in-depth review
- **Clarity:** 
The full chart is available, with the filtered chart also available for a quicker pass. 
- **Correctness safeguards:** insufficient-data / not-eligible states are explicit and cannot be mistaken for “normal”
Time windows currently not enforced in all charts, can be introduced if needed  
Segmented winrates are still assessed with a 10 game minimum limit, and 30 overall game total minimum requirement. 
-Players not meeting requirements are not recorded on the dashboard. 
---

## 6) Dashboard Pattern (Choose One)
Default for operational workflows is **Queue → Details**.

- [ ] **Queue → Details** (ranked entities; click into evidence)
- [x] **Trend → Breakdown** (time trend then segmentation)
- [ ] **Funnel** (stage progression and drop-offs)
- [ ] **Cohort Comparison** (A vs B, before/after)


**Why this pattern fits the workflow:** <1–2 sentences, no metrics>
The usage of the dashboard is to spot outliers, then go into details to determine course of action
---

## 7) Information Architecture (Sections = Jobs-to-be-Done)
Define sections by what the user is trying to do, not by chart type.
Section 1: The user checks the dashboard section to spot extreme winrates with reference to games played.
Section 1.3: The user is able to check the dashboard for more info on the likely inaccurately calibrated players (60% winrate and higher), also with their corresponding rank brackets and winrates.
Section 2.1: The user is able to spot higher potential inaccurately calibrated players, with high overall winrates that accounts for role preference.
Section 2.2: Chart similar to 1.3, no filter

### Section A — Queue / Triage
**Job-to-be-done:** “Who should we look at first?”  
To identify inaccuracy in rank calibration, we can look at players with a higher than average winrate(assuming balanced shuffle) over a given period of time
(above 60%)
**Required outputs (must display):**
- <entity identifier(s)>
  Player names
- <priority score or ranking field>
  NA for initial pass, for detailed breakdown ranking score
- <recommended next step label (if applicable)>
  Identified players to be checked on the filtered chart/detailed breakdown
- <data sufficiency indicator (sample size / eligibility)>
  Ideally 30 games played total for accuracy, 10 games for minimum eligibility in segmented data
- <top 1–3 evidence highlights (short)>
  Cumulative winrate above 1.6 total, above 60% winrate by rank lobby exceeding their calibrated rank
**Primary interaction:**   
Step 1. Identifying potential inaccurate rank calibration candidates by hovering over the plots that are outliers on the charts
Step 2. Selecting player's name on the segmented charts to view their winrate breakdowns
Step 3. Confirming attributed winrate to known factors, make decision whether to flag the player for detailed review.
Additional steps: 
Review detailed breakdown for overperforming players(i.e:overperforming in 2 roles, underperforming in 1), and flag them for review  
**Optional:** export, copy link, quick filters (keep minimal).

---

### Section B — Evidence Panel (Entity Detail)
**Job-to-be-done:** “Why is this entity prioritized, and what’s the evidence?”  
60% winrate set as baseline winrate flag for further scrutiny, evidence for inaccurate rank calibration in winrate breakdowns
**Required outputs (must display):**
- Context fields needed to interpret metrics (always visible): <list>
  50% winrate is expected, 5% deviation is normal due to small sample size, above 60% winrate is uncommon at larger sample sizes 
- **Diagnostic Cards** 
  Sample size(30) 
  time window (6 months)
  45-55% winrate expected, ~1.25-1.5 winrate cumulative expected


**Optional:** example pointers / drill links (match IDs, logs) only if they reduce review time.

---

### Section C — Drilldown / Raw Records (Optional)
**Job-to-be-done:** “Show me the underlying records quickly.”  
**Required outputs:** <table of events/matches/logs, etc.>  
Include only if it materially reduces debate or back-and-forth.

---

## 8) Diagnostic Card Specification (Reusable Contract)
Each card is a standardized unit of evidence. Define 3–6 cards.
Do not re-define the metric; reference the Signal Design.

### Diagnostic Card <#> — <Card Title>
- **Metric / Signal reference:** <exact name from Signal Design>
- **Question it answers:** <plain-language>
- **Required segmentation (dimensions):** <role / bracket / cohort / etc.>
- **Required comparison/baseline:** <expected value, prior period, peer group, etc.>
- **Display form:** <single value / mini trend / distribution / breakdown>
- **Decision states (interpretation rules):**
  - **High concern:** <condition in words>
  - **Normal:** <condition in words>
  - **Inconclusive / insufficient data:** <condition in words>
- **Common confounder (1 line):** <typical failure mode or caveat>
- **Link-out (optional):** <raw examples / records>

*(Repeat for each card.)*

---

## 9) Default State (Make It Useful on Open)
Defaults should match the most common operational review session.

- **Default time window:** <e.g., last 60d>
- **Default filters:** <minimal set>
- **Eligibility gating:** <what appears by default and why>
- **Default sort / ranking:** <field + direction + short rationale>
- **Default list size:** <e.g., top 50>
- **Empty and low-data behavior:** <what the user sees and how to interpret it>

---

## 10) Interaction Model (Keep It Simple)
**Primary navigation:** Queue row → Evidence Panel  
**Secondary interactions (optional):**
- filter chips / dropdowns (minimal)
- tooltips linking to glossary definitions
- export/share link
Avoid multi-step flows; this is a dashboard, not an application workflow.

---

## 11) Data Requirements (Operational Reality)
- **Data sources:** <tables/views/events>
- **Refresh frequency:** <hourly/daily/near-real-time>
- **Expected latency:** <e.g., up to 24h behind>
- **Data quality checks (must pass to trust outputs):**
  - <check 1>
  - <check 2>
- **Degraded mode:** <what happens if checks fail (banner, disable ranking, etc.)>
https://github.com/saltfreegaming/analytics-docs/pulse
---

## 12) Caveats & Guardrails (Prevent Misinterpretation)
- **Not valid for:**  
  - <1–3 bullets: conclusions that should not be drawn>
- **Best used for:**  
  - <1–3 bullets: intended use>
- **Human review required when:**  
  - <1–3 bullets: where judgment is needed>
- **Mandatory UI cues:**  
  - sample size always visible  
  - eligibility / insufficient-data states explicit  
  - baseline/expected value shown when relevant

---

## 13) Build Readiness Gate (Last Stop Before Implementation)
This slice can move into dashboard construction only when:

**Must-have for v1:**
- Sections A and B are fully specified (required outputs + interactions)
- All Diagnostic Cards are specified (states + required dimensions + baseline + insufficient-data behavior)
- Default state is explicit (window, filters, sort, list size)
- Data sources and freshness expectations are defined
- Caveats/guardrails are written and mapped to UI cues

**Nice-to-have (v2):**
- <1–5 bullets>

**Dependencies / instrumentation gaps:**
- <missing fields, backfills, logging needs>

**Open questions (max 5):**
- <threshold/baseline unknowns, missing dimension, unresolved definition>

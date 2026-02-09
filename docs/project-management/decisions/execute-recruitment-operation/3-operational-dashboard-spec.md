# Operational Dashboard Spec: Execute Recruitment Operation

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
This dashboard covers the health of the server's intake of new members, and whether a recruitment operation is needed
**Out of scope (Non-goals):** <explicit exclusions to prevent scope creep>
The dashboard only covers server intake, and while close, conversion will not be included
---

## 4) Unit of Analysis (Grain)
**One row/item represents:**
One point - day's total/ week's total
Filters/Segments : New members/ Fresh members(Subset)
**Primary time windows supported:**
Initial pass: 3 Months 
**Eligibility rule (data sufficiency):** <minimum sample size or completeness required to interpret>
NA
---

## 5) Acceptance Criteria (Dashboard-Level)
Define what “good” looks like for the dashboard artifact (not the business decision):
The dashboard should show a clear representation of player intake, showing trends and history

- **Time-to-triage:** 
Reviewer can review if a flag should be raised with one look at the dashboard
- **Self-sufficiency:** 
Dashboard should update automatically for the last 3 months of data
- **Clarity:** 
The full chart is available in either grain if needed
- **Correctness safeguards:** insufficient-data / not-eligible states are explicit and cannot be mistaken for “normal”
Chart is based on raw data, not applicable
---

## 6) Dashboard Pattern (Choose One)
Default for operational workflows is **Queue → Details**.

- [ ] **Queue → Details** (ranked entities; click into evidence)
- [x] **Trend → Breakdown** (time trend then segmentation)
- [ ] **Funnel** (stage progression and drop-offs)
- [ ] **Cohort Comparison** (A vs B, before/after)


**Why this pattern fits the workflow:** <1–2 sentences, no metrics>
The usage of the dashboard is to take note of intake health, only the trend is required 

---

## 7) Information Architecture (Sections = Jobs-to-be-Done)
Define sections by what the user is trying to do, not by chart type.
Section 1:
Checking history of member intake

### Section A — Queue / Triage
**Job-to-be-done:** “Is a recruitment operation required?”  
To identify whether a recruitment operation is needed, check trends and health of intake, if the overall health is on a decline, prepare for recruitment operation if not addressed or changed
**Required outputs (must display):**
- <entity identifier(s)>
  Date range/stamp
**Primary interaction:**   
Step 1. Identifying intake health by hovering section if needed
Step 2. Intake healthy > do nothing, intake unhealthy> flag


---

## 8) Diagnostic Card Specification (Reusable Contract)
Each card is a standardized unit of evidence. 
Intake health

### Diagnostic Card <#> — Intake health
- **Metric / Signal reference:** Intake Health
- **Question it answers:** Do we need to do a recruitment operation soon?
- **Required segmentation (dimensions):** New members segments only
- **Required comparison/baseline:** To be decided- currently using previous trends 
- **Display form:**  History of new member intake- line/bar graph
- **Decision states (interpretation rules):** 
  - **High concern:** Intake has been below 5 in the past 3 weeks
  - **Low Concern:** Intake has been below 5 in past week
  - **Normal:** Intake healthy- at least 5member per week

---

## 9) Default State (Make It Useful on Open)
Defaults should match the most common operational review session.

- **Default time window:**  3 months
- **Default filters:** Not applicable 
- **Eligibility gating:** New members only 
- **Default sort / ranking:** Not applicable
- **Default list size:** Eligible members
- **Empty and low-data behavior:**
 Empty data-no plots for date range

---

## 10) Interaction Model (Keep It Simple)
**Primary navigation:** Not applicable
---

## 11) Data Requirements (Operational Reality)
- **Data sources:** Discord API data
- **Refresh frequency:** daily
- **Expected latency:** daily
- **Data quality checks (must pass to trust outputs):**
  - Check if output is correct corresponding to discord
- **Degraded mode:** <what happens if checks fail (banner, disable ranking, etc.)>
Do not use if degraded
https://github.com/saltfreegaming/analytics-docs/pulse

---

## 12) Caveats & Guardrails (Prevent Misinterpretation)
- **Not valid for:**  
  Examining members in specifics
- **Best used for:**  
  -Intended usage is as a early warning for unhealthy server growth

- **Human review required when:**  
  -Server growth shows 0 for a week



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

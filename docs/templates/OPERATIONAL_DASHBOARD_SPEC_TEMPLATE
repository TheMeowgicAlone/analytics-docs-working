# Set to New File name: Operational Dashboard Spec — <Slice Name>

## 1) Deliverable (What This Slice Produces)
One sentence describing the concrete output (e.g., “a ranked review queue plus an entity-level evidence panel”).

---

## 2) Source of Truth (Do Not Re-define Here)
- Decision Brief: <link/path>
- Signal Design / Metric Definitions: <link/path>
- Glossary (optional): <link/path>

**This document is the final build blueprint:** layout, default state, interaction model, caveats/guardrails, and acceptance criteria.

---

## 3) Scope
**In scope:** <what this dashboard slice covers operationally>  
**Out of scope (Non-goals):** <explicit exclusions to prevent scope creep>

---

## 4) Unit of Analysis (Grain)
**One row/item represents:** <entity> (account / player / match / case / event / member)  
**Primary time windows supported:** <e.g., 7d / 30d / 60d / season>  
**Eligibility rule (data sufficiency):** <minimum sample size or completeness required to interpret>

---

## 5) Acceptance Criteria (Dashboard-Level)
Define what “good” looks like for the dashboard artifact (not the business decision).

- **Time-to-triage:** <e.g., reviewer can process top 25 entities in ≤ 30 minutes>
- **Self-sufficiency:** <e.g., ≥ 90% of entities can reach a provisional outcome using this dashboard alone>
- **Clarity:** sample size + time window are always visible where interpretation depends on them
- **Correctness safeguards:** insufficient-data / not-eligible states are explicit and cannot be mistaken for “normal”

---

## 6) Dashboard Pattern (Choose One)
Default for operational workflows is **Queue → Details**.

- [ ] **Queue → Details** (ranked entities; click into evidence)
- [ ] **Trend → Breakdown** (time trend then segmentation)
- [ ] **Funnel** (stage progression and drop-offs)
- [ ] **Cohort Comparison** (A vs B, before/after)

**Why this pattern fits the workflow:** <1–2 sentences, no metrics>

---

## 7) Information Architecture (Sections = Jobs-to-be-Done)
Define sections by what the user is trying to do, not by chart type.

### Section A — Queue / Triage
**Job-to-be-done:** “Who should we look at first?”  
**Required outputs (must display):**
- <entity identifier(s)>
- <priority score or ranking field>
- <recommended next step label (if applicable)>
- <data sufficiency indicator (sample size / eligibility)>
- <top 1–3 evidence highlights (short)>

**Primary interaction:** selecting an entity opens Section B focused on it.  
**Optional:** export, copy link, quick filters (keep minimal).

---

### Section B — Evidence Panel (Entity Detail)
**Job-to-be-done:** “Why is this entity prioritized, and what’s the evidence?”  
**Required outputs (must display):**
- Context fields needed to interpret metrics (always visible): <list>
- **Diagnostic Cards** (3–6) fed by the Signal Design
- Sample size + time window (prominent)
- Baseline/expected value or comparison (when relevant)
- Explicit “insufficient data / not eligible” state (not hidden)

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

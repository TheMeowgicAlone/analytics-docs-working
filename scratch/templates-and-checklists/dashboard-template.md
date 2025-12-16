# Dashboard Documentation Template

Use this template for every Apache Superset dashboard. Copy the sections and fill them out in a dedicated page under `docs/platform/` or a subfolder.

---

## Overview

- **Dashboard name**: Include Draft if necessary, see naming conventions in style.
- **Owner**:
- **Audience**:
- **Iteration status**: backlog / in-progress / needs-testing / needs-approval / approved
- **Related Grafana panels**:

---

## Purpose & Questions

- What decision does this dashboard support?
- What questions does this dashboard answer?

---

## Data Sources

- Superset dataset(s) and schemas.
- Discord Gateway API events utilized (e.g., `MESSAGE_CREATE`, `VOICE_STATE_UPDATE`).
- Refresh cadence and retention window.

---

## KPI Definitions

- KPI name, formula, and filters.
- Dimensionality (e.g., by channel, role, region).
- Acceptance criteria and validation steps.

### Example KPI Definition

- **Name**: Active Members (30d)
- **Purpose**: Track recent member engagement for moderation and community leads.
- **Formula**: Count distinct `member_id` with ≥1 message OR ≥5 voice minutes in the last 30 days.
- **Filters/Exclusions**: Exclude bot accounts and private staff channels; time zone UTC.
- **Grains/Breakdowns**: Daily trend; breakdowns by cohort (Fresh, Club, Contributor, Staff) and channel category.
- **Datasets**: `discord_activity_daily` (messages, voice_minutes), `member_cohorts`.
- **Validation**: Reconcile daily counts against Discord Gateway logs within ±2%; spot-check top 10 members against raw events.
- **Acceptance Thresholds**: Data freshness <24h; no more than 1% null `member_id` rows in source tables.

---

## Visuals & Interactions

- List charts with titles and links.
- Default filters, drill paths, and expected user flows.
- Accessibility considerations (color, labeling).

---

## Security & Permissions

- Roles with access.
- Sensitivity level (public/internal/confidential).

---

## Change Log

- Date, change description, and reviewer.

---

## Open Questions & Next Steps

- Pending data gaps or feature requests.
- Experiments or A/B tests to run in the next phase.

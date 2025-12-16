# Calibration Queue Dashboard

**Status:** Design Phase  
**Owner:** Analytics Team  
**Target Audience:** Moderation Team  
**Related Decision Brief:** [Inaccurate Account Calibration](../project-management/decision-briefs/inaccurate-account-calibration.md)

---

## Overview

Dashboard to support weekly review of player calibration accuracy and enable data-driven moderation decisions.

- **Dashboard name**: Calibration Queue Dashboard (Draft)
- **Owner**: Analytics Team
- **Audience**: Moderation Team, League Administrators
- **Iteration status**: design-phase
- **Cadence**: Weekly review (with ad-hoc nightly flag checks)

---

## Purpose & Questions

**Decision Supported:** Which accounts should enter human investigation queue for possible enforcement action?

**Questions Answered:**
1. Which players show sustained performance outliers relative to their ticket/league?
2. Which players have low verification confidence (estimated MMR, privacy flags, smurf flags)?
3. Which players show role-specific dominance patterns?
4. Which players have abnormal captain winrate deltas?
5. Which players combine multiple risk signals?

---

## Dashboard Design Workflow

This dashboard follows the SFG dashboard design process:

```
Decision Brief → Signal Catalog → KPI/Chart Definitions → Dashboard Implementation
```

### 1. Decision Brief (Complete)
[Inaccurate Account Calibration](../project-management/decision-briefs/inaccurate-account-calibration.md) defines:
- Problem statement
- Action options (A0–A5)
- Success criteria and guardrails
- High-level inputs/outputs

### 2. Signal Catalog (In Progress)
[Signal Catalog](../analytics-frameworks/signal-catalog.md) defines:
- S01: Sustained Winrate Outlier
- S02: Role/Position Skew
- S03: Economic Dominance Proxy (GPM Outlier)
- S04: Captain Delta Risk
- S05: Low Confidence / Risk Flags

**Status:** Thresholds need tuning via backtesting

### 3. KPI & Chart Definitions (This Section)
Translate signals into concrete charts and metrics for dashboard implementation.

### 4. Dashboard Implementation (Future)
Build in Superset following [Style Guidelines](../platform/superset-style-guidelines.md) and [Dashboard Template](../templates-and-checklists/dashboard-template.md).

---

## KPI Definitions

### KPI-01: Investigation Queue
**Purpose:** Ranked list of players requiring review, ordered by signal severity.

**Formula:**
```sql
-- Composite risk score from signals S01-S05
SELECT 
    discord_user_id,
    display_name,
    primary_steam_id,
    -- Signal components (TBD: finalize weights)
    s01_winrate_severity * 1.5 AS s01_score,
    s02_role_skew_severity * 1.0 AS s02_score,
    s03_gpm_severity * 1.2 AS s03_score,
    s04_captain_delta_severity * 1.0 AS s04_score,
    s05_confidence_score * 0.8 AS s05_score,
    -- Total risk score
    (s01_score + s02_score + s03_score + s04_score + s05_score) AS total_risk_score,
    -- Metadata
    games_played_60d,
    current_ticket,
    existing_flags
FROM signal_rollup_view
WHERE games_played_60d >= 15  -- Minimum sample size
ORDER BY total_risk_score DESC
LIMIT 50
```

**Filters/Exclusions:**
- Exclude players with `is_disabled = true`
- Exclude players with active investigation cases (optional)
- Time zone: UTC

**Grains/Breakdowns:**
- Per player (discord_user_id)
- Optionally grouped by ticket_id

**Datasets:** 
- `signal_rollup_view` (to be created, joins signals S01-S05)
- `dota_player_full_view`
- `official_dota_matches_view`

**Validation:**
- Spot-check top 10 flagged players against raw match data
- Review false positive rate with moderation team weekly
- Target: <20% false positive rate in top 25

**Acceptance Thresholds:**
- Queue must include sample size check (≥15 matches)
- No permanent bans based on score alone (enforced via action codes)

---

### KPI-02: Signal Breakdown
**Purpose:** Show which signals triggered for each player in queue.

**Formula:** Binary flags for each signal (0/1) plus severity level.

**Grains/Breakdowns:**
- Per player per signal
- Heatmap view: players (rows) × signals (columns)

**Datasets:** `signal_rollup_view`

**Validation:** Ensure signal logic matches [Signal Catalog](../analytics-frameworks/signal-catalog.md) definitions.

---

### KPI-03: Winrate vs. League Baseline (S01)
**Purpose:** Visualize player winrate relative to league distribution.

**Formula:**
```sql
SELECT 
    discord_user_id,
    ticket_id,
    rolling_winrate_60d,
    league_median_winrate,
    league_stddev_winrate,
    (rolling_winrate_60d - league_median_winrate) / league_stddev_winrate AS z_score,
    games_played_60d
FROM player_winrate_stats
WHERE games_played_60d >= 15
```

**Grains/Breakdowns:**
- Per player per ticket
- Trend over time (last 90 days)

**Datasets:**
- `official_dota_matches_view`
- League aggregates (median, stddev by ticket_id)

**Validation:** Verify z-score calculation against manual computation for sample players.

---

### KPI-04: Role Winrate Delta (S02)
**Purpose:** Show performance variance across roles/positions.

**Formula:**
```sql
SELECT 
    discord_user_id,
    role,
    COUNT(*) AS games_in_role,
    AVG(CASE WHEN win THEN 1.0 ELSE 0.0 END) AS winrate_in_role,
    MAX(winrate_in_role) - MIN(winrate_in_role) AS role_delta
FROM official_dota_matches_view
WHERE start_timestamp >= NOW() - INTERVAL '60 days'
GROUP BY discord_user_id, role
HAVING COUNT(*) >= 5  -- Minimum matches per role
```

**Grains/Breakdowns:**
- Per player per role
- Stacked bar chart: roles on x-axis, winrate on y-axis

**Datasets:** `official_dota_matches_view`

**Validation:** Cross-check role assignments against match replay data (sample check).

---

### KPI-05: GPM Outlier (S03)
**Purpose:** Identify economic dominance relative to position baseline.

**Formula:**
```sql
SELECT 
    discord_user_id,
    position,
    AVG(gold_per_minute) AS avg_gpm,
    -- Baseline for position in ticket
    (SELECT AVG(gold_per_minute) FROM official_dota_matches_view 
     WHERE position = p.position AND ticket_id = p.ticket_id) AS position_baseline_gpm,
    (avg_gpm - position_baseline_gpm) / STDDEV(gold_per_minute) AS gpm_z_score
FROM official_dota_matches_view p
WHERE start_timestamp >= NOW() - INTERVAL '60 days'
  AND position IN (1, 2)  -- Carry positions
GROUP BY discord_user_id, position, ticket_id
HAVING COUNT(*) >= 10
```

**Grains/Breakdowns:**
- Per player per position
- Box plot: GPM distribution by position

**Datasets:** `official_dota_matches_view`

**Validation:** Validate GPM values against OpenDota/Stratz match details for sample matches.

---

### KPI-06: Captain Winrate Delta (S04)
**Purpose:** Compare captain vs. non-captain performance.

**Formula:** Use pre-computed `captain_winrate_delta` from `dota_league_stats_view`.

**Filters/Exclusions:** Only include players with ≥10 captain games (enforced by view logic).

**Grains/Breakdowns:**
- Per player per ticket
- Scatter plot: captain winrate (x) vs. player winrate (y)

**Datasets:** `dota_league_stats_view`

**Validation:** Spot-check delta calculation against raw match counts.

---

### KPI-07: Risk Flag Summary (S05)
**Purpose:** Display account verification confidence and external risk signals.

**Formula:** Point-based scoring per [Signal Catalog S05](../analytics-frameworks/signal-catalog.md#s05--low-confidence--risk-flags).

**Grains/Breakdowns:**
- Per player
- Badge/icon view: show active flags (MMR estimated, smurf flag, privacy flags)

**Datasets:** `dota_players`

**Validation:** Confirm flag logic matches source data definitions.

---

### KPI-08: Match Sample Size
**Purpose:** Ensure all queue entries meet minimum sample size guardrail.

**Formula:** `COUNT(DISTINCT match_id)` per player in last 60 days.

**Filters/Exclusions:** Highlight players below threshold (15 matches).

**Grains/Breakdowns:** Per player

**Datasets:** `official_dota_matches_view`

**Validation:** Cross-reference counts with match logs.

---

## Visuals & Interactions

### Tab 1: Investigation Queue
**Chart Type:** Table (sortable, filterable)

**Columns:**
- Player Name (linked to profile)
- Risk Score (total)
- Signal Flags (S01-S05 badges)
- Games Played (60d)
- Current Ticket
- Recommended Action (A0-A5)
- Review Status (dropdown: pending/reviewed/actioned)

**Interactions:**
- Click player → drill to player detail view
- Filter by ticket, risk score threshold, signal type
- Sort by any column

---

### Tab 2: Signal Details
**Charts:**
1. **Signal Heatmap** (KPI-02): Players × Signals matrix
2. **Winrate Distribution** (KPI-03): Histogram with player overlay
3. **Role Winrate Breakdown** (KPI-04): Grouped bar chart per player
4. **GPM Outliers** (KPI-05): Box plot with flagged players highlighted
5. **Captain Delta Scatter** (KPI-06): Captain WR vs. Player WR
6. **Risk Flag Icons** (KPI-07): Badge grid per player

**Interactions:**
- Cross-filtering: select player in one chart → highlight in all charts
- Time range selector (30d/60d/90d)
- Ticket filter

---

### Tab 3: Evidence Detail (Drill-Down)
**Activated by:** Clicking player in queue table

**Charts:**
1. Match history table (last 30 matches)
2. Winrate trend line (rolling 10-match average)
3. Hero frequency (if available)
4. Recent action history (warnings, flags)

**Data:** Player-specific queries from `official_dota_matches_view`, `dota_players`, `command_audit`

---

## Data Sources

- **Primary Dataset:** `signal_rollup_view` (to be created as materialized view or virtual dataset)
- **Supporting Tables:**
  - `official_dota_matches_view` (match participation)
  - `dota_league_stats_view` (aggregated stats)
  - `dota_player_full_view` (player profiles)
  - `dota_players` (flags and MMR data)
  - `command_audit` (action history)
- **Refresh Cadence:** Nightly (incremental for last 90 days)
- **Retention Window:** 90 days for signal computation; full history for audit

---

## Security & Permissions

- **Roles with Access:** Moderation Team, League Administrators, Analytics Team
- **Sensitivity Level:** Internal (contains player identification and performance data)
- **Row-Level Security:** Future consideration for ticket-level permissions

---

## Implementation Checklist

- [ ] **Backtest Signals:** Run S01-S05 against historical data to tune thresholds
- [ ] **Create `signal_rollup_view`:** Materialized view combining all signals
- [ ] **Build Draft Dashboard in Superset:** Implement KPIs 01-08 as charts
- [ ] **Pilot with Moderation Team:** Shadow mode for 2 weeks (no enforcement actions)
- [ ] **Review False Positive Rate:** Target <20% in top 25 flagged players
- [ ] **Finalize Action Workflow:** Document how moderators use dashboard to take actions A1-A5
- [ ] **Production Release:** Enable nightly refresh and weekly review cadence
- [ ] **Document Dashboard:** Complete [Dashboard Template](../templates-and-checklists/dashboard-template.md) for final version

---

## Open Questions & Next Steps

### Signal Tuning
- [ ] What are optimal thresholds for each signal severity level (Low/Med/High)?
- [ ] Should signal weights in composite risk score be adjustable via dashboard parameter?
- [ ] How often should thresholds be reviewed and updated?

### Workflow Integration
- [ ] How do moderators record investigation outcomes in dashboard vs. external system?
- [ ] Should dashboard write back to `dota_players` flags (`is_inaccurate_calibration`, etc.)?
- [ ] What triggers automated notifications to moderation team (Slack/Discord)?

### Data Gaps
- [ ] Hero pick data not currently in schema—defer hero-specific analysis to Phase 2?
- [ ] Match replay data for detailed stats (K/D/A, damage)—use OpenDota API integration?

### Next Phase Enhancements
- [ ] Add "similar players" comparison view
- [ ] Trend analysis: track signal changes over multiple review periods
- [ ] Automated case creation integration with moderation system

---

## Change Log

| Date       | Change                                      | Author          |
|------------|---------------------------------------------|-----------------|
| 2025-12-12 | Initial design document created             | Analytics Team  |

---

## See Also

- [Inaccurate Account Calibration Decision Brief](../project-management/decision-briefs/inaccurate-account-calibration.md)
- [Signal Catalog](../analytics-frameworks/signal-catalog.md)
- [Dota Data Entity Taxonomy](../project-management/data-entity-taxonomy/dota-taxonomy.md)
- [Dashboard Template](../templates-and-checklists/dashboard-template.md)
- [Superset Style Guidelines](../platform/superset-style-guidelines.md)

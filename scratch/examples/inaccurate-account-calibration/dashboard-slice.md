# Dashboard Slice – Inaccurate Account Calibration

## Decision Supported
Which accounts should be prioritized for human review due to potential
inaccurate skill calibration?

---

## Dashboard Structure

### Section 1: Investigation Queue
**Table**
- Player ID
- Division
- Total Matches
- Winrate Delta
- Avg KDA Z-Score
- Flag Count

Sorted by:
1. Flag Count
2. Winrate Delta

Purpose:
- Quickly identify top review candidates

---

### Section 2: Account Performance Trends
**Line Charts**
- Winrate over time
- KDA Z-score over time

Purpose:
- Validate sustained performance patterns

---

### Section 3: Match Evidence
**Table**
- Match Date
- Role
- Impact Score
- Result

Purpose:
- Provide concrete examples for reviewer confidence

---

## Design Principles
- No single chart implies enforcement
- All metrics require context
- Emphasis on trends over absolutes

---

## Known Risks
- False positives for improving players
- Meta shifts affecting division baselines

This dashboard is intended to **support human judgment**, not replace it.
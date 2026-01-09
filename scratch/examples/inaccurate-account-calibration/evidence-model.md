# Evidence Model – Investigation Queue

## Purpose
Define what evidence a human reviewer should see when evaluating a flagged
account for inaccurate calibration.

---

## Primary Entity
**Player Account**

---

## Required Evidence Per Account

### Summary Metrics
| Metric | Description |
|------|------------|
| Total Matches (Window) | Sample size validation |
| Winrate Delta | vs division average |
| Avg KDA Z-Score | Role-adjusted |
| Flag Count | Number of thresholds exceeded |

---

### Trend Evidence
- Winrate over time (rolling window)
- KDA Z-score over time
- Match participation frequency

Purpose:
- Distinguish sustained dominance from short-term streaks

---

### Match-Level Examples
Display:
- 3–5 representative matches
- Highest impact performances
- Recent games preferred

Each match should include:
- Date
- Role
- Final score
- Performance summary

---

## Explicit Exclusions
The following should **not** be shown:
- Opponent identities
- Enforcement history
- Automated conclusions

---

## Reviewer Outcome
This evidence should allow a reviewer to:
- Decide whether to take A0–A5 actions
- Document reasoning for follow-up
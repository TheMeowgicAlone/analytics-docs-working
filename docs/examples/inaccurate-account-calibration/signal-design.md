# Signal Design – Inaccurate Account Calibration

## Decision
Determine which accounts should enter a human investigation queue for potential
inaccurate skill calibration, without triggering automatic enforcement.

---

## Candidate Signals

| Signal | Definition | Grain | Rationale |
|------|-----------|-------|-----------|
| Winrate Delta | Winrate vs division average | player × window | Detects sustained dominance |
| KDA Z-Score | KDA standardized within division | player × window | Normalizes role differences |
| Match Impact Score | Composite performance metric | player × match | Captures overall contribution |
| Volatility Index | Std dev of performance metrics | player × window | Differentiates smurfs vs streaks |
| Sample Size Gate | Minimum matches threshold | player | Prevents early misclassification |

---

## Selected Signals

### 1. Sustained Winrate Delta
Selected because it:
- Is intuitive to reviewers
- Aligns with competitive outcomes
- Can be thresholded conservatively

### 2. Role-Adjusted KDA Z-Score
Selected to:
- Reduce bias across roles
- Avoid raw KDA misuse
- Highlight abnormal performance within peer groups

### 3. Sample Size Gate
Required guardrail to:
- Avoid flagging new or low-activity players
- Align with decision brief minimum sample guidance

---

## Rejected Signals

### Single-Match Outperformance
Rejected due to:
- High variance
- Encourages false positives
- Misaligned with sustained miscalibration definition

### Raw Damage Metrics
Rejected because:
- Strongly role-dependent
- Easily gamed
- Poor signal-to-noise ratio

---

## Known Limitations
- Performance proxies may lag true skill
- Division averages may drift over time
- Signals cannot distinguish intent (smurfing vs sandbagging)

These signals are intended to **prioritize review**, not determine enforcement.

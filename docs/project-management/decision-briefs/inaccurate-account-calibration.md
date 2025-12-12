# Investigation Queue for Inaccurate Calibration

- **Owner:** Moderation Team
- **Cadence:** Weekly (with ad-hoc flags nightly)
- **Decision:** Which accounts should enter a human investigation queue for possible enforcement action?

## Options / actions
- A0: No action (ignore)
- A1: Monitor (watchlist)
- A2: Warn (offer voluntary inaccurate identification)
- A3: Request Recalibration
- A4: Issue balance shuffle ban
- A5: Investigate for intentional cheating (ie MMR drops)

## Success definition
- Reduce bad/skewed games where single players dominate.

## Guardrails
- No permanent bans triggered by score alone.
- Minimum sample size before escalation (e.g., ≥ 15 inhouses in last 60 days).

## Inputs (high level)
- Player identity + current flags + verification confidence
- Performance signals derived from match history + rollups
- Optional risk flags (provider privacy, external smurf flag)

## Outputs
- Ranked investigation queue
- Recommended action (A0–A6)
- Evidence summary for reviewer (top signals + match pointers)

# Signal Design – <Decision Name>
Player winrates by lobby's rank average

## Decision
What decision does this signal design support?
Inaccurate account calibration
https://saltfreegaming.github.io/analytics-docs/project-management/decision-briefs/inaccurate-account-calibration/#options-actions
---

## Candidate Signals

| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|
|Division Winrate| Division Winrate | player x division | Detects division overperformance/underperformance |
|Role Winrate | Winrate per role | player x role | Detects role overperformance/underperformance,overall winrate breakdown|
|Overall Role Winrates | Winrate per role summed | player x role | Detects overall overperformance/underperformance, sign of mmr inaccuracy |
| Avg GPM/XPM | Average GPM/XPM performance of player | player x game result | Detects high average performance(win or loss) | 
| Win-loss GPM/XPM Delta |  Difference in GPM/XPM compared to avg | player x match x avg | Detects variation in performance |
| Role GPM/XPM Delta | Role based average GPM/XPM | player x role x avg | Controls for role based differences in performance |
|Lobby rank delta| Lobby average rank difference to player | player x avg lobby rank | controls for variation pertaining to rank advantage |
---

## Selected Signals
Division Winrate
### <Signal Name>
Why this signal was selected:
This signal was selected as it allows us to reference the spread of lobbies that the player is participating in, as we have the player's calibrated rank.  


What it captures that others do not:
While winrate by ticket allows us to capture the event's winrate, it does not account for the particpants of the lobby
This signal would allow us to capture the effect of the lobby's average rank participation, rather than relying solely on ticketed rank segments by event.

---

## Rejected Signals

### <Signal Name>
Why it was rejected.
What risk or failure mode it introduces.

---

## Guardrails
Explicit constraints applied:
- Minimum sample size: 10-30 games played across events, 10 for individual contexts
- Required context : 
    Rank brackets still have to be contextualised against the player's calibrated rank
- Known exclusions 

---

## Known Limitations
What this **cannot** reliably detect.
Where human judgment is required.

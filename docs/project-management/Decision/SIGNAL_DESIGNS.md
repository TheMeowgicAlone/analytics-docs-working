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
| Hero GPM/XPM percentile | Player's hero performance by public percentile | Player x Hero | Indicates the player's performance percentile on a given hero, controls for hero specific factors |
| Team avg Percentile delta | Deviation from lobby's percentile | match x team x player x hero | Controlling for win/loss variance |


---

## Selected Signals

### <Division Winrate>
Why this signal was selected:
This signal was selected as it allows us to reference the spread of lobbies that the player is participating in, as we have the player's calibrated rank.  
The winrates should be centered around 50% for balanced shuffled lobbies

What it captures that others do not:
This signal would allow us to capture the effect of the lobby's average rank participation, rather than relying solely on ticketed rank segments by event.

While winrate by ticket allows us to capture the event's winrate, it does not account for the particpants of the lobby and role makeup.

### <Role winrate>
Why this signal was selected:
Segmenting the player's winrate to control for variation introduced by role difference, a player hovering at 50% winrate could be a result of having to off-role. 
Also showing the makeup of a player's winrate total

What it captures that others do not:
Role based winrates being lopsided for specific roles (inaccurate rank calibration)

### <Overall Role winrate>
Why this signal was selected:
This signal shows a player's overall winrate against the sum of winrates. Thus providing an obvious signal for outlier performance (winrate above 50%, AND high role winrates above 50%) 
Used with role winrate for detailed breakdown

What it captures that others do not:
An average performance should hover around 150% total across 3 roles, or lower if anticipated 50%. Captures expected performance, as well as under/over performing players

Top right- High overall winrate, high sum role winrate, 
Top left- High overall winrate, 1 high role winrate, 2 low winrate, 
Bottom left- low overall winrate, low role winrate
Bottom mid-low overall winrate, high role winrate

---

## Rejected Signals
The signals that have not been included are still under consideration
### <Signal Name>
Why it was rejected.
What risk or failure mode it introduces.

---

## Guardrails
Explicit constraints applied:
- Minimum sample size: 30 games played across events, 10 for individual contexts(balanced shuffle,player draft, roles, lobby brackets)
- Required context : 
    Performance still have to be contextualised against the player's calibrated rank, participation in lobbies above their rank should result in a lowered performance
    Winrates are supposed to be around 50% for balanced shuffle lobbies, but balance shuffle does not account for player roles. 
- Known exclusions 

---

## Known Limitations
What this **cannot** reliably detect.
Where human judgment is required.
The signals cannot reliably detect  an individual's performance and account for their rank solely on gpm/xpm, and might require on an hero basis percentile check. However with limited matches it would not be a large enough sample size for an accurate representation  



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
|Lobby winrates|A player's winrate should hover around 50% around their calibrated rank  | per player per lobby average rank bracket | Seeing the player's winrates at different brackets can allow us to see where player performance peters off and give an idea on the accurate rank bracket   |

---

## Selected Signals
Winrate, segmented by lobby rank bracket
### <Signal Name>
Why this signal was selected:
This signal was selected as it allows us to reference the spread of lobbies that the player is participating in, as we have the player's calibrated rank.  
This captures the effects of players filling in for events, as this would not be accurately recorded on a granular level by events(spread is possibly large)

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
- Minimum sample size: 10 games played across events, 3 per rank bracket 
- Required context : 
    Larger sample size is more effective as the player's participation spread can be limited in some rank categories.
    Rank brackets still have to be contextualised against the player's calibrated rank
- Known exclusions 

---

## Known Limitations
What this **cannot** reliably detect.
Where human judgment is required.
This only detects up to a certain point, hovering around the divine rank bracket, as pure immortal lobbies are extremely uncommon. 
This does not take into account gpm/xpm or performance indicators in that particular lobby, balanced shuffled lobbies should still hover around 50% winrates if the same pool of players are consistently involved. Player's roles are also not controlled for in this case, as this plays a large part in a lobby's outcome and resulting winrates.
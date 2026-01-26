# Signal Design: Conversion Pipeline

## Decision
What decision does this signal design support?
Improve Conversion Pipeline
https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/decisions/improve-conversion-pipeline/1-decision-brief.md
---

## Candidate Signals

| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|


### Member activity 
| User messages  | User messages | server x population x messages | Number of messages sent by users split by roles | 
| New User activity % | new user messages/joins channel (yes/no) | server x population + age x messages | Did a new user interact? | 
| New/Fresh member activity | Activity of users below 4months | Server x population x age | Message history of members with age below 4months, participations, channel joins, allows us to see at individual grains/ monthly grains | 
| Additional charts could include a breakdown for individual user activity, day of week of activity, duration of joins/messages, Event only participation message, chatting |

### Population
| Population Role Assignment breakdown | Conversion rate and Role Distribution | Server x population | Knowing the server role distribution allows us to target conversion / activities for newer members | 
| Role promotion history | Role promotion over time | server x roles x time | Promotions for new>fresh>club members to be used as a sign of promotion health, (good health/bad health) | 
| Cohort population (Quarterly/ Biannual) | Population of cohorts | Server x population | Cohorts of the recent months of members, split into different month segments | 

### Events
| Events & participation allowance (provisional) | Events and who is allowed to participate | server x population | Certain events might not be in the territory that a user can interact with, spotting zones that could be improved or to include users that might want to participate but cant | 
|  |  |  |  | 
|  |  |  |  | 

---

## Selected Signals

### Server joins
Why this signal was selected:

What it captures that others do not:




---

## Rejected Signals
The signals that have not been included are still under consideration
### Server population delta
Why it was rejected:

What risk or failure mode it introduces:




---

## Guardrails
Explicit constraints applied:
-
- Required context : 
- Known exclusions 

---

## Known Limitations
What this **cannot** reliably detect.




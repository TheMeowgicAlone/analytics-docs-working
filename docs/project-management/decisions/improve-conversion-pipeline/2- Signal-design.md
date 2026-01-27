# Signal Design: Conversion Pipeline

## Decision
What decision does this signal design support?
Improve Conversion Pipeline
https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/decisions/improve-conversion-pipeline/1-decision-brief.md
---

## Candidate Signals

| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|
| User messages  | User messages | server x population x messages | Number of messages sent by users split by roles (information metric) | 
| New User activity % | new user messages/joins channel (yes/no) | server x population + age x messages | Did a new user interact?, if a new user joined but did not interact, flag for information (information metric - KPI Bounce rate) | 
| New/Fresh member activity | Activity of users below 4months | Server x population x age | Message history of members with age below 4months, participations, channel joins, allows us to see at individual grains/ monthly grains, (information metric - KPI: New member retention rate, (Fresh) Club member retention rate) | 
|||| Additional charts could include a breakdown for individual user activity, day of week of activity, duration of joins/messages, Event only participation message, chatting |
| Population Role Assignment breakdown | Conversion rate and Role Distribution | Server x population | Knowing the server role distribution allows us to target conversion / activities for newer members, allows us to identify areas that targetted conversion could be used | 
| Role promotion history | Role promotion over time | server x roles x time | Promotions for new>fresh>club members to be used as a sign of promotion health  (information metric - KPI Club member conversion rate) | 
| Cohort population (Quarterly/ Biannual) | Population of cohorts | Server x population | Cohorts of the recent months of members, split into different month segments  (information metric) | 
| Events & participation allowance (provisional) | Events and who is allowed to participate | server x population | Certain events might not be in the territory that a user can interact with, spotting zones that could be improved or to include users that might want to participate but cant | 


---

## Selected Signals
### Server joins
Why this signal was selected:
What it captures that others do not:


---

## Rejected Signals
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




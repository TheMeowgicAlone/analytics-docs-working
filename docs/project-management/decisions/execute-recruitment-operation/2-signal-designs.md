# Signal Design: Execute Recruitment Operation

## Decision
What decision does this signal design support?
Execute recruitment operations
https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/decisions/execute-recruitment-operation/1-decision-brief.md
---

## Candidate Signals

| Signal | Definition | Grain | Why It Might Help |
| Server population delta | Weekly difference for server population | Server x Population | Increase/Drops in server population must be noted |

| Server total population | Server population | Server x Population | Total numbers (used with delta) | 

| Server joins (week/month) | Server joins | Server x population | New user joins (raw numbers) | 

| Server Growth % | New population in server population % | Server x population  | Records new server population | 

|------|-----------|-------|------------------|

### Population



### Roles(Discord)
| Role population | Role population | server x population | Number of users in a particular role, new =/= promoted  | 
| Role promotion history | Role promotion over time | server x roles x time | new members role assignment + club member role assignment, promotions help to identify club member growth | 
| New member Role Age | Age of new member | server x population / role | Age of new members also shows how many new members stayed there | 
| Cohort population (Quarterly/ Biannual) | Population of cohorts | Server x population | Cohorts of the recent months of members | 

### Channels
| Channel voice hours/minutes | Voice hours per day | Server x channel x time | Representation of activity | 
| Channel joins (day/week) | User joined channel | server x population | total channel joins | 

### New user activity
| New User messages  | new user messages | server x population + age x messages | Number of messages sent by new users | 
| new user activity % | new user messages/joins channel (yes/no) | server x population + age x messages | Did a new user interact? | 

### Messages
| Message quantity | Messages and where they were sent | server x channel | Messages and which channels people interact in | 

---

## Selected Signals

### Server joins
Why this signal was selected:
This signal shows the amount of new population entering the server
This captures server growth in its base form, required to contextualise population growth

What it captures that others do not:
-base form- 

### Role population / cohort population
Why this signal was selected: 
This describes the server population with relation to their role/cohorts, directly showing the number of new server joins in the past months, as well as the population that stayed as new members, promoted to fresh members, and promoted to club members. As well as the inactive population for longer periods of time.

What it captures that others do not:
The total number of the poulation that is sitting in a particular role/cohort helps to contextualise the growth in the server and where they are ending up
Shows the overall health of the new server population

### Role promotion history (week/month)
Why this signal was selected: 
This allows us to see how many members have been promoted in the past months/weeks. When there has been no promotions for a month, it could be a sign that the existing population of new players are not returning. 

What it captures that others do not:
Capturing the current intake of fresh members, as well as the new member population that is entering the fresh member territory
Sign to not promote too many too quickly, as well as when there is no promotions happening



---

## Rejected Signals
The signals that have not been included are still under consideration
### Server population delta
Why it was rejected:
The population that joins the server but does not participate, and does not leave contributes to this number
What risk or failure mode it introduces:
Inaccurate representation of server health


### Rejected (unrelated)
| Server Interaction/Engagement | Population in server that interacts with events quantified | server x population x event participation | engagement directly affects server health | 
| Server Inactivity % | Population in server that is inactive | server x population x duration | checking inactivity duration > outreach health, engagement health | 
| Active Population % | Population with active participation | server x population x duration | engagement and retention health | 

Why these signals was rejected:
Unrelated for recruitment operation

### Server Inactivity 
Why this signal was selected:
If the server inactivity rate is going up, that would directly mean that either event retention is low/ needed to be looked at. 
Additionally, active/inactive time can also be quantified for the server population

What it captures that others do not:
Inverse of server engagement, effectively capturing "dead" population

---

## Guardrails
Explicit constraints applied:
-
- Required context : 
- Known exclusions 

---

## Known Limitations
What this **cannot** reliably detect.




# Signal Design: Execute Recruitment Operation

## Decision
What decision does this signal design support?
Execute recruitment operations
https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/decisions/execute-recruitment-operation/1-decision-brief.md

The key performance indicators for the recruitment portion are as follows:

New member amount

New members that are converted known as fresh members

New member participation (Related to conversion pipeline)

---

## Candidate Signals
| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|
| Server population delta | Weekly/Monthly difference for server population | Server x time | Server population and growth used as a information metric  |
| Server total population | Server population | Server x time | Does not inform the decision (used with above) |
| Server Growth % | New population in server population % over past months | Server x time  | Does not inform the decision (used with above) | 
| Server joins (week/month) | Server joins | Server x population | New user joins (raw numbers), required to be tracked, if server joins do not meet a certain number, recruitment operation might be required | 


| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|
| Role population | Role population | server x population | Does not inform decision (information metric) | 
| Role promotion history | Role promotion over time | server x roles x time | new members role assignment + club member role assignment, promotions help to identify club member growth, if there has been no promotions in a period, recruitment operation might be required | 
| New member Role Age | Age of new member | server x population / role | Age of new members also shows how many new members stayed there, if many new members are old, recruitment operation might be required |
| Fresh member Role Age | Age of fresh member | server x population / role | Same as above, but to signal if there is no requirement for recruitment operation |
| Cohort population (Quarterly/ Biannual) | Population of cohorts | Server x time | Does not inform decision (information metric) | 
| Channel voice hours/minutes | Voice hours per day | Server x channel x time | Representation of activity,  Does not inform the decision (information metric)  | 
| Channel joins (day/week) | User joined channel | server x channel | total channel joins,  Does not inform the decision (information metric)  | 
| New User messages  | new user messages | server x population + age x messages | Number of messages sent by new users, if there has been no new user messages, signal that a recruitment operation might be required | 
| new user activity % | new user messages/joins channel (yes/no) | server x population + age x messages | Did a new user interact,if there has been no new user interaction, signal that a recruitment operation might be required | 
| New user Message breakdown | Messages and where they were sent | server x population + age x messages | Messages and which channels new people interact in, Does not inform the decision (information metric) | 


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
| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|
| Server Interaction/Engagement | Population in server that interacts with events quantified | server x population x event participation | engagement directly affects server health, low engagement can be addressed by recruitment | 
| Server Inactivity % | Population in server that is inactive | server x population x duration | checking inactivity duration > outreach health, engagement health, does not inform decision(unrelated) | 
| Active Population % | Population with active participation | server x population x duration | engagement and retention health, does not inform decision(information metric) | 

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
- Recruitment operation should not be carried out if there is sufficent organic growth, additionally, relating to conversion, if there is enough conversion but low organic growth, do not carry out recruitment operation
- Hence: if new members below x amount(Signal to carry out recruitment), but fresh member above x amount (Good conversion amount), do not carry out recruitment
- Required context : New members are not necessarily converted to a fresh member, as a means to an end, a recruitment operation is aimed to recruit members which will remain and participate in the server, numbers to be tuned according to server population size

---





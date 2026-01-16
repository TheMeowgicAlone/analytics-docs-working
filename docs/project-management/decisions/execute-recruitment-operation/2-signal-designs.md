# Signal Design: Execute Recruitment Operation

## Decision
What decision does this signal design support?
Execute recruitment operations
https://github.com/saltfreegaming/analytics-docs/blob/main/docs/project-management/decisions/execute-recruitment-operation/1-decision-brief.md
---

## Candidate Signals

| Signal | Definition | Grain | Why It Might Help |
|------|-----------|-------|------------------|
| Server population delta | Weekly difference for server population | Server x Population | Increase/Drops in server population must be noted |
| Server Growth % | New population in server population % | Server x population  | Records new server population | 
| Server Interaction/Engagement | Population in server that interacts with events quantified | server x population x event participation | engagement directly affects server health | 
| Server Inactivity % | Population in server that is inactive | server x population x duration | checking inactivity duration > outreach health, engagement health | 
| Active Population % | Population with active participation | server x population x duration | engagement and retention health | 
| | | | | 
| | | | | 


---

## Selected Signals

### Server growth
Why this signal was selected:
This signal shows the amount of new population entering the server
This captures server growth in its base form, required to contextualise population delta, engagement, growth

What it captures that others do not:
-base form- 

### Server engagement
Why this signal was selected:
This signal directly quantifies server activity, and is the main KPI for this scenario, as server growth would mean a larger engaged population.   

What it captures that others do not:
Server activity and participation

### Server Inactivity 
Why this signal was selected:
If the server inactivity rate is going up, that would directly mean that either event retention is low/ needed to be looked at. 
Additionally, active/inactive time can also be quantified for the server population

What it captures that others do not:
Inverse of server engagement, effectively capturing "dead" population

---

## Rejected Signals
The signals that have not been included are still under consideration
### Server population delta
Why it was rejected:
The population that joins the server but does not participate, and does not leave contributes to this number
What risk or failure mode it introduces:
Inaccurate representation of server health

---

## Guardrails
Explicit constraints applied:
-
- Required context : 
- Known exclusions 

---

## Known Limitations
What this **cannot** reliably detect.




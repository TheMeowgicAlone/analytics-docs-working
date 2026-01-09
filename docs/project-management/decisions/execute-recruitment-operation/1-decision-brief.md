# Execute Recruitment Operation

- **Owner:** Promotion Team
- **Cadence:** Monthly
- **Decision:** Should we run a recruitment operation (ie recruitment tournament)?

## Options / actions
- A0: All Good (green)
- A1: Increase monitoring frequency to weekly (yellow).
- A2: Initiate promote operation (red).

## Success definition
- A promotion team member can see at a glance if the club's current new member inflow rate is healthy.

## Guardrails
- This should pull on a large enough window of time to not false positive.
- We want to be VERY sure, because a tournament is extremely expensive.
- Bounces need to be excluded (a join and sudden leave should not count as a join)

## Input Information (Suggested)
- join_date field for discord_users (or club_members if you want more future proof but less tied to specific user behavior).

## Outputs
- Necessary charts to determine if we're in a danger situation.
- KPI: % server growth monthly (growth as a percentage of the current size)
- % Organic growth (cost permitting)

## Quickstart
Helpful basic info.

### Use these datasets by default
- `match_facts_v1` — default for user-centric match analysis in Superset
- `official_dota_matches_view` — match source of truth (includes overrides)
- `dota_league_stats_view` / `dota_overall_stats_view` — pre-aggregated rollups

### Identity keys (do not confuse these)
- `member_id` — internal person identity
- `discord_user_id` — Discord account identity (may change per member)
- `steam_id` — Steam account identity (alts possible)
- `match_id` — Dota match identifier

### Primary Accounts
- Members must have:
    - one member_id (represents a human)
    - one primary Steam account (Others are considered smurf accounts. Note a smurf can transition to a main.)
    - one primary Discord account (Other accounts are considered alts. Note the system assumes each new account = new human. This is retroactive.)

### Discord Names
- **username**: unique amongst all discord users, cannot be changed
- **nickname**: set per server
- **global_name**: global nickname
- **display_name**: locally resolved (context-specific) nickname. Utilized both in PwR and in Discord logically. 

### Safe join helpers
- Use `pivot_view` to map between member/discord/steam quickly.

## Sensitive Fields (Do Not Use Without Approval)
The following fields should not appear in dashboards and should not be used in analysis
unless explicitly approved and documented:
- DOB, race, pronouns, gender-related fields
- Freeform `note` fields
- Moderation-only names/labels

Default: dashboards should use internal IDs + display_name only.
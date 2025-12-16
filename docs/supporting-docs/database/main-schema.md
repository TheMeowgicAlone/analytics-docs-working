# Dota Data Entity Taxonomy

Core data entities for SFG's Dota inhouse program analytics. Use these definitions to ensure consistent analysis, dashboard design, and stakeholder communication.

**Authoritative Source:** Database schema defined in `saltfreebi/common/db/models/main.py`

---

## Core Entities

### Member (`club_members`)
The root identity for community participation across all SFG programs.

**Purpose:** Central record linking Discord identities, Steam accounts, and participation across programs.  
**Table:** `club_members`  
**Primary Key:** `member_id` (BigInteger, auto-increment)  
**Key Attributes:**
- `member_id`: Internal unique identifier
- `join_date`: When the member joined SFG (timestamp with timezone)
- `left_date`: When the member departed (nullable)
- `first_name`, `last_name`, `middle_name`: Personal information (optional)
- `moderation_name`: Alternative name for moderation team use
- `dob`, `pronouns`, `race`, `gender_nonconform`: Demographic data (optional)
- `note`: Freeform text for member notes

**Relationships:**
- One member → many Discord users (accounts)
- One member → many Steam users (accounts)
- One member → one Dota player profile
- One member → many event attendance records
- One member → many audit trail records

**Analytics Use:** Population analysis, retention tracking, member lifecycle analysis, demographic insights.

---

### Discord User (`discord_users`)
A Discord account identity.

**Purpose:** Represents a specific Discord account (user ID, username, discriminator).  
**Table:** `discord_users`  
**Primary Key:** `discord_user_id` (BigInteger)  
**Key Attributes:**
- `discord_user_id`: Discord's unique user ID
- `member_id`: Link to club member (foreign key)
- `discord_name`: Username (max 32 chars)
- `discriminator`: Four-digit discriminator (deprecated by Discord, integer)
- `global_name`: Display name across Discord
- `primary_flag`: Boolean indicating if this is the member's primary Discord account
- `is_bot`: Boolean flag for bot accounts
- `is_discord_admin`: Boolean flag for Discord admin status

**Cardinality:** Many-to-one with Member (one member may have multiple Discord accounts, but typically one primary).  
**Constraints:** At most one primary Discord account per member (partial unique index).  
**Analytics Use:** Identity resolution, activity attribution, admin/moderator tracking.

---

### Discord Member (`discord_members`)
A Discord user's membership in a specific guild (server).

**Purpose:** Tracks guild-specific attributes like nickname, join/leave dates.  
**Table:** `discord_members`  
**Primary Key:** `entry_id` (auto-increment)  
**Unique Constraint:** `(guild_id, discord_user_id)`  
**Key Attributes:**
- `guild_id`: Discord server ID
- `discord_user_id`: Foreign key to discord_users
- `member_id`: Foreign key to club_members
- `join_date`, `left_date`: Guild membership lifecycle
- `nickname`: Server-specific nickname
- `display_name`: Effective display name in guild
- `extra`: JSONB field for additional metadata

**Analytics Use:** Server-specific behavior analysis, nickname changes, guild tenure.

---

### Steam User (`steam_users`)
A Steam account linked to a club member.

**Purpose:** Represents a Steam identity used for Dota 2 and other Steam games.  
**Table:** `steam_users`  
**Primary Key:** `steam_id` (BigInteger, Steam's 64-bit ID)  
**Key Attributes:**
- `steam_id`: Steam's unique 64-bit account ID
- `member_id`: Link to club member (foreign key, nullable)
- `steam_name`: Display name on Steam (max 45 chars)
- `primary_flag`: Boolean indicating primary account (default true)

**Cardinality:** Many-to-one with Member (one member may link multiple Steam accounts for alt detection).  
**Constraints:** At most one primary Steam account per member (partial unique index).  
**Analytics Use:** Smurf/alt detection, cross-account aggregation, Steam name history.

---

### Dota Player (`dota_players`)
Extended profile for a Dota 2 player with rank, calibration flags, and provider data.

**Purpose:** Central record for skill assessment, rank tracking, and matchmaking suitability.  
**Table:** `dota_players`  
**Primary Key:** `entry_id` (auto-increment)  
**Unique Constraints:** `member_id`, `steam_id` (one-to-one relationships)  
**Key Attributes:**
- `member_id`: Foreign key to club_members
- `steam_id`: Foreign key to steam_users
- `is_disabled`: Exclude from monitoring and operations (boolean, default false)
- **Rank data from providers:**
  - `leaderboard_dotabuff`, `leaderboard_stratz`, `leaderboard_opendota`: Leaderboard rank (nullable integers)
  - `rank_opendota`, `rank_stratz`, `rank_dotabuff`: Medal rank integers (Herald 1 = 11, Crusader 1 = 31, etc.)
- **Privacy flags:**
  - `is_private_dotabuff`, `is_private_opendota`, `is_private_stratz`: Boolean flags
- **Calibration & placement:**
  - `is_inaccurate_calibration`: Flag for unsuitable shuffle participation (boolean)
  - `is_inaccurate_listing`: Flag requiring careful drafting (boolean)
  - `role_note`: Short text for captain guidance (max 32 chars)
- **MMR estimates:**
  - `pwr_rank`: Custom PWR rank override (nullable integer)
  - `mmr_estimate`: Internal MMR estimate
  - `mmr_last_estimated`, `mmr_was_estimated`: Tracking for estimates
  - `actual_mmr`, `actual_mmr_last_updated`, `actual_mmr_was_verified`: Verified MMR data
- **Provider metadata:**
  - `region_opendota`, `region_stratz`, `region_dotabuff`: Region per provider (max 256 chars)
  - `last_updated_opendota`, `last_updated_stratz`, `last_updated_dotabuff`: Last fetch timestamp per provider
- `sz_smurf_flag`: Valve smurf detection flag (nullable boolean)

**Relationships:**
- One member → one Dota player (enforced by unique constraint)
- One Steam ID → one Dota player (enforced by unique constraint)

**Analytics Use:** Rank distribution, calibration accuracy, provider data quality, matchmaking balance, smurf detection, captain drafting insights.

---

### Dota Ticket (`dota_tickets`)
A league, division, or event ticket defining a matchmaking context.

**Purpose:** Categorizes matches by league, season, or event type.  
**Table:** `dota_tickets`  
**Primary Key:** `ticket_id` (BigInteger)  
**Key Attributes:**
- `ticket_id`: Unique ticket identifier
- `name`: Ticket name (max 128 chars)
- `description`: Detailed description (max 2000 chars)
- `active`: Boolean indicating if ticket is currently active

**Relationships:** One ticket → many match participations.  
**Analytics Use:** League health metrics, ticket activity trends, historical league comparisons.

---

### Official Match Records (`dota_official_event_matches`)
A player-in-match record for official inhouse games.

**Purpose:** Captures individual participation, role, and performance in a single match.  
**Table:** `dota_official_event_matches`  
**Primary Key:** `entry_id` (auto-increment)  
**Unique Constraint:** `(steam_id, match_id)`  
**Key Attributes:**
- `match_id`: Dota 2 match ID (BigInteger)
- `steam_id`: Player's Steam ID (BigInteger, nullable)
- `ticket_id`: Foreign key to dota_tickets
- `start_timestamp`, `end_timestamp`: Match lifecycle (timestamps with timezone; end_timestamp null if ongoing)
- `team_id`: Team identifier (BigInteger, nullable)
- `win`: Boolean outcome
- `radiant`: Boolean indicating Radiant side (true) or Dire (false)
- `is_captain`: Boolean flag for captain role
- `average_rank`: Average rank of lobby (nullable integer)
- `position`: Lane position 1–5 (nullable integer)
- `role`: Role description (max 32 chars, nullable)
- `gold_per_minute`: GPM stat (nullable integer)
- `game_mode`: Mode string (e.g., "Captain's Mode", max 32 chars, nullable)
- `all_talks`: JSONB array of voice activity or chat metadata (nullable)

**Cardinality:** One match → typically 10 participations; one player → many participations over time.  
**Analytics Use:** Win rate by player, ticket, role, position, team composition; captain performance; lobby balance; match volume trends.

---

### Match Info Overrides (`dota_official_event_matches_overrides`)
Manual corrections or additions to match participation data.

**Purpose:** Allows league managers to correct erroneous match data or add missing records.  
**Table:** `dota_official_event_matches_overrides`  
**Primary Key:** `override_entry_id` (auto-increment)  
**Unique Constraint:** `(steam_id, match_id)`  
**Key Attributes:** Mirrors `dota_official_event_matches` plus audit fields:
- `created_by_discord_id`, `updated_by_discord_id`: Audit trail
- `created_at`, `updated_at`: Timestamps for change tracking
- `game_type`: Type of game (default "Dota", max 32 chars)

**Analytics Use:** Data quality tracking. 

---

### Reconciled Match View (`official_dota_matches_view`)
Unified view combining official match data with manual overrides.

**Purpose:** Provides a single source of truth for match analysis by prioritizing overrides.  
**Table/View:** `official_dota_matches_view` (database view)  
**Primary Key:** `(match_id, steam_id)`  
**Key Attributes:** All fields from `dota_official_event_matches` plus:
- `player_name`: Player's internal name (max 45 chars)
- `steam_name`: Steam display name (max 45 chars)
- `game_type`: Game type (max 32 chars, nullable)

**Analytics Use:** Preferred source for dashboard queries, reporting, and aggregations.

---

### Match Facts View (`match_facts_v1`)
Row-level match facts attributed to primary Discord identity.

**Purpose:** Staging view for analytics that maps all Steam IDs to primary Discord identity.  
**Table/View:** `match_facts_v1` (database view)  
**Primary Key:** `(discord_user_id, match_id)`  
**Key Attributes:**
- `discord_user_id`: Primary Discord identity
- `discord_name`, `display_name`: Display names
- `steam_id`: Concrete Steam ID used in match (preserves alt visibility)
- `ticket_id`, `match_id`: Match identifiers
- `start_timestamp`, `end_timestamp`: Match lifecycle
- `win_int`, `is_captain_int`, `radiant`: Boolean outcomes as integers
- `average_rank`, `position`, `role`, `gold_per_minute`: Match stats
- `all_talks`: Serialized JSON string

**Analytics Use:** User-centric aggregations, captain vs. non-captain comparisons, cross-ticket analysis.

---

## Aggregated Statistics Views

### League Stats by User (`dota_league_stats_view`)
Per-ticket rollup across all Steam IDs for a member.

**Purpose:** Provides ticket-level statistics attributed to primary Discord identity.  
**Primary Key:** `(discord_user_id, ticket_id)`  
**Key Attributes:**
- `discord_user_id`, `ticket_id`: Composite key
- `primary_steam_id`, `primary_steam_name`: Current primary Steam account for labeling
- Player aggregates: `games_played`, `wins`, `winrate` (Numeric 6,4)
- Captain aggregates: `games_played_as_captain`, `wins_as_captain`, `winrate_as_captain`
- `captain_winrate_delta`: Captain vs. player winrate difference (nullable, requires minimum sample size)

**Analytics Use:** Ticket leaderboards, captain evaluation, per-league performance tracking.

---

### Overall Stats by User (`dota_overall_stats_view`)
Aggregate statistics across all tickets for a member.

**Purpose:** Member-level career statistics.  
**Primary Key:** `discord_user_id`  
**Key Attributes:** Same as `dota_league_stats_view` but without `ticket_id`.  
**Analytics Use:** Career summaries, overall captain performance, member comparison.

---

### Debug Aggregates (Per-Steam Views)
Diagnostic views showing statistics at the Steam account level (not rolled up to member).

**Tables/Views:**
- `player_ticket_per_steam_dbg`: Per-ticket player stats by Steam ID
- `player_overall_per_steam_dbg`: Overall player stats by Steam ID
- `captain_ticket_per_steam_dbg`: Per-ticket captain stats by Steam ID
- `captain_overall_per_steam_dbg`: Overall captain stats by Steam ID

**Purpose:** Alt account analysis, smurf detection, data quality validation.  
**Primary Keys:** Composite of `(discord_user_id, steam_id)` plus `ticket_id` where applicable.

---

## Pivot & Utility Views

### Pivot View (`pivot_view`)
Convenience view for ID type conversions.

**Purpose:** Simplifies joins between member, Discord, and Steam identities.  
**Primary Key:** `member_id`  
**Key Attributes:**
- `member_id`: Club member ID
- `discord_user_id`: Primary Discord user ID (nullable)
- `steam_id`: Primary Steam ID (nullable)
- `steam_name`, `discord_name`: Display names
- `display_name`: Coalesced name (Discord member display → global → username)

**Analytics Use:** ID lookups, name resolution, quick joins for dashboards.

---

### Dota Player Full View (`dota_player_full_view`)
Comprehensive player profile combining member, Discord, Steam, and Dota data.

**Purpose:** Single view for all player-related attributes.  
**Primary Key:** `member_id`  
**Key Attributes:** All fields from `dota_players` plus member/Discord/Steam identifiers and names.  
**Computed Fields:**
- `effective_rank`: Final rank used for matchmaking (nullable integer)
- `effective_mmr`: Final MMR used for balancing (nullable integer)
- `mmr_last_updated`: Timestamp of last MMR update

**Analytics Use:** Player profile dashboards, rank distribution analysis, calibration status reports.

---

## Decision-Driven Analytics Considerations

These entities support multiple decision-making workflows:

1. **Matchmaking Balance:**
   - Use `dota_player_full_view` for effective rank/MMR distribution
   - Query `match_facts_v1` or `official_dota_matches_view` for team composition analysis
   - Identify calibration issues via `is_inaccurate_calibration` and `is_inaccurate_listing` flags
   - Detect smurfs using `sz_smurf_flag` and debug per-steam aggregates

2. **Captain Evaluation:**
   - Use `dota_league_stats_view` or `dota_overall_stats_view` for captain winrate delta
   - Filter for `games_played_as_captain >= threshold` to ensure sample size
   - Compare captain vs. player performance across tickets for promotion decisions

3. **League Health & Activity:**
   - Track active vs. inactive tickets via `dota_tickets.active`
   - Monitor match volume trends using `official_dota_matches_view` grouped by `ticket_id` and time
   - Identify abandoned tickets (low recent activity) for decommissioning

4. **Player Retention & Engagement:**
   - Join `club_members` with match participation to calculate activity recency
   - Segment by `join_date` cohorts for retention curve analysis
   - Use `left_date` for churn analysis and exit interviews

5. **Data Quality & Provider Reliability:**
   - Compare `rank_*` and `leaderboard_*` fields across providers (DotaBuff, Stratz, OpenDota)
   - Track `last_updated_*` timestamps to identify stale data
   - Monitor privacy flags (`is_private_*`) for coverage gaps
   - Audit override frequency via `dota_official_event_matches_overrides`

6. **Moderation & Governance:**
   - Use `command_audit` to track moderator actions and workload
   - Flag repeated calibration or listing issues for review
   - Analyze `role_note` for captain feedback patterns

7. **Smurf & Alt Detection:**
   - Cross-reference multiple `steam_users` per `member_id`
   - Use debug per-steam views to detect performance discrepancies across accounts
   - Correlate `sz_smurf_flag` with actual performance outliers

---
# Discord Gateway API

The Discord Gateway API is the primary ingress for player behavior and engagement signals. Although Superset does not interact with it directly, understanding what it exposes explains much of what is available.

## Key Event Types
- **Guild & Channel**: `GUILD_CREATE`, `CHANNEL_CREATE`, `CHANNEL_UPDATE`
- **Messaging**: `MESSAGE_CREATE`, `MESSAGE_UPDATE`, `MESSAGE_DELETE`
- **Voice**: `VOICE_STATE_UPDATE`, `SPEAKING_START`, `SPEAKING_STOP`
- **Presence**: `PRESENCE_UPDATE`, `TYPING_START`
- **Moderation**: `GUILD_MEMBER_ADD`, `GUILD_MEMBER_REMOVE`, `GUILD_BAN_ADD`

Reference the full event catalog in the [Discord Gateway API documentation](https://discord.com/developers/docs/topics/gateway-events). Capture which events feed each Superset dataset and note any filters (e.g., ignoring bot messages, private channels, or ephemeral interactions).

## Data Handling Guidelines
- Persist raw events with timestamps and correlation IDs for replay during investigations.
- Derive curated tables for sessions, message velocity, and role-based engagement.
- Mask or exclude sensitive content when unnecessary for analytics.
- Document refresh cadence and backfill strategies for every dataset consuming Gateway events.

## Example Lineage
```
Gateway events -> ingestion worker -> staging tables -> curated fact tables -> Superset datasets -> dashboards
```

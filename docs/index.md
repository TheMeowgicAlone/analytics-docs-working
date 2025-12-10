# Salt Free Gaming Analytics Documentation

Welcome to the documentation hub for Salt Free Gaming (SFG), a Discord-centered community focused on Dota 2 and other titles. This site is under construction, and will be changed heavily as time goes.

Consider all documents in DRAFT format.

## Credits
See [credits](credits.md) for contribution and design information.

## Purpose
- Develop and expose KPI
- Provide long term, high level insights.
- Provide detailed drilldowns for specific system analysis.
- Enable observability of complex systems.
- Establish behavioral understanding of members.
- Evaluate efficacy of services.

## Scope
- Long-term behavioral analytics live in **Apache Superset**; short-term operational logs stay in **Grafana**.
- Data primarily ingress via the **Discord Gateway API** and supporting services.
- Data is not necessary up to date, refreshes and processing tasks may need to be invoked.

## Quick Links
- [Discord Gateway API events](https://discord.com/developers/docs/topics/gateway-events)
- [Apache Superset documentation](https://superset.apache.org/docs)

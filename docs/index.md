# Salt Free Gaming Analytics Documentation

Welcome to the documentation hub for Salt Free Gaming (SFG), a Discord-centered community focused on Dota 2 and other titles. This site is the entrypoint for understanding how our Business Intelligence (BI) platform is planned, built, and operated.

## Credits
Salt Free Gaming's analytics platform is led by a small set of committed contributors:
- **John C. Quinn** — Project Manager
- **Gavin Chen** — Business Intelligence Engineer (Volunteer)

See [Credits](credits.md) for how we recognize and engage these leaders.

## Purpose
- Provide a single source of truth for Apache Superset dashboards, Grafana logging, data pipelines, and governance.
- Equip the Business Owner (project manager), the Analytics Engineer (M.S. Business Analytics & AI candidate), and contributors with actionable guidance.
- Capture institutional knowledge as the project moves through its four phases: **minimum**, **operational**, **acceptable**, and **exceptional**. Each phase follows a **design → build → test** iteration cycle.

## Scope
- Long-term behavioral analytics live in **Apache Superset**; short-term operational logs stay in **Grafana**.
- Data primarily ingress via the **Discord Gateway API** and supporting services.
- Documentation covers both **project management** practices and **reference materials** for dashboards, datasets, and queries.

## Getting Started
1. Review the [Platform Roadmap](strategy/platform-roadmap.md) to understand the phased rollout.
2. Align on the [Delivery Methodology](project-management/methodology.md) and team roles.
3. Use the [Dashboard Documentation Template](templates-and-checklists/dashboard-template.md) when creating or updating Superset content.
4. Build the site locally with `mkdocs serve` or generate static files with `mkdocs build --clean`.

## Quick Links
- [Discord Gateway API events](https://discord.com/developers/docs/topics/gateway-events)
- [Apache Superset documentation](https://superset.apache.org/docs)
- [Grafana documentation](https://grafana.com/docs/)

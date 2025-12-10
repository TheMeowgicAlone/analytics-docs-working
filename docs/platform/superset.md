# Apache Superset

Apache Superset houses long-term behavioral analytics for SFG. Because Superset configuration is not fully managed via IaC, documentation is the control plane for dashboards, datasets, and access.

## Scope of Documentation
- **Dashboards**: Purpose, audience, KPIs, and links to charts.
- **Charts**: Query logic, filters, and expected behavior.
- **Datasets**: Source tables, refresh cadence, owner, and sensitivity.
- **Permissions**: Roles and datasource access rules.

## Authoring Best Practices
- Align every dashboard with a phase (minimum/operational/acceptable/exceptional) and iteration status.
- Store SQL for custom queries in version control or paste into documentation with context and parameter notes.
- Reference Discord Gateway API events used (e.g., `MESSAGE_CREATE`, `VOICE_STATE_UPDATE`) to clarify lineage.
- Capture data validation steps comparing Superset to Grafana logs when relevant.

## Dashboard Definition of Done
- **Problem and audience are explicit**: Purpose, intended roles (moderation/community/engineering), and phase/iteration are completed in the Dashboard Documentation Template.
- **Data is trustworthy**: Datasets have owners, refresh cadence, and sensitivity noted; SQL is checked into version control; row-level security and time zone (default UTC) are set.
- **KPI clarity**: KPI formulas, filters, and validation steps against source systems (e.g., Discord Gateway, Grafana logs) are documented with acceptance thresholds.
- **User experience is ready**: Default filters work, charts render without errors or empty states, labels include units and grains, and colors meet accessibility contrast expectations.
- **Performance and stability**: Charts load within acceptable thresholds, caching or resampling is enabled when needed, and cross-filters do not break context.
- **Governance is in place**: Correct Superset roles are applied, sensitivity is communicated, and change log entries are updated.
- **Release artifacts exist**: Dashboard link, screenshots if helpful, and known gaps or limitations are captured in documentation before publishing.

## Working Standards
- Follow the [Superset Style Guidelines](superset-style-guidelines.md) for layout, naming, filters, accessibility, and privacy.
- Use the [Dashboard Documentation Template](../templates-and-checklists/dashboard-template.md) to capture required metadata and KPIs.
- Capture new asks using the [Dashboard Feature Request](../templates-and-checklists/dashboard-feature-request.md) template so stakeholders can document must-haves and acceptance criteria.

## Deployment Notes
- Superset content changes must be documented before publishing.
- Use the [Dashboard Documentation Template](../templates-and-checklists/dashboard-template.md) for each dashboard and chart.

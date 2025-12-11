# Superset Style Guidelines

Standards for building and shipping Superset dashboards at SFG. Use alongside the Dashboard Documentation Template and Feature Request template.

## Audience and Framing
- Start with who the dashboard serves (moderation, community, engineering, leadership) and what decision it supports.
- Optimize layouts for the viewing context: single screen when possible; trim tiles if stakeholders primarily use laptops or tablets.
- Group charts by journey stage (acquisition → engagement → retention) or workflow so readers can scan without jumping around.

## Layout and Storytelling
- Put the highest-signal tiles in the top-left; decrease detail as you move right and down.
- Keep KPI cards on the first row; avoid scroll bars by limiting dashboards to ~9–12 visuals.
- Use full-screen mode in reviews and ensure spacing is clean and uncluttered; remove redundant tiles.
- Provide clear drill paths: link out to detailed dashboards or reports instead of cramming every detail on one page.

## Visual Choices
- Favor bar/column, line, and area charts for comparisons and trends; avoid 3D charts.
- Use pie/donut sparingly (≤8 categories) for part-to-whole; use gauges only for goal tracking.
- Keep scales, ordering, and colors consistent across charts; reuse segment colors across the page.
- Avoid mixing huge and tiny measures on the same axis; use secondary axes only when necessary and clearly label them.
- Keep numbers readable: at most 3–4 significant digits, scale large values (e.g., 3.4M), and keep time frames consistent across visuals.
- Remove unnecessary data labels; rely on tooltips when values can be read from bars/lines.
- Sort charts to emphasize the story (e.g., highest-to-lowest for quick scanning).

## Filters and Interactions
- Provide global time and primary segment filters with safe defaults (30–90 days); document any required combinations to avoid misleading results.
- Prefer one dashboard with flexible time controls over separate “last 30 days,” weekly, and quarterly variants; create distinct dashboards only when audiences or workflows diverge.
- Use cross-filters carefully; verify they maintain context and do not break linked visuals.
- Make drill-through links explicit in subtitles or annotations so stakeholders know where to click.

## Dataset Access
- Concept: data presentation can be modified at 4 layers:
  - chart 
  - dataset (through SqlLab)
  - view
  - database (physical modification)
- Work with high level views first.
- Do not duplicate already created logic. Lack of duplication is a huge focus of PwR.

## Naming Conventions

### Dashboards

#### Format
<Domain>: <The question the dashboard answers>

#### Examples
- Dota Program: Are members playing enough matches by ticket and over time?
- Dota League: Who is winning and who are the outliers?
- Dota League Captains: Which captains are the most successful?
- Data Hygiene: Which player records and Steam accounts are incomplete?
- Discord Activity: When is the server alive and who is engaging?
- Cohorts: How are member segments growing, shrinking, and converting?

### Charts
<Metric> by <Dimension> (<Timeframe>)



## Naming, Labels, and Accessibility
- Name charts as `SFG <Domain>: <Metric> (<grain>)`; include units and time zones (default UTC) in titles or subtitles.
- Avoid game-specific jargon on shared dashboards; prefer plain language for non-technical readers.
- Ensure contrast meets accessibility expectations; add redundant cues (color + label/shape) instead of color alone.

## Data Hygiene and Security
- Surface owner, refresh cadence, and sensitivity in dashboard descriptions; annotate known data quality gaps.
- Prefer reusable datasets with version-controlled SQL; keep virtual dataset queries formatted and free of secrets.
- Apply row-level security and aggregate or mask PII where possible.

## Release Discipline
- Update the [Dashboard Documentation Template](../templates-and-checklists/dashboard-template.md) and record validation steps against source systems (Discord Gateway events, Grafana logs, warehouse queries).
- Capture new asks with the [Dashboard Feature Request](../templates-and-checklists/dashboard-feature-request.md) before build.
- Run the [Release Checklist](../templates-and-checklists/release-checklist.md) and confirm the dashboard meets the [Definition of Done](superset.md#dashboard-definition-of-done) before stakeholder release.

## Further Reading
- Superset official docs: [Dashboards](https://superset.apache.org/docs/creating-charts-dashboards/dashboards/), [Datasets](https://superset.apache.org/docs/creating-charts-dashboards/datasets/), [SQL Lab](https://superset.apache.org/docs/creating-charts-dashboards/sql-lab/).
- Visualization basics: [W3C contrast guidance](https://www.w3.org/TR/WCAG21/#contrast-minimum), [Gestalt principles overview](https://en.wikipedia.org/wiki/Principles_of_grouping).

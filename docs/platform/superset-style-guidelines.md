# Superset Style Guidelines
TODO: VERIFY & ACCEPT 

Standards for building and shipping Superset dashboards at SFG. Use alongside the Dashboard Documentation Template and Feature Request template.

## Layout and Framing
- Lead with a concise description of the SFG service or behavior under review and who should use the view (moderation/community/engineering).
- Keep KPI tiles on the first row; group charts by journey stage (acquisition → engagement → retention) and cap dashboards at ~9–12 charts to reduce cognitive load.
- Set default time and segment filters with safe ranges (last 30–90 days) and document drill paths.

## Naming and Labeling
- Use `SFG <Domain>: <Metric> (<grain>)` for charts and spell out units and time zones (default UTC).
- Avoid game-specific jargon on shared dashboards; prefer plain language for stakeholder clarity.
- Include grains and units in axis labels and tooltips; avoid abbreviations that can be misread.

## Filters and Interactions
- Provide global filters for time and primary segments; apply sensible defaults.
- Use cross-filters sparingly to prevent accidental context loss; test interactions to ensure consistent results across charts.
- Document any required filter combinations that keep results accurate.

## Accessibility and Visuals
- Favor high-contrast palettes; add redundant cues (color + shape/label) instead of red/green-only encodings.
- Reserve warm colors for alerts or regressions; avoid hue overload on categorical charts.
- Keep font sizes legible and use consistent number/date formats across charts.

## Data Hygiene and Security
- Surface owner, refresh cadence, and sensitivity in dashboard descriptions; annotate known data quality gaps.
- Prefer reusable datasets with version-controlled SQL; format queries for readability and avoid embedding tokens or secrets in virtual datasets.
- Apply row-level security where needed and aggregate PII wherever possible.

## Release Discipline
- Update the [Dashboard Documentation Template](../templates-and-checklists/dashboard-template.md) for every change and record validation steps against source systems (e.g., Discord Gateway, warehouse queries).
- Use the [Dashboard Feature Request](../templates-and-checklists/dashboard-feature-request.md) to capture must-haves, nice-to-haves, and acceptance criteria before build.
- Keep change logs current and confirm dashboards meet the [Definition of Done](superset.md#dashboard-definition-of-done) prior to stakeholder release.

## Further Reading
- Superset official docs: [Dashboards](https://superset.apache.org/docs/creating-charts-dashboards/dashboards/), [Datasets](https://superset.apache.org/docs/creating-charts-dashboards/datasets/), [SQL Lab](https://superset.apache.org/docs/creating-charts-dashboards/sql-lab/).
- Accessibility basics: [W3C contrast guidance](https://www.w3.org/TR/WCAG21/#contrast-minimum).

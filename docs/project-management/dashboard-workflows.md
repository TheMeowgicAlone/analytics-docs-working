# Dashboard Workflows (SFG)

How stakeholders and engineers collaborate on Superset dashboards, from request to release.

## Stakeholder Workflow
- Capture the ask using the [Dashboard Feature Request](../templates-and-checklists/dashboard-feature-request.md) template with problem statement, audience, and phase goal (minimum/operational/acceptable/exceptional).
- Specify success metrics, guardrails, and acceptance criteria (time zone defaults to UTC); include any privacy constraints or segments to exclude.
- Note data sources (e.g., Discord Gateway events) and known gaps so engineering can size the work.
- Share the request link in the working channel and align on priority and target milestone.
- Participate in review: validate early mocks, confirm KPI definitions, and sign off once the Definition of Done is met.

## Engineer Workflow
- Intake: review the submitted feature request, confirm owner and priority, and flag dependencies (upstream data, roles, privacy review).
- Design: propose layout and filters that follow the [Superset Style Guidelines](../platform/superset-style-guidelines.md) and [Definition of Done](../platform/superset.md#dashboard-definition-of-done), referencing existing dashboards to stay consistent.
- Build: create or reuse datasets, check SQL into version control, and annotate refresh cadence, sensitivity, and row-level security needs.
- Validate: test filters, cross-filters, and performance; reconcile metrics against source systems (e.g., Grafana logs or warehouse queries) within agreed tolerances.
- Document and release: populate the [Dashboard Documentation Template](../templates-and-checklists/dashboard-template.md), update the change log, and run the relevant items from the [Release Checklist](../templates-and-checklists/release-checklist.md) before publishing to stakeholders.

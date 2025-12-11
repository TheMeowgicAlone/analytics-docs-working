# Dashboard Release Checklist

Use this checklist before sharing a Superset dashboard with SFG stakeholders. Confirm each item; if something is not applicable, call it out in the change log.

## Readiness
- Purpose, audience, phase (minimum/operational/acceptable/exceptional), and iteration status documented in the dashboard page.
- Owner, refresh cadence, sensitivity level, and access roles noted in the dataset and dashboard descriptions.
- SQL saved to version control or documented with parameters; virtual datasets avoid secrets.

## Data Quality
- KPI formulas, filters, and grains match the dashboard documentation; acceptance thresholds agreed with stakeholders.
- Metrics validated against source systems (e.g., Grafana logs, warehouse queries, Discord Gateway events) within tolerance.
- Time zone defaults to UTC unless specified; row-level security applied where needed.

## UX and Accessibility
- Default time/segment filters set; cross-filters tested for consistency.
- Chart titles, labels, and tooltips include units and time frames; avoid jargon.
- Colors meet contrast expectations; warm colors reserved for alerts/regressions.
- Layout fits on one screen where possible; highest-priority tiles/cards in the top-left.

## Performance
- Charts load within agreed thresholds; caching or sampling applied where necessary.
- No broken queries or empty states; drill paths and links work.

## Communication
- Change log updated (date, change, reviewer).
- Stakeholders notified with dashboard link, known limitations, and next planned improvements.

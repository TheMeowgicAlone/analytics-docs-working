# Superset Style Guidelines

Standards for building and shipping Superset dashboards at SFG. Use alongside the Dashboard Documentation Template and Feature Request template.

## Audience and Framing
- Determine the audience: promotion team, event hosting team, moderation team, dota division management, stakeholer

## Layout and Storytelling
- Put the highest-signal tiles in the top-left; decrease detail as you move right and down.
- Keep KPI cards on the first row; avoid scroll bars by limiting dashboards to ~9–12 visuals.

## Visual Choices
- Keep numbers readable: at most 3–4 significant digits. 
- Keep it one page.
- Remove unnecessary data labels; rely on tooltips when values can be read from bars/lines.
- Sort charts to emphasize important. Most important = top left.

## Filters and Interactions
- Provide global time and primary segment filters with safe defaults (30–90 days)
- Prefer one dashboard with flexible time controls over separate “last 30 days,” weekly, and quarterly variants; create distinct dashboards only when audiences or workflows diverge.
- Use cross-filters carefully; verify they maintain context and do not break linked visuals.

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
- `<Domain>`: `<The question the dashboard answers>`

#### Examples
- Dota Program: Are members playing enough matches by ticket and over time?
- Dota League: Who is winning and who are the outliers?
- Dota League Captains: Which captains are the most successful?
- Data Hygiene: Which player records and Steam accounts are incomplete?
- Discord Activity: When is the server alive and who is engaging?
- Cohorts: How are member segments growing, shrinking, and converting?

### Charts
- `<Metric>` by `<Dimension>` (`<Timeframe>`)
# Time Off Report Research and Recommended Redesign

This document captures business-intelligence dashboard guidance from vendor
documentation and applies it to a redesigned Time Off Report experience.

## Research Basis

This recommendation set is based on the following primary references reviewed on
March 31, 2026:

- Microsoft Learn, "Tips for designing a great Power BI dashboard"
  - https://learn.microsoft.com/en-us/power-bi/create-reports/service-dashboards-design-tips
- Microsoft Learn, "Design Power BI reports for accessibility"
  - https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-creating-reports
- Tableau Help, "Best Practices for Effective Dashboards"
  - https://help.tableau.com/current/pro/desktop/en-us/dashboards_best_practices.htm
- Tableau Blueprint, "Visual Best Practices"
  - https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm
- PatternFly, "Usage and behavior"
  - https://www.patternfly.org/design-foundations/usage-and-behavior/
- Atlassian Design System, "Forms"
  - https://atlassian.design/patterns/forms

These sources are consistent on the main themes: keep dashboards focused on
one-screen comprehension, place the highest-value information first, limit the
number of primary visuals, use progressive disclosure for advanced controls, and
build accessibility directly into the layout.

The current flow appears to be:

1. Select a department.
2. Review the report for that department, then use filters and user-level views.

That structure works, but it splits a single analytical task across two screens. For this report, the better BI pattern is a one-screen dashboard with the department selector in the page header and the detail tools progressively disclosed only when the user needs them.

## Executive Recommendation

The recommended redesign is a single-screen analytical dashboard with:

- Persistent header filters for department, date range, and status
- A KPI strip above the fold for current-state monitoring
- Two high-value analysis visuals in the main body
- A sortable employee table on the same page
- Inline drill-down through a detail drawer instead of page-to-page navigation

This is the best fit because the Time Off Report is an operational monitoring
tool, not a wizard. Users are trying to answer staffing and leave questions
quickly, so the interface should optimize for scanability, comparison, and
follow-up investigation without breaking context.

## What Current BI Guidance Recommends

### 1. Keep the dashboard on one screen and lead with the most important information

Microsoft recommends treating a dashboard as an overview, keeping it uncluttered, and placing the highest-level information at the top left so the page reads from summary to detail. Tableau similarly recommends a logical flow, a Z-layout, clear white space, and larger visual weight for KPIs and summary views.

Applied to Time Off Report:

- Use a single report canvas instead of a selection step followed by a separate results step.
- Keep summary metrics and the main trend visuals above the fold.
- Let the page scan from left to right and top to bottom: filters, KPI summary, trends, breakdowns, employee-level detail.

### Summary mapping from research to UI decisions

| Research theme | Recommendation for this report | Why it matters |
| --- | --- | --- |
| One-screen overview | Replace the two-step flow with a single report canvas | Reduces context switching and supports at-a-glance monitoring |
| Top-left information hierarchy | Put title, freshness, and KPIs first | Matches normal reading/scanning behavior |
| Limited number of visuals | Use two primary charts in the main analysis row | Keeps the page readable and prevents dashboard clutter |
| Progressive disclosure | Collapse advanced filters by default | Avoids turning the dashboard into a long form |
| Accessible structure | Use visible labels, contrast, alt text, and logical tab order | Keeps the report usable for keyboard and assistive-tech users |
| Device-aware layout | Stack sections on smaller screens instead of shrinking everything | Prevents unusable dense layouts on tablet and mobile |

### 2. Use the report as an overview; let deeper detail come through drill-downs

Microsoft notes that dashboards should show current-state overview information and rely on underlying detail views for deeper investigation. Tableau recommends obvious interactivity through filters, tooltips, and actions.

Applied to Time Off Report:

- Show department-level status immediately after selection.
- Keep the employee-level table on the same page, but below summary visuals.
- Open employee or team detail in an inline drawer or expandable row instead of routing users to another step.

### 3. Prefer chart types that support comparison and trend reading

Microsoft explicitly recommends bar and column charts for comparison, warns against 3D visuals, and says pie or donut charts are only appropriate for small-category part-to-whole cases. Tableau’s chart guidance maps bars to category comparison, lines to trends over time, bullet charts to goal tracking, heatmaps to relationship density, and Gantt-style views to durations over time.

Applied to Time Off Report:

- Use KPI cards for current totals.
- Use a line or area chart for absence trend over time.
- Use horizontal bars for leave by type, leave by team, or leave by approval status.
- Use a timeline or Gantt-like visual for upcoming leave windows.
- Avoid pie charts unless the report has very few leave types and the part-to-whole question is the main point.

### 4. Use progressive disclosure to reduce clutter

PatternFly recommends expandable sections to support progressive disclosure by hiding additional page or form content until it is needed. Atlassian similarly recommends progressive disclosure for long or conditional workflows, and reserves multi-step forms for cases where fields naturally belong to separate stages.

Applied to Time Off Report:

- Keep only the primary filters visible by default: department, date range, leave status, leave type, and search.
- Move advanced filters into an expandable section or side panel: manager, location, tenure band, policy type, approval state, overlapping leave toggle.
- Do not keep the entire report behind a mandatory stepper unless the data cannot be loaded without that first action.

### 5. Make accessibility part of the dashboard structure, not an afterthought

Microsoft’s accessibility guidance recommends at least 4.5:1 contrast for text, meaningful titles and labels, alt text for non-decorative visuals, consistent slicer placement, and a tab order that matches visual reading order.

Applied to Time Off Report:

- Keep all primary insights visible without hover.
- Use tooltips only for secondary detail.
- Keep filter placement consistent across states.
- Ensure the keyboard order follows header filters, KPI cards, charts, table, and detail drawer.

## Recommended Layout for the New Time Off Report

Replace the two-step flow with a single responsive dashboard page.

### Header band

- Report title: `Time Off Report`
- Short subtitle describing scope and freshness, for example `Department-level availability, upcoming leave, and employee detail`
- Primary controls on the same row:
  - Department selector
  - Date range selector
  - Leave status selector
  - `Reset filters`
  - `Export`

### Optional advanced filters

- Place these in a collapsed `Advanced filters` section directly beneath the header.
- Show applied-filter chips when collapsed so users can see active constraints without opening the panel.

### KPI row

Use 4 to 5 cards with strong visual hierarchy:

- Employees on leave today
- Upcoming approved leave in next 30 days
- Pending requests
- Departments or teams with overlap risk
- Average days off per employee in selected period

Each card should include:

- The number
- Small context text
- Delta vs previous comparable period when available

### Main analysis row

Use two strong visuals:

- Left: absence trend over time
- Right: leave by type or leave by team

This row answers: `How much leave is happening, and where is it concentrated?`

### Secondary analysis row

Use visuals that support planning:

- Upcoming leave timeline by employee or team
- Calendar heatmap for daily concentration
- Approval-status distribution if approvals are operationally important

Pick two, not all three, to keep the screen readable.

### Employee detail section

Use a sortable, filterable table below the visuals.

Recommended columns:

- Employee
- Team
- Manager
- Leave type
- Start date
- End date
- Days
- Status
- Coverage risk or overlap flag

Recommended table behavior:

- Sticky header
- Default sort by upcoming start date or highest overlap risk
- Row click opens a right-side detail drawer with employee history and request context

## Recommended Interaction Model

The page should answer different user intents in one place:

- Monitoring: KPI cards and trend chart answer what is happening now
- Analysis: breakdown chart and timeline answer where the impact is concentrated
- Action: the table and detail drawer answer which employee or team needs attention

Recommended default behavior:

- Load the last-used department when possible
- Default the date filter to a practical operational window such as the next 30
  or 60 days
- Refresh summary cards and charts immediately when primary filters change
- Preserve the current table scroll position when a user adjusts a chart filter
- Allow export of the filtered table and summary state

## Recommended UX Changes

### Replace the explicit step flow with immediate context

If the user must choose a department first, keep that control prominent but persistent in the page header. Once selected, the user should remain on the same report canvas while the rest of the page updates.

Why this is better:

- It removes an avoidable page transition.
- It keeps filters and results in the same mental model.
- It matches the BI pattern of overview first, detail second.

### Prioritize common questions

The default view should answer these questions without scrolling far or opening extra controls:

- Who is out now?
- What is coming up soon?
- Which teams are most affected?
- Where are there approval or coverage risks?

### Keep interaction obvious

Use visible labels such as:

- `Department`
- `Date range`
- `Advanced filters`
- `View employee details`

Avoid hidden drill paths that depend on users guessing they can click a chart element.

### Reduce filter fatigue

Do not put every possible filter at the top of the page. That turns the report into a form instead of a dashboard.

Recommended filter tiers:

- Always visible: department, date range, status, leave type, employee search
- On demand: location, manager, policy, overlap-only, tenure, request source

### Empty, loading, and error states

The current ticket describes the happy-path layout, but the redesign should also
handle operational states cleanly:

- Loading state: show skeleton cards, chart placeholders, and disabled export
  until filters resolve
- Empty state: explain why no rows are shown and keep the active filter chips
  visible so users can diagnose the empty result
- Error state: keep filter controls available, show a retry action, and avoid
  replacing the whole page with a generic failure message

### Make state changes visible

When a filter changes:

- Refresh KPI cards first
- Preserve user context if they are scrolled into the employee section
- Show the active filter chips near the top of the page
- Include `Last refreshed` text near the title or export action

## Recommended Visual Design Direction

### Structure

- Use a light neutral canvas with card surfaces
- Keep generous spacing between sections
- Use a restrained accent color for interactive states and alerts

### Typography

- Strong page title
- Medium-weight section headers
- Large bold KPI numerals
- Small muted explanatory text for comparison periods and data freshness

### Color

- Neutral baseline palette for standard values
- One accent color for interactive selections
- One warning color for overlap or staffing risk
- One success color for approved or healthy coverage states

Do not use color as the only signal. Pair color with labels, icons, or badges.

## Accessibility Checklist for Implementation

- Ensure text contrast meets at least 4.5:1 against the background
- Add clear titles and alt text to non-decorative visuals
- Keep slicers and filters in a consistent position across report states
- Make the tab order follow the visual reading order from header to detail
- Do not hide critical information behind hover-only tooltips
- Do not rely on color alone for approval state, overlap risk, or warning states

## Wireframe Recommendation

The repository includes an example SVG mockup here:

- `elixir/docs/assets/time_off_report_layout_example.svg`

The mockup demonstrates:

- persistent header filters
- a collapsed advanced-filter affordance
- KPI cards above the fold
- trend and breakdown visuals in the main analysis area
- a detailed employee table with a right-side detail drawer

## Recommended Acceptance Criteria for a Future Implementation

- The department can be changed without leaving the report page.
- The default state shows KPI summary plus at least two analytical visuals before the employee table.
- Advanced filters are hidden by default but clearly discoverable.
- The employee table is sortable and supports opening row-level detail without page navigation.
- Critical insights remain visible without relying on tooltips.
- The desktop layout fits the primary content on one screen without unnecessary scrolling.
- The layout degrades cleanly on tablet and mobile by stacking sections in reading order.

## Suggested Future Enhancements

- Add saved filter presets for common department views
- Add comparison mode for current period versus prior period
- Add staffing-threshold annotations on the timeline and trend chart
- Add export variants for summary-only and employee-detail datasets

## Sources

- Tableau, `Visual Best Practices`, accessed March 31, 2026: https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm
- Microsoft Learn, `Tips for designing a great Power BI dashboard`, last updated October 20, 2025: https://learn.microsoft.com/en-us/power-bi/create-reports/service-dashboards-design-tips
- Microsoft Learn, `Design Power BI reports for accessibility`, last updated January 1, 2026: https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-creating-reports
- PatternFly, `Expandable section`, accessed March 31, 2026: https://v4-archive.patternfly.org/v4/components/expandable-section/design-guidelines/
- Atlassian Design System, `Forms`, long-form and progressive-disclosure guidance, accessed March 31, 2026: https://atlassian.design/patterns/forms/

## Notes on Interpretation

Some recommendations in this document are direct applications of the cited guidance to a Time Off Report use case rather than exact quotes from those sources. In particular, the recommendation to replace the current two-step flow with a single dashboard page is an inference based on the sources' emphasis on overview-first dashboards, one-screen storytelling, and progressive disclosure for optional controls.

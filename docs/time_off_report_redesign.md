# Time Off Report Redesign Research

## Purpose

This document summarizes recommended dashboard patterns for redesigning Symphony's Time Off Report. The current flow described in Jira is:

1. Select a department.
2. Review time-off data for that department, with filters and user-level detail.

The recommended redesign should move this into a clearer operational dashboard pattern: one page, decision-first, with summary metrics at the top and employee-level detail below.

## Research Summary

The strongest recurring guidance across BI platforms is consistent:

- Start from the audience and their first decision, not from the raw dataset.
- Put the highest-value metrics in the most visible area.
- Limit the number of competing visuals on a page.
- Use filters deliberately and make interactivity obvious.
- Keep detail available, but behind a clear summary layer.
- Design for accessibility, keyboard navigation, and readable contrast.

For this report, the audience is likely department managers, HR, and operations users. That makes this an operational dashboard, not an executive dashboard. The page should answer these questions quickly:

- How much planned time off exists in the selected time period?
- Which employees or teams create coverage risk?
- What trends, spikes, or seasonal clusters are present?
- Which records need action or follow-up?

## Recommended BI Layout Pattern

### Best-fit pattern

Use a single-page operational dashboard with progressive disclosure:

- Header with page title, selected date range, and last refresh time.
- Compact filter rail for department, time period, leave type, status, and manager.
- KPI strip for high-signal metrics.
- Trend and distribution visuals in the middle band.
- Employee table as the primary detailed view.
- Optional side panel or drawer for record-level detail instead of a separate step.

This layout fits BI best practices better than a step-by-step wizard because users often need to compare filters repeatedly. Requiring a separate selection screen adds friction and hides context.

### Why this pattern fits time-off reporting

- Department is a persistent context filter, not a workflow step.
- Managers need both aggregate and user-level answers on the same page.
- Time-off data is naturally split into summary, trend, and exception views.
- A data grid remains essential because staffing decisions often end with employee-level review.

## Recommended Information Architecture

### 1. Header

Include:

- Title: `Time Off Report`
- Subtitle with department name and reporting period
- Last data refresh timestamp
- Primary actions: export, reset filters

The header should confirm context immediately so users never wonder which department or time span they are viewing.

### 2. Filter rail

Recommended filters:

- Department
- Date range
- Leave type
- Approval status
- Manager or team
- Employee search

Recommendations:

- Keep filters in a fixed left rail on desktop.
- Collapse them into a drawer on smaller screens.
- Show active filter chips above the content area.
- Add `Reset` and `Apply` only if query cost requires it; otherwise use immediate filtering.

### 3. KPI strip

Show 4 to 6 metrics only:

- Employees on leave
- Total leave days
- Pending requests
- Overlapping absences
- Peak day this period
- Coverage risk count

Each KPI should support a small comparison, such as vs. prior period or vs. department average.

### 4. Visual analysis band

Recommended visuals:

- Trend chart: time off days by week or month
- Calendar heatmap: concentration of absences by date
- Breakdown chart: leave type or status distribution

This combination gives both time-based pattern recognition and categorical explanation.

### 5. Detail table

The table should remain the core working area. Recommended columns:

- Employee
- Team or manager
- Leave type
- Start date
- End date
- Duration
- Status
- Overlap or coverage flag

Recommended behavior:

- Sticky header
- Sorting on every column
- Inline search
- Row click opens side drawer with notes, approvals, and overlapping requests
- Conditional emphasis only for true exceptions such as pending approval or critical overlap

## Recommended UX Updates

### Replace the two-step flow

Current behavior appears to separate department selection from report analysis. Replace it with a single report page where department is the first filter and the page updates immediately. This reduces navigation cost and supports comparison across departments.

### Emphasize decision-making, not just reporting

The page should highlight exceptions first:

- overlapping absences
- pending approvals
- periods with high concentration of leave

This is more useful than showing only a generic list.

### Use details on demand

Do not overload the top of the page with every metric or filter combination. Keep the first view readable, then let users drill into an employee row or hover/select visuals for extra context.

### Make interactions explicit

Use labels such as `Filter by department`, `Select a date range`, and `Click a row for details`. BI tools consistently recommend obvious interaction cues because many users do not assume dashboards are interactive.

### Keep the page visually stable

Avoid layout jumps after filtering. Preserve panel sizes and table position so users can compare states without re-orienting.

## Accessibility and Usability Requirements

The redesign should include:

- Logical tab order matching visual reading order
- High color contrast
- Text labels or icons in addition to color
- Alt text for non-decorative visuals
- Consistent filter placement across views
- Keyboard access to filters, table, and detail drawer

For tables and charts, never rely on color alone to communicate risk or status. Pair it with icons, labels, or pattern differences.

## Proposed New Layout

### Desktop layout

1. Top header with title, department context, date range, and export action.
2. Left filter rail with persistent controls.
3. Top content row with 5 KPI cards.
4. Middle row with:
   - trend chart
   - calendar heatmap
   - leave type breakdown
5. Bottom full-width employee table.
6. Right-side detail drawer on row selection.

### Mobile or narrow layout

1. Header
2. Filter drawer trigger
3. KPI cards in a vertical stack or 2-column grid
4. One chart per row
5. Employee list with expandable rows

On mobile, do not place large visuals side by side.

## Example Wireframe

See the companion mockup image:

- `elixir/docs/images/time_off_report_layout.svg`

The wireframe shows a recommended operational dashboard layout with persistent filters, summary metrics, trend analysis, and employee-level details.

## Detailed Recommendations by Area

### Visual hierarchy

- Most important information belongs in the upper-left and first scan row.
- The page should lead with status and risk, then trend, then detail.
- Keep decorative elements minimal.

### Density

- Use a clean but information-rich layout.
- Prefer 2 to 3 visuals in the analysis band.
- Avoid adding more charts when a table already answers the question better.

### Labels and terminology

- Use straightforward labels such as `Employees on leave` instead of internal terminology.
- Expand abbreviations in visible UI.
- Show date units explicitly, for example `Leave days` or `Hours`.

### Table design

- Freeze the header row.
- Keep row height compact but readable.
- Right-align numeric values and dates only if the design system already supports it consistently.
- Support export from the detailed table, not only the whole report.

### Performance and resilience

- Load the KPI strip and table first.
- Defer less critical visuals if needed.
- Keep the number of page-level visuals limited to preserve readability and performance.

## Suggested Acceptance Criteria for the Actual Product Redesign

- Department selection is part of the main report page, not a separate step.
- Users can understand department context, date range, and refresh time without scrolling.
- Page highlights summary, trend, and exception information before raw detail.
- Employee-level detail remains accessible in a sortable table.
- Filters are consistent, visible, and easy to reset.
- Report meets basic accessibility expectations for contrast and keyboard navigation.

## Sources

These sources were used for the research direction:

- Tableau, "Best Practices for Effective Dashboards"  
  https://help.tableau.com/current/pro/desktop/en-us/dashboards_best_practices.htm
- Tableau, "Visual Best Practices"  
  https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm
- Microsoft Learn, "Design Power BI Reports for Accessibility"  
  https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-creating-reports
- Microsoft Learn, "Overview of Accessibility in Power BI"  
  https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-overview
- Microsoft Learn, "Best practices for creating mobile-optimized Power BI reports"  
  https://learn.microsoft.com/en-us/power-bi/create-reports/power-bi-create-mobile-optimized-report-best-practices

## Inference Notes

The proposed layout is an inference from the source guidance above applied to Symphony's described Time Off Report use case. Because the current screenshots were not present in this repository, the recommendations are based on the Jira description of the current two-step flow and the typical needs of operational time-off dashboards.

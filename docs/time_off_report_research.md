# Time Off Report Redesign Research

## Context

`RDSP-219` asks for research and a concrete recommendation for redesigning the Time Off Report. The current flow is:

1. Select department.
2. Review a department-scoped report with filters and employee-level data.

The main opportunity is to move from a sequential, utility-first flow to a single analysis surface that follows common BI dashboard patterns: quick orientation, visible filters, summary first, and detail on demand.

## Research Summary

The most consistent guidance across current BI and design-system sources is:

- Put the highest-value information in the upper-left and let the page flow from summary to detail.
- Keep dashboards focused; do not overload the first screen with too many views.
- Keep global filters visible and close to the content they affect.
- Give dense tables the largest share of horizontal space and pair them with search, sort, export, and pagination.
- Design explicitly for desktop and mobile rather than relying on uncontrolled auto-resize.
- Use clear labels, consistent spacing, and accessible table/filter behavior.

## Sources

- Tableau, "Best Practices for Effective Dashboards": https://help.tableau.com/current/pro/desktop/en-us/dashboards_best_practices.htm
- Tableau Blueprint, "Visual Best Practices": https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm
- Tableau, "Size and Lay Out Your Dashboard": https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm
- Microsoft Learn, "Tips for designing a great Power BI dashboard": https://learn.microsoft.com/en-us/power-bi/create-reports/service-dashboards-design-tips
- Microsoft Learn, "Best practices for creating mobile-optimized Power BI reports": https://learn.microsoft.com/en-us/power-bi/create-reports/power-bi-create-mobile-optimized-report-best-practices
- Microsoft Learn, "Design Power BI reports for accessibility": https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-creating-reports
- Carbon Design System, "Data table usage": https://carbondesignsystem.com/components/data-table/usage/
- Nielsen Design System, "Filter Bar": https://nielsendesignsystem.com/patterns/filter-bar

## What BI Guidance Means For This Report

### 1. Replace the two-step flow with one report workspace

Department selection should become the first and most prominent filter in a persistent filter bar, not a separate page step. In BI tools, users expect to refine the same analysis surface rather than leave the report context and re-enter it.

Recommended behavior:

- Open directly into the report.
- Preselect the user's last-used department when possible.
- Keep department as the first global filter.
- Update all summary cards, charts, and the employee table from the same filter state.

This reduces context switching and makes the report feel analytical rather than transactional.

### 2. Use an overview-to-detail layout

The best-fit layout pattern for a Time Off Report is "overview first, drill into detail second":

- Top: page title, date context, and filter bar.
- Upper content: 3-4 KPI cards for fast scanning.
- Middle content: 2 supporting visuals that explain the story.
- Lower content: the employee-level detail table.

This matches Tableau and Power BI guidance to emphasize the main signal first and move into deeper detail below it.

### 3. Keep filters visible, compact, and scannable

The filter model should prioritize the few questions managers repeatedly ask:

- Which department?
- What date range?
- What status or leave type?
- Which manager, team, or employee?

Recommended filter set:

- Department: required, primary.
- Date range: required, sticky.
- Leave status: approved, pending, rejected, canceled.
- Leave type: vacation, sick, personal, other.
- Employee search: text search, not a long dropdown.
- Optional advanced filters: manager, location, tenure band.

Recommended filter UX:

- Put the most-used global filters in a horizontal filter bar at the top.
- Show applied-filter chips below the bar.
- Include a single `Reset filters` action.
- Use auto-apply only if query latency is low; otherwise provide `Apply filters`.

### 4. Reserve the widest area for the data table

The detailed employee table is likely the primary working surface for HR and department managers. Carbon's guidance fits this well: the table should live in the main content area, with toolbar controls for search, filter refinements, export, and sorting.

Recommended table columns:

- Employee
- Team / manager
- Leave type
- Start date
- End date
- Days requested
- Days approved
- Status
- Balance after leave

Recommended table capabilities:

- Sort on all date, balance, and quantity columns.
- Sticky header.
- Horizontal scroll only as a last resort; hide less critical columns behind a column picker first.
- Row expansion or side panel for per-employee details instead of stuffing the grid with extra columns.
- Export current result set.
- Pagination plus total-result count.

### 5. Show two explanatory visuals, not a wall of charts

For this report, the strongest BI layout is a small number of high-signal visuals above the table:

- Time-off trend over time.
- Breakdown by leave type or approval status.

Optional third visual if the data supports it:

- Calendar-style heatmap for absence concentration by day or week.

Avoid adding many small charts. Tableau's current guidance is still to limit dashboard views because too many views reduce clarity and can also hurt performance.

### 6. Make the page understandable at a glance

The page should tell the user what they are looking at without requiring hover or training.

Recommended framing:

- Title: `Time Off Report`
- Subtitle: `Department, date range, and result count`
- KPI labels that answer manager questions directly:
  - `Employees on leave`
  - `Pending requests`
  - `Approved days`
  - `Remaining balance risk`

Use status colors sparingly and consistently:

- Neutral base palette for most UI.
- One accent color for selected filters and focus.
- Semantic colors only for status values.
- Do not rely on color alone; add text/icon cues.

### 7. Build for responsive behavior intentionally

This report is table-heavy, so desktop should be the primary layout. Mobile should not simply shrink the desktop table.

Desktop recommendation:

- 12-column grid.
- Filter bar spans full width.
- KPI row in 4 cards.
- Two visual panels beneath.
- Full-width table at the bottom.

Tablet recommendation:

- Filters wrap to two rows.
- KPI cards in 2x2 layout.
- Visuals stack vertically.
- Table shows reduced columns with quick row drill-in.

Mobile recommendation:

- Department and date range remain first.
- KPI cards stack vertically.
- One key chart only.
- Replace wide table with employee list cards plus drill-in detail view.

### 8. Accessibility should be part of the redesign, not a later fix

Power BI's accessibility guidance maps directly to this UI:

- Logical keyboard tab order from filters to summaries to table.
- Clear visible focus states.
- Text labels for every filter and icon-only action.
- Sufficient color contrast.
- Sort state announced in column headers.
- Descriptive empty states and no-results states.
- Alt text or accessible labels for visual summaries if this is rendered in a BI environment.

## Recommended Information Architecture

### Header

- Title
- Saved-view or last-updated text
- Export action

### Filter Bar

- Department
- Date range
- Leave status
- Leave type
- Employee search
- Reset / Apply

### KPI Row

- Employees on leave now
- Pending approvals
- Approved leave days in range
- Employees near low balance

### Analysis Row

- Left: time-off trend
- Right: leave type or approval status breakdown

### Optional Insight Row

- Calendar heatmap or balance-risk distribution

### Detail Section

- Toolbar with search, column settings, export
- Employee detail table
- Row drill-in for request history and balances

## Recommended UX Updates

- Merge department selection into the report instead of using a separate first step.
- Persist the last selected department and date range per user.
- Add applied-filter chips so users always know why data changed.
- Default the report to a meaningful date range, such as current quarter.
- Include result count and last refresh timestamp near the table title.
- Use empty-state guidance such as `No approved vacation found for Engineering in Q2`.
- Allow deep links or saved views for common department/date combinations.

## Example Layout Recommendation

The repository includes an SVG mockup here:

- `elixir/docs/assets/time_off_report_mockup.svg`

This mockup uses a single-page BI layout with:

- a persistent top filter bar,
- summary cards for rapid scanning,
- two mid-page analytical views,
- and a dominant employee table for operational detail.

## Concrete Recommendation

If only one redesign direction is taken forward, it should be:

1. Collapse the current two-step flow into one report page.
2. Put department and date range into a persistent filter bar.
3. Add four KPI cards at the top.
4. Keep only two supporting visuals above the table.
5. Make the employee table the primary lower section with strong table tools.

That direction is the clearest match to current BI product guidance and is the lowest-risk improvement to comprehension, speed, and usability.

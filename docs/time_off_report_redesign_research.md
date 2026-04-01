# Time Off Report Redesign Research

## Goal

Redesign the Time Off Report so it is easier to scan, filter, compare, and act on. The current flow appears to be:

1. Select a department.
2. Review a department-level report with filters and user-level data.

This document summarizes relevant business-intelligence layout guidance, translates it into UI and UX recommendations for this specific report, and includes a proposed layout mockup.

Related mockup: [time_off_report_wireframe.svg](./time_off_report_wireframe.svg)

## Research Summary

The strongest recurring guidance across Tableau and Microsoft Power BI is consistent:

- Put the highest-value information in the upper-left area because users usually scan from top-left toward the right and downward.
- Keep the dashboard on one screen when possible and avoid clutter or unnecessary scrolling.
- Limit the number of primary views and use clear grouping, whitespace, and alignment.
- Put frequently used filters on the canvas, not only in a hidden side panel.
- Use simple visuals for comparison and trends, then let users drill into detailed tables.
- Keep layout and filter patterns consistent across views and devices.
- Design for accessibility by keeping contrast high, avoiding color-only meaning, and making labels explicit.

## Recommended Layout Pattern

### Recommended structure

Use a two-step but single-experience layout:

1. Persistent filter rail at the top.
2. Summary strip with key metrics.
3. Main analytical views in the middle.
4. Detailed employee table at the bottom.

This keeps department selection visible at all times instead of making it feel like a separate isolated step.

### Recommended page anatomy

#### 1. Header

- Report title: `Time Off Report`
- Context subtitle: selected department, time window, and any active filters
- Secondary actions: export, reset filters, share link if the product supports it

#### 2. Filter rail

Recommended filters:

- Department
- Date range
- Time-off type
- Employee status
- Manager
- Location, if relevant in the data

Recommended interaction pattern:

- Put the most-used filters on-canvas as dropdowns, chips, or slicers.
- Show the active filter count.
- Add a single `Reset` action.
- Keep filter labels always visible.
- Avoid hiding all filters behind an icon unless screen width forces it.

#### 3. KPI summary strip

Recommended cards:

- Total employees in scope
- Employees with approved time off
- Total days requested
- Total days approved
- Upcoming absences in next 30 days
- High-risk coverage gaps or overlapping absences, if the data supports it

Reasoning:

- BI tools consistently use top-level cards to orient users before they read a table.
- This lets leaders answer "what is happening?" before exploring "who is affected?"

#### 4. Analytical middle section

Recommended primary views:

- Time-off trend by month or week
- Breakdown by time-off type
- Department or team comparison

Recommended chart choices:

- Use bars for comparisons.
- Use a line or area chart for trends over time.
- Avoid pie-heavy layouts and 3D charts.

For this report, two charts plus one compact comparison view is the right density. More than that will make the page harder to scan.

#### 5. Detail table

The detailed employee table should remain available, but it should be clearly treated as the detail layer, not the entire report.

Recommended columns:

- Employee
- Team
- Manager
- Time-off type
- Requested days
- Approved days
- Upcoming dates
- Status
- Remaining balance, if relevant

Recommended table behaviors:

- Sticky header
- Sorting on key columns
- Search by employee name
- Row expansion or drill-through for detailed absence history
- Conditional emphasis only for exceptions, such as overlap risk or denied requests
- Pagination or virtualization if the dataset is large

## Specific UI Recommendations

### 1. Replace the hard step break with persistent context

Instead of making department selection feel like a separate screen, keep department in the filter rail so users can change scope without restarting the workflow.

Expected benefit:

- Faster comparison between departments
- Lower interaction cost
- Less orientation loss

### 2. Lead with summary, not raw rows

The current description suggests the existing experience emphasizes data display by user. That is useful, but it should come after summary metrics and trends.

Expected benefit:

- Better executive scanning
- Better support for manager-level decisions
- Less dependence on reading the whole table

### 3. Make filters visible and compact

Use on-canvas filters with a predictable order:

- Scope filters first: department, location, manager
- Time filters second: year, quarter, date range
- Category filters third: time-off type, status

Expected benefit:

- Users can predict where filters live
- Lower training burden
- Better mobile adaptation

### 4. Add explicit empty, loading, and no-result states

Recommended states:

- `No department selected`
- `No time-off records match current filters`
- `Data last refreshed on <timestamp>`

Expected benefit:

- Less ambiguity
- Better trust in the data

### 5. Use visual hierarchy for exceptions

Most records are routine. Visual emphasis should be reserved for exceptions:

- overlapping absences
- high upcoming absence counts
- denied or pending approvals
- low remaining balance

Expected benefit:

- Better signal-to-noise ratio
- Faster operational triage

### 6. Keep labels plain and business-facing

Use labels like:

- `Upcoming absences`
- `Approved days`
- `Employees with scheduled leave`

Avoid internal or technical field names unless users already work with them daily.

### 7. Support accessibility from the start

- Meet readable contrast targets.
- Do not rely on color alone for status.
- Use clear headings and predictable tab order.
- Provide alt text if the final implementation includes explanatory images.
- Keep visual density controlled so the table remains usable with zoom.

## Recommended UX Flow

### Desktop flow

1. User lands on report.
2. User sees default department or selects one immediately from the top filter rail.
3. KPI strip updates.
4. Mid-page charts explain trend and composition.
5. User scans the table for individual employees.
6. User drills into a row or changes filters without leaving the report.

### Mobile or narrow layout

Use the same order, stacked vertically:

1. Filters
2. KPI cards
3. Trend chart
4. Breakdown chart
5. Table or simplified list

On small screens, collapse less-used filters into a secondary drawer, but keep department and date range visible.

## Suggested Visual System

- Neutral base with one strong accent color
- Green, amber, and red only for real status meaning
- Soft section backgrounds or cards to separate summary from detail
- Consistent spacing grid
- Moderate typography contrast between section titles, KPIs, and row data

This should feel more like an operational dashboard than a plain exported table.

## Proposed Layout

The mockup in [time_off_report_wireframe.svg](./time_off_report_wireframe.svg) shows:

- a persistent title and action row
- an on-canvas filter band
- six KPI cards
- three analysis panels
- a detailed employee table

## Why This Layout Fits a Time Off Report

- Managers need immediate staffing and coverage signals, not only row-level records.
- HR and operations users still need detailed employee rows, but usually after narrowing scope.
- Department changes are common, so scope selection should stay persistent.
- Time-off data naturally supports a summary-to-detail flow: totals, trend, composition, then people.

## Implementation Notes For A Future Product Pass

- Default the report to the user's department when appropriate.
- Keep URL state for filters so links are shareable.
- Use sticky filters and sticky table headers on long pages.
- If comparison across departments matters, consider a toggle between `Single department` and `Cross-department comparison`.
- If the system already tracks approvals, add `Pending approvals` as a first-class card.

## Source Notes

The recommendations above are based on the following sources and a small amount of product-specific inference:

- Tableau, "Best Practices for Effective Dashboards": top-left emphasis, limited views, visible filters, device-aware sizing. https://help.tableau.com/current/pro/desktop/en-us/dashboards_best_practices.htm
- Tableau, "Visual Best Practices": logical flow, Z-layout, whitespace, simplified design. https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm
- Tableau, "Size and Lay Out Your Dashboard": alignment, grids, tiled layouts, size strategy. https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm
- Microsoft Learn, "Tips for Designing a Great Power BI Dashboard": one-screen storytelling, top-left prioritization, use of summary cards, avoiding clutter, choosing readable visuals. https://learn.microsoft.com/en-us/power-bi/create-reports/service-dashboards-design-tips
- Microsoft Learn, "Overview of visualizations in Power BI": slicers for on-canvas filtering, cross-filtering, drill-through, themes, and choosing visuals based on space and audience. https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualizations-overview
- Microsoft Learn, "Design Power BI Reports for Accessibility": contrast, explicit labeling, consistent slicer patterns, avoiding color-only meaning. https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-creating-reports

Inference:

- The proposed Time Off Report layout adapts general BI dashboard guidance to an HR operations use case where users need both monitoring and employee-level detail.

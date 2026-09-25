# Webenza WFO Intelligence Dashboard

A browser-based dashboard for analysing **Work From Office (WFO) compliance, attendance patterns, and employee leave** from Excel workbooks.

Designed for HR teams and managers, it combines employee master records with monthly attendance data to generate interactive summaries, employee profiles, and exportable reports.

> Single HTML application. No backend, database, package installation, or build process required. Spreadsheet processing runs in the browser; external libraries and fonts require network access to load.

## Features

| Section | Capabilities |
| --- | --- |
| Executive Overview | Workforce counts, aggregate compliance, department comparisons, and monthly attendance trends. |
| WFO Compliance | Expected versus actual attendance, variance, and employee filters. |
| Attendance Behaviour | Weekday patterns and rule-based monthly consistency classifications. |
| Leave Intelligence | Leave breakdowns and Monday/Friday, long-weekend, and sandwich-leave indicators. |
| Risk Dashboard | Rule-based attendance and leave review flags. |
| Manager & Team | Team summaries and copyable manager email drafts. |
| Employee Drilldown | Employee profiles, attendance calendars, and printable reports. |
| Data Quality | Sheet parsing audit, unknown codes, unmatched records, and methodology notes. |

## Quick Start

Download `Webenza WFO Dashboard.html` and open it in a modern browser with JavaScript enabled.

| Step | Action |
| --- | --- |
| 1 | Open the HTML file. |
| 2 | Select the Employee Master Database workbook. |
| 3 | Select the Attendance Workbook. |
| 4 | Review the WFO policy configuration. |
| 5 | Click **Generate Dashboard**. |
| 6 | Check **Data Quality** before interpreting or exporting results. |

An internet connection is needed initially to load dependencies. Fully offline startup requires bundling the external scripts and fonts locally.

## Input Requirements

The file selectors accept `.xlsx` and `.xls` workbooks.

### Employee Master Database

Only the **first worksheet** is read. Place the header within the first five rows. Use unique employee IDs that match the attendance workbook; IDs are trimmed and converted to uppercase.

| Suggested header | Purpose |
| --- | --- |
| Employee ID | Matches attendance records. |
| Employee Name | Displays the employee name. |
| Department | Supports grouping and policy assignment. |
| Work Location | Determines location-specific rules. |
| Work Mode | Identifies remote arrangements. |
| Reporting Manager | Groups employees into teams. |
| Date of Joining | Adjusts the effective WFO start date. |
| Employment Status | Supports inclusion and exclusion rules. |
| Designation | Appears in profiles and reports. |

### Attendance Workbook

Use one worksheet per month with employee name, employee ID, and day columns labelled `1` through `31`, as applicable. Keep the header within the first four rows.

Name sheets with a month and year, for example **Jan 2026**. Without a year in the sheet name, the parser uses the browser's current year.

### Attendance Codes

| Code | Current treatment |
| --- | --- |
| `P` | Office presence on eligible working days. |
| `WFH` | Work from home, tracked separately. |
| `EL` | One day added to EL. |
| `SL` | One day added to SL. |
| `LOP` | One day added to LOP. |
| `COF` | One day added to COF. |
| `HA` | Half a day added to EL. |
| `HDL` | Half a day added to LOP. |
| `ML` | One day added to maternity leave. |
| `MAT` | One day added to maternity leave. |
| `FH` | One day added to other leave. |
| `VH` | One day added to other leave. |
| `H` | Marks the date as a holiday globally. |
| `R` | Recognised but not counted as office presence or leave. |
| `-` | Missing attendance data. |
| Blank | Missing attendance data. |

Confirm these mappings against your organisation's definitions.

## Policy Configuration

| Setting | Default |
| --- | --- |
| Include probation employees | Enabled |
| Include interns | Disabled |
| Exclude Kolkata from WFO | Enabled |
| Include partially remote employees under a two-day policy | Enabled |

Location- and department-specific schedules are defined in `parseMaster()`. Review these before using the dashboard for another organisation.

## Calculation Method

Expected WFO is the rounded result of working days multiplied by required office days per week, divided by five.

Actual WFO counts `P` entries on Mondayâ€“Friday, excluding detected holidays and dates before the employee's effective start.

Compliance is actual WFO divided by expected WFO, multiplied by 100. Aggregate compliance uses total actual and expected days, not the average of individual percentages.

| Status | Compliance |
| --- | --- |
| Exceeded | At least 100% |
| Met | At least 90%, below 100% |
| Partial | At least 75%, below 90% |
| Below | Below 75% |
| Not Applicable | Employee excluded from WFO requirements |

Mandatory-day adherence is evaluated separately in calendars and manager email drafts.

## Exports

| Output | Behaviour |
| --- | --- |
| Compliance CSV | Exports all WFO-eligible employees, irrespective of table filters. |
| All Employees CSV | Exports employees retained after master-data filtering. |
| Employee PDF | Opens a printable report; choose **Print / Save as PDF**. |
| Manager Email | Generates a copyable draft; does not send email. |

Allow pop-ups for reports. Clipboard copying may require permission or a secure context.

## Technology

| Component | Technology |
| --- | --- |
| Interface | HTML and CSS |
| Application logic | Vanilla JavaScript |
| Excel parsing | SheetJS 0.18.5 |
| Charts | Chart.js 4.4.1 |
| Typography | Google Fonts |
| File reading | Browser FileReader API |

## Privacy and Responsible Use

Workbooks are processed in browser memory. The supplied application does not implement a server upload flow or persistent data storage. Refreshing requires selecting the files again.

External scripts and fonts generate network requests. Review dependencies before processing confidential HR information. Do not commit real employee workbooks or exported reports to a public repository.

**Review flags are not evidence of misconduct, productivity, or medical conditions.** Validate approved leave, accommodations, WFH exceptions, and missing records before taking action. Protected leave must not be treated as an adverse performance indicator.

## Known Limitations

| Area | Current behaviour |
| --- | --- |
| Partial months | Future day columns can increase expected WFO; headline calculations do not stop at today. |
| Approved leave | Does not automatically reduce expected WFO. |
| Holidays | An `H` entry in any record excludes that date globally. |
| Consistency | Months with zero office presence are absent from the consistency input. |
| CSV output | Zero compliance exports as `N/A`; embedded quotes and formula-like values need additional handling. |
| Input safety | Spreadsheet text is inserted into HTML without comprehensive escaping; use trusted workbooks. |

## Development

Styles, application logic, and interface are contained in the HTML file. Test changes with anonymised workbooks, particularly date handling, policy calculations, filtering, and exports. This README describes source behaviour, not a completed runtime test or security audit.

## Licence

No licence declaration was identified in the supplied source. Add an appropriate `LICENSE` file and confirm rights to included branding before public distribution.

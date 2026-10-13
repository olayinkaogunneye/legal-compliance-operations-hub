# Testing log and project status

## Current status

The reporting and email path has been tested successfully. The scheduled cloud flow is enabled. The first unattended recurrence and the no-match case have not yet been independently confirmed.

## Verified tests

| Area | Evidence / observed result | Status |
|---|---|---|
| Excel connector | `List rows present in a table` returned data from the OneDrive workbook; the action showed HTTP status 200. | Passed |
| Filter array | An inspected run returned six requests matching the rule. | Passed for the inspected data and date |
| Rule behaviour | Results included open High-priority records and an overdue Medium-priority record, while no Closed records appeared in the inspected output. | Passed for inspected run |
| HTML table | Table was configured with Request ID, Request Type, Due Date, Priority, Status and Assigned To. Its input is `body('Filter_array')`. | Passed in corrected test |
| Email delivery | One email arrived containing six filtered request rows after the unnecessary `For each` loop was removed. | Passed for test run |
| Summary structure | HTML table custom column values were entered as expressions to avoid per-row dynamic content causing an unnecessary loop. | Corrected and test-run |
| Schedule | Weekday recurrence at 09:00 with `Central Europe Standard Time`; scheduled flow is enabled. | Configured; awaiting natural scheduled-run verification |

## Example result from one inspected run

The following six records appeared in the Filter array output inspected on 12 October 2026. This is evidence from that run, not a fixed result count for future runs.

| Request ID | Priority | Status | Due date | Why included |
|---|---|---|---|---|
| REQ-002 | High | In Progress | 2026-10-09 | High priority; overdue on the inspected date |
| REQ-003 | Medium | In Progress | 2026-10-10 | Overdue on the inspected date |
| REQ-006 | High | In Progress | 2026-10-07 | High priority; overdue on the inspected date |
| REQ-010 | High | New | 2026-10-11 | High priority; overdue on the inspected date |
| REQ-012 | High | New | 2026-10-17 | High priority, though not overdue on the inspected date |
| REQ-014 | High | In Progress | 2026-10-15 | High priority, though not overdue on the inspected date |

## Remaining validation

- [ ] Confirm the first unattended scheduled run occurs at the configured weekday time.
- [ ] Confirm the scheduled run sends at most one email and only when qualifying requests exist.
- [ ] Test a no-match scenario and confirm the False branch sends no email.
- [ ] Review the run history after the natural scheduled run and add the date/result below.
- [ ] Add project screenshots after redacting personal and environment details.
- [ ] Add a clean synthetic CSV, if intended for publication, after data review.

## Scheduled-run evidence log

Add a new entry when verified:

| Date/time (Prague) | Run outcome | Matching rows | Email count | Notes |
|---|---|---:|---:|---|
| Pending | Pending | — | — | First unattended scheduled run has not yet been recorded here. |

## Operational note: two flows

The manual flow is retained as a test/reference flow. The scheduled flow is the operational flow. The flows are independent; manually running the test flow near a scheduled run can produce two separate summary emails. This is not the same as duplicate email actions inside one run.

# Testing log and status

## Verified

| Area | Evidence / result | Status |
|---|---|---|
| Excel connector | `List rows present in a table` successfully retrieved the OneDrive workbook/table; HTTP status 200. | Passed |
| Filter array | The rule excluded closed items and included open High-priority items or open overdue items. One inspected run returned six requests. | Passed for inspected run |
| HTML summary | A Create HTML table action was configured for Request ID, Request Type, Due Date, Priority, Status and Assigned To. | Configured |
| Manual email | An email was received from the tested workflow. | Email delivery passed; row contents require final consistency check |
| Scheduled flow | Initial scheduled run produced more emails than intended. A `For each` container was identified around reporting actions and removed during restructuring. | Not yet validated; keep disabled |

## Filter rule

A request should pass only if:

```text
Status != "Closed" AND (Priority == "High" OR Due_Date < Today in Prague)
```

In the inspected sample output, these six records matched on 12 October 2026:

| Request ID | Priority | Status | Due date | Why included |
|---|---|---|---|---|
| REQ-002 | High | In Progress | 2026-10-09 | High priority and overdue |
| REQ-003 | Medium | In Progress | 2026-10-10 | Overdue |
| REQ-006 | High | In Progress | 2026-10-07 | High priority and overdue |
| REQ-010 | High | New | 2026-10-11 | High priority and overdue |
| REQ-012 | High | New | 2026-10-17 | High priority, not overdue |
| REQ-014 | High | In Progress | 2026-10-15 | High priority, not overdue |

This is a record of the sample run, not a promise that the data will always return six rows; the result changes when the workbook or date changes.

## Required validation before scheduled use

1. Ensure the scheduled flow remains turned off while editing.
2. Check the flow structure: Recurrence → List rows → Filter array → Condition.
3. Ensure Condition uses `length(body('Filter_array'))` **is greater than** `0`.
4. Ensure only the True branch contains Create HTML table followed by Send an email (V2); False has no action.
5. Ensure Create HTML table's From is `body('Filter_array')`, not the original Excel `value` array.
6. Ensure no `For each`/`Apply to each` loop wraps the HTML table or email when sending one summary per run.
7. Run one controlled test and inspect the run history.
8. Confirm the email contains exactly the filtered records and is sent once.
9. Test the no-match case and verify no email is sent.
10. Only after successful tests, decide whether to enable the weekday schedule.

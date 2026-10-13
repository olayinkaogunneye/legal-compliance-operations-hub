# Setup and validation guide

This document records the resources needed to understand the prototype. It is not a one-click deployment guide: connections and cloud resources remain in the project owner's Microsoft environment.

## Required resources

- Power BI Desktop and the project report (kept local unless explicitly reviewed for publication).
- `Legal_Compliance_Requests.xlsx` with an Excel table named `tbl_LegalRequests`, stored in OneDrive.
- Power Automate access with an Excel Online (OneDrive) connection.
- An Outlook.com connection for the email action.
- A scheduled cloud flow using the recurrence and action structure described in [`architecture.md`](architecture.md).

## Power BI refresh

The existing Power BI report reads the locally synchronised copy of the OneDrive workbook. To reload data in Desktop:

1. Make sure the OneDrive file has synchronised to the computer.
2. Open the report in Power BI Desktop.
3. Select **Home → Refresh**.
4. Confirm that all visuals load without errors.

This prototype does not implement automatic Power BI Service refresh.

## Power Automate workflow configuration

The expected structure is:

```text
Recurrence
└── List rows present in a table
    └── Filter array
        └── Condition: length(body('Filter_array')) > 0
            ├── True
            │   ├── Create HTML table (From: body('Filter_array'))
            │   └── Send an email (V2)
            └── False
                └── No action
```

Set the recurrence to weekdays at 09:00 in `Central Europe Standard Time`. Confirm the flow is saved and enabled before expecting the next scheduled run.

## Validation checklist

- [ ] Excel action returns the expected table rows.
- [ ] Filter Array excludes all rows whose Status is `Closed`.
- [ ] A non-closed High-priority request qualifies even when its due date is in the future.
- [ ] A non-closed non-High request qualifies if its due date is before today.
- [ ] A request due today is not overdue under the strict comparison.
- [ ] HTML table input is `body('Filter_array')`, not the original Excel array.
- [ ] HTML table and email are not inside a `For each` loop.
- [ ] When qualifying rows exist, one email contains only those rows.
- [ ] When zero rows qualify, no email is sent.
- [ ] The first unattended recurrence is recorded as successful in run history.

Record evidence and dates in [`testing-and-status.md`](testing-and-status.md). Do not record a check as passed until you have observed the result.

## Security before sharing

Use your own connections when rebuilding the flow. Do not include passwords, tokens, environment identifiers, connection reference data, personal mailbox information, or unredacted run-history exports in GitHub. This repository documents the design rather than deploying the owner's Microsoft resources automatically.

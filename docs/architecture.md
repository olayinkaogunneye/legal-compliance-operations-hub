# Architecture and workflow design

## Components

### Excel in OneDrive

The fictional request workbook is the source for the prototype. The Power Automate Excel action reads the table `tbl_LegalRequests`.

Fields used by the rule/report include:

- `Request_ID`
- `Request_Type`
- `Due_Date`
- `Priority`
- `Status`
- `Assigned_To`

Other source fields can remain in the workbook but do not need to appear in the email summary.

### Power Automate filtering

The Filter array input is the `value` array returned by `List rows present in a table`:

```text
outputs('List_rows_present_in_a_table')?['body/value']
```

The rule is:

```text
@and(
  not(equals(item()?['Status'], 'Closed')),
  or(
    equals(item()?['Priority'], 'High'),
    less(
      formatDateTime(item()?['Due_Date'], 'yyyy-MM-dd'),
      formatDateTime(convertTimeZone(utcNow(), 'UTC', 'Central Europe Standard Time'), 'yyyy-MM-dd')
    )
  )
)
```

This expression assumes the field names and values match the workbook (including `Closed` and `High`). If the workbook changes, validate the expression against the current column names and values.

### Condition and email

The intended condition checks whether the filtered array contains records:

```text
length(body('Filter_array')) > 0
```

Only the True branch should contain these actions:

1. **Create HTML table**, with its From value set to:

   ```text
   body('Filter_array')
   ```

2. **Send an email (V2)** using the HTML table output.

The False branch should have no actions. Neither reporting action should be inside a `For each` loop when the intended behaviour is one summary email containing the complete filtered array.

## Power BI

The Power BI dashboard provides operational visibility (request volume, status, request type, overdue items, priority and assigned workload). It is a reporting layer; the Power Automate workflow is the action/notification layer.

## Intended schedule

The planned recurrence is weekdays at 09:00 in the `Central Europe Standard Time` time zone. This schedule is a design target only until the scheduled flow passes controlled validation.

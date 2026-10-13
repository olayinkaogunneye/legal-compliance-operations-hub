# Business rules

## Objective

Produce a daily email summary of requests that need attention while excluding completed requests.

## Selection rule

A request qualifies when:

```text
Status != "Closed" AND (Priority == "High" OR Due_Date < Today in Prague)
```

Both parts of the rule are required:

1. The request must not be closed.
2. At least one of these must be true:
   - Priority is `High`.
   - Due date is strictly before today's date in Prague.

## Examples

| Status | Priority | Due date | Should be included? | Reason |
|---|---|---|---|---|
| In Progress | High | Future date | Yes | Open and high priority |
| New | Medium | Yesterday | Yes | Open and overdue |
| In Progress | Low | Today | No | Open, but not High and not overdue |
| Closed | High | Yesterday | No | Closed requests are always excluded |
| Closed | Medium | Future date | No | Closed and does not qualify |

These are rule examples, not a fixed expected count. Actual results change when the workbook or current date changes.

## Date interpretation

The flow is configured to compare date values using the `Central Europe Standard Time` time zone for today's date. The due-date comparison is strict: a request due today is not overdue under this rule.

## Notification behaviour

- If one or more records qualify, create one HTML table from the filtered array and send one summary email.
- If zero records qualify, the Condition's False branch should do nothing and send no email.
- The workflow is designed to send one summary email per qualifying flow run, not one email per request.

## Out of scope

This prototype does not update the source workbook, assign tasks, email each assignee individually, validate the legitimacy of legal requests, or refresh Power BI automatically. It is a demonstration of operational monitoring and rule-based email notification, not a legal decision system.

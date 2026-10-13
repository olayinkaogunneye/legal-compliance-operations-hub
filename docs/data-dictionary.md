# Data dictionary

The project uses fictional Legal & Compliance request records held in `Legal_Compliance_Requests.xlsx`, in the Excel table `tbl_LegalRequests`.

The fields below are based on the columns observed in the workflow output. Confirm the workbook itself before changing the schema or adding new fields.

| Field | Meaning | Used by filter? | Included in email? |
|---|---|---:|---:|
| `Request_ID` | Unique-looking request reference used to identify the record | No | Yes |
| `Request_Type` | Category of request, for example Data Privacy or Regulatory Compliance | No | Yes |
| `Date_Received` | Date the request was received | No | No |
| `Due_Date` | Target date for handling the request | Yes | Yes |
| `Date_Closed` | Date the request was closed, if available | No | No |
| `Priority` | Priority classification, including High and Medium | Yes | Yes |
| `Status` | Current workflow status, including Closed, In Progress, or New | Yes | Yes |
| `Assigned_To` | Person or role responsible for the request | No | Yes |
| `Business_Unit` | Business unit associated with the request | No | No |

Power Automate may also return connector metadata such as `@odata.etag` and `ItemInternalId`. These are not business fields and should not be included in the human-readable email.

## Data-handling guidance

The published dataset should contain only synthetic records. Before publishing a CSV sample, check for hidden sheets, hidden rows or columns, comments, embedded files, names, email addresses, or other private information in the workbook. Publish only the fields needed to demonstrate the project.

Do not assume that any real-world legal/compliance dataset is safe to publish merely because some fields are omitted from the email table.

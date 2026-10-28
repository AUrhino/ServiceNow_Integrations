# LogicMonitor to ServiceNow Integration - Reusable Enhancement Prompt

## Objective

Review and enhance an existing LogicMonitor Custom HTTP integration JSON payload used to create or update incidents in ServiceNow.

The integration contains an HTML-formatted alert table in the `work_notes` field.

**Always return the entire updated JSON payload in a single copy-and-paste code block. Never provide only snippets, partial JSON or instructions to manually merge changes.**

## Requirements

### 1. Preserve existing integration functionality

- Retain all existing JSON fields unless explicitly instructed otherwise.
- Preserve LogicMonitor alert tokens and ServiceNow custom fields.
- Preserve `correlation_id`, `logicmonitor_alert_id`, `cmdb_ci`, `user_input` and other integration mappings.
- Do not invent LogicMonitor tokens, properties or ServiceNow fields.
- Ensure the final output is valid JSON with correctly escaped HTML.

### 2. HTML table layout

Use a professional LogicMonitor Envision design:

- Dark navy header: `#123047`
- White header text
- Font: Arial, sans-serif
- Font size: 13px
- Table width: 100%
- Collapsed borders
- White and light blue alternating data rows
- White: `#FFFFFF`
- Light blue: `#EAF3F9`
- First data row must be white.
- Use inline CSS for ServiceNow compatibility.

### 3. Table field order

The Alert Information table must contain the following rows in exactly this order:

| Order | Field | Token |
|---|---|---|
| 1 | Severity | `##LEVEL##` |
| 2 | Alert Status | `##ALERTSTATUS##` |
| 3 | Resource | `##HOST##` |
| 4 | LogicModule | `##DATASOURCE##` |
| 5 | Instance | `##INSTANCE##` |
| 6 | Datapoint | `##DATAPOINT##` |
| 7 | Alert Value | `##VALUE##` |
| 8 | Datapoint Description | `##DPDESCRIPTION##` |
| 9 | Notes and Properties | `##custom.alert.context##` |

Do not display Resource Group in the HTML table.

Keep `##GROUP##` elsewhere in the JSON if required by the existing integration.

### 4. Severity indicators

Display a severity indicator immediately after the severity text.

Desired appearance:

- Critical 🔴
- Error 🟠
- Warning 🟡
- Informational 🔵
- Clear 🟢

Use severity-specific icons only where conditional rendering is genuinely supported.

If the static LogicMonitor JSON template cannot conditionally select an icon from `##LEVEL##`, use a neutral diamond (`&#9670;`) and explain the limitation. Do not claim dynamic severity icons work without validation.

### 5. Notes and Properties

The final row must be labelled:

**Notes and Properties**

Default token:

`##custom.alert.context##`

Allow replacement with a specified LogicMonitor resource property using:

`##<DEVICEPROPERTYNAME>##`

Do not assume the property exists or resolves unless confirmed.

### 6. Other HTML elements

Retain:

- LogicMonitor Envision branding
- Infrastructure Monitoring Alert heading
- Alert ID and Resource summary
- Alert Status and Severity summary bar
- Alert Information table
- Alert Message section using `##MESSAGE##`
- View Alert in LogicMonitor hyperlink using `##AlertDetailURL##`
- Footer containing the alert ID

Preserve the original `[code]...[/code]` wrapper unless explicitly instructed to change it.

### 7. Validation

Before returning the result:

1. Validate JSON syntax.
2. Ensure HTML quotation marks are escaped correctly.
3. Confirm the table contains exactly nine data rows.
4. Confirm the row order matches the specification.
5. Confirm alternating white and blue backgrounds.
6. Confirm Resource Group is absent from the table.
7. Confirm Notes and Properties is the final row.
8. Preserve all existing integration mappings.
9. Do not claim that ServiceNow will render HTML without testing.
10. Do not replace valid LogicMonitor tokens with fabricated ones.

### 8. Required output

Always provide:

1. A short summary of changes.
2. **The complete updated JSON payload**, ready to paste into LogicMonitor.
3. A visual preview of the resulting HTML table where possible.
4. Any compatibility limitations or testing requirements.

Use Australian English and avoid em dashes.

## Sample input

Use the following sample as the starting point when no existing payload is supplied.

```json
{
  "short_description": "##ALERTID## ##HOST## ##DATASOURCE## ##DATAPOINT## ##VALUE##",
  "caller_id": "##servicenow.user##",
  "description": "LogicMonitor Alert: ##ALERTID## on ##HOST##\n\nResource: ##HOST##\nLogicModule: ##DATASOURCE##\nInstance: ##INSTANCE##\nDatapoint: ##DATAPOINT##\nValue: ##VALUE##\nSeverity: ##LEVEL##\nStatus: ##ALERTSTATUS##\n\n##MESSAGE##\n\nAlert Details: ##AlertDetailURL##",
  "company": "##servicenow.company##",
  "due_date": "##servicenow.duedatetime##",
  "severity": "##servicenow.severity##",
  "impact": "##servicenow.impact##",
  "logicmonitor_alert_id": "##ALERTID##",
  "correlation_id": "##INTERNALID##",
  "logicmonitor_alert_state": "##ALERTSTATUS##",
  "logicmonitor_alert_level": "##LEVEL##",
  "cmdb_ci": "##HOST##",
  "cmdb_ci_instance": "##INSTANCE##",
  "category": "Monitoring",
  "subcategory": "Logicmonitor Monitoring",
  "user_input": "##GROUP##&&##servicenow_table##",
  "urgency": "##LEVEL##",
  "work_notes": "[code]<table width=\"100%\" cellpadding=\"0\" cellspacing=\"0\" style=\"border-collapse:collapse;font-family:Arial,sans-serif;font-size:13px;color:#263746;\"><tr><td style=\"background-color:#123047;padding:18px 20px;color:#ffffff;\"><div style=\"font-size:11px;letter-spacing:1px;color:#b8d7e8;font-weight:bold;\">LOGICMONITOR ENVISION</div><div style=\"font-size:21px;font-weight:bold;margin-top:7px;\">Infrastructure Monitoring Alert</div><div style=\"font-size:13px;margin-top:7px;color:#d4e4ef;\">##ALERTID## | ##HOST##</div></td></tr><tr><td style=\"padding:14px 18px;background-color:#f1f5f9;border:1px solid #d9e2ea;\"><span style=\"font-weight:bold;\">Alert Status:</span> ##ALERTSTATUS## &nbsp;&nbsp;|&nbsp;&nbsp; <span style=\"font-weight:bold;\">Severity:</span> ##LEVEL##</td></tr><tr><td style=\"padding:18px 0 8px 0;font-size:15px;font-weight:bold;color:#123047;\">Alert Information</td></tr><tr><td><table width=\"100%\" border=\"1\" cellpadding=\"9\" cellspacing=\"0\" style=\"border-collapse:collapse;border-color:#d9e2ea;font-size:13px;\"><tr style=\"background-color:#123047;color:#ffffff;\"><th align=\"left\" width=\"32%\">Field</th><th align=\"left\">Value</th></tr><tr style=\"background-color:#ffffff;\"><td><b>Severity</b></td><td>##LEVEL## <span style=\"color:#64748b;font-size:15px;\">&#9670;</span></td></tr><tr style=\"background-color:#eaf3f9;\"><td><b>Alert Status</b></td><td>##ALERTSTATUS##</td></tr><tr style=\"background-color:#ffffff;\"><td><b>Resource</b></td><td>##HOST##</td></tr><tr style=\"background-color:#eaf3f9;\"><td><b>LogicModule</b></td><td>##DATASOURCE##</td></tr><tr style=\"background-color:#ffffff;\"><td><b>Instance</b></td><td>##INSTANCE##</td></tr><tr style=\"background-color:#eaf3f9;\"><td><b>Datapoint</b></td><td>##DATAPOINT##</td></tr><tr style=\"background-color:#ffffff;\"><td><b>Alert Value</b></td><td>##VALUE##</td></tr><tr style=\"background-color:#eaf3f9;\"><td><b>Datapoint Description</b></td><td>##DPDESCRIPTION##</td></tr><tr style=\"background-color:#ffffff;\"><td><b>Notes and Properties</b></td><td>##custom.alert.context##</td></tr></table></td></tr><tr><td style=\"padding:18px 0 8px 0;font-size:15px;font-weight:bold;color:#123047;\">Alert Message</td></tr><tr><td style=\"padding:14px;background-color:#f8fafc;border:1px solid #d9e2ea;border-left:4px solid #e09f3e;white-space:pre-wrap;\">##MESSAGE##</td></tr><tr><td style=\"padding:20px 0 12px 0;\"><a href=\"##AlertDetailURL##\" style=\"display:inline-block;background-color:#176b9a;color:#ffffff;text-decoration:none;padding:11px 17px;font-weight:bold;\">View Alert in LogicMonitor &#8594;</a></td></tr><tr><td style=\"border-top:1px solid #d9e2ea;padding:12px 0;color:#66788a;font-size:11px;\">Generated by LogicMonitor Envision | Automated Monitoring | Alert ID: ##ALERTID##</td></tr></table>[/code]",
  "u_alert_value": "##VALUE##",
  "u_alert_key": "##DPDESCRIPTION##",
  "u_datasource": "##DATASOURCE##",
  "u_datapoint": "##DATAPOINT##"
}
```

## New changes requested

Apply the following additional changes to the sample or supplied payload:

[DESCRIBE NEW CHANGES HERE]

Return the entire updated JSON payload, not just the modified sections.

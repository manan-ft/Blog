## How to Trigger a Function in ERPNext or Frappe through CLI

` bench --site <site_name> execute <path of function>`

For path of function, refer to github breadcrumb

For example
`bench --site staging execute frappe.email.doctype.auto_email_report.auto_email_report.send_daily`

In this `frappe.email.doctype.auto_email_report.auto_email_report` is path on Github, replace `/` with `.`
Last part `send_daily` is function name




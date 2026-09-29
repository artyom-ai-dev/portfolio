# 04 · Internal web + Excel automation

## Problem
Recurring spreadsheet work and email steps were manual, error-prone, and hard to standardize.

## Role
Fullstack / data automation — Flask app, LDAP auth, Excel pipelines, email notifications.

## What I built
Internal Flask web app:
- directory-based auth and access control
- Excel upload/processing with pandas + openpyxl
- normalization, matching, exports
- email notifications for completed flows

## Constraints
- Staff need a simple UI, not a notebook or CLI
- Spreadsheet formats vary; matching must be explicit and reviewable
- Access control via corporate directory

## Stack
Python, Flask, LDAP, pandas, openpyxl, SMTP

## Related private repos
`YGO_WEB`

## Outcome
Repeated table operations became a service workflow with a simple UI for staff and fewer manual mistakes.

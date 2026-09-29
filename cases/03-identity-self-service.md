# 03 · Identity self-service bot

## Problem
Expired directory passwords created repetitive support tickets. Users needed a guided self-service flow inside the corporate messenger.

## Role
Backend / automation engineer — bot + webhook service, directory integration, basic audit trail.

## What I built
Messenger bot + webhook service:
- receives password-expiry events
- verifies the user
- validates password policy
- updates credentials through a secure directory connection
- keeps session state and basic audit trail

Related: alerts on account lifecycle events (`usercreatealertbot`).

## Constraints
- Secure directory connection (LDAPS)
- Password policy must be enforced before write
- Session and audit trail for supportability

## Flow

```text
Expiry event
  → bot message in messenger
  → user verification
  → new password + policy checks
  → secure directory update
  → confirmation
```

## Stack
Python, Flask, LDAP/LDAPS, webhooks, SQLite, Docker

## Related private repos
`pass_bot` · `usercreatealertbot`

## Outcome
Common “password expired” cases moved to self-service instead of manual support handling.

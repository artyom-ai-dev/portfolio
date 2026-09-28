# 03 · Identity self-service bot

## Problem
Expired directory passwords created repetitive support tickets. Users needed a guided self-service flow inside the corporate messenger.

## What I built
Messenger bot + webhook service:
- receives password-expiry events
- verifies the user
- validates password policy
- updates credentials through a secure directory connection
- keeps session state and basic audit trail

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

## Outcome
Common “password expired” cases moved to self-service instead of manual support handling.

---
title: Logging Functions
layout: functionv2
---

# Logging

## Log

A normal level of log message, this is generally informational or helpful for the user.

```yaml
- log: You've successfully logged in!
```

## Debug

Less important information that is only visible if the user has enabled the `isVerbose` flag.

```yaml
- debug: Logging into https://login.example.org
```

## Warn

This log level is not necessarily an error, but it should be brought to the user's attention.

```yaml
- warn: Your password will expire in 5 days
```

## Error

This level is used for error information

```yaml
- error: Your username and/or password is incorrect
```
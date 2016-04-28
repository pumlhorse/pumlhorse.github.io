---
title: Filters
layout: reference
---
# Filters
Filters can handle the following events:

 - `onSessionStarting(): Promise<bool>`
   - Occurs after all files have been detected, but before any scripts have been loaded
 - `onScriptStarting(script: Script): Promise<bool>`
   - Occurs after a script has been loaded, but before it has run
 - `onScriptFinished(script: Script, isSuccess: bool): Promise<void>`
   - Occurs after a script has run. If the script was canceled through a filter,
   this event will still occur
 - `onSessionFinished(scriptCount: int, failedScripts: int): Promise<void>`
   - Occurs after all scripts have run. If the session was canceled through a filter,
   this event will still occur

As the name implies, the "starting" events can effectively cancel that event by throwing an error or returning false.
This can be used to perform some validation on the script, or just to run external processes. Some examples:
 - Cancel a test case if it doesn't have `testCaseId` specified
 - Mark test cases as passed/failed in an external system
 - Update an audit database after deployment scripts have been run 

Filters are implemented as javascript files. They are passed in via the `profile` option in the profile page.

See the `examples/filters` directory for sample implementations.

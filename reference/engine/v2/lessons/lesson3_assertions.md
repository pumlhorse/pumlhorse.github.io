---
title: Assertions
layout: lesson
---

One of the key uses for Pumlhorse is writing test scripts. To facilitate this, Pumlhorse provides a set of
basic assertions. These assertions will verify that an expectation is met and throw an error if it fails.

```yaml
name: Do an assertion
steps:
  - isTrue: ${ 5 > 6 }
```

If you run this script, it will error with the following info:

```
Line 3: Assertion 'isTrue' failed. Expected 'true', actual 'false'
SCRIPT FAILED
1 scripts run, 1 failure
```

Change `5 > 6` to `5 < 6` and run it again. No errors!

Another useful assertion is `areEqual`. It takes two parameters, `expected` and `actual`.

```yaml
name: Check equality
steps:
  - areEqual:
      expected: 14
      actual: 15
```

What error shows when you run this script? How would you change it to not throw an error?
What if you use `areNotEqual`?

The following script uses all the standard assertions (all of which pass).

```yaml
name: Use assertions
steps:
# all of these assertions succeed
- isTrue: ${5 == 7 - 2}
- isFalse: ${Math.PI == 3}
- areEqual: 
    expected: men 
    actual: ${"women".substr(2)}
- areEqual:
    expected:
      game: horseshoe
    actual:
      game: horseshoe
      another: handgrenades
    partial: true # Only properties in expected value will be checked in actual value
- areNotEqual:
    expected: free as in beer
    actual: free as in speech
- isEmpty: ${[]}
- isNotEmpty: ${[3, 4, 5]}
- contains: 
    array: 
      - 4
      - 5
      - 6
    value: 6
- contains:
    array:
      - game: horseshoe
        another: handgrenades
    value:
      game: horseshoe 
    partial: true
- isNull: null
- isNotNull: 19
```

## Deliberately failing

If you need an easy way to fail a script, you can use `fail`

```yaml
name: Fail a script
steps:
  - fail
#   optionally, you can pass a message
# - fail: My failure message
  - log: this never gets called
```

Assertions are very useful in identifying issues during testing, but what if we want something more complex?
Let's find out in the next lesson.
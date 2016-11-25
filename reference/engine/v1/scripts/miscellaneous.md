---
title: Miscellaneous functions
layout: reference
---
# Miscellaneous functions

## Parallel steps

Run multiple steps in parallel

```yaml
name: Run multiple steps in parallel
steps:
  - parallel:
      - postUpdateToFacebook: This is my new status!
      - postUpdateToTwitter: This is my new tweet!
      - postUpdateToInstagram: This is a picture of my cat!
```

## Wait

Pause the script for the given amount of time

```yaml
name: Wait for an amount of time
steps: 
  - wait:
      milliseconds: 23
      seconds: 45
  - log: Well that was kinda long
  # Waits for 45.023 seconds
  - wait:
      minutes: 60
      hours: 3
  # Waits for four hours...use at your own discretion
  - log: Is anyone even here anymore?
```

## Timer
Keep track of time spent between steps using `startTimer` and `stopTimer`

```yaml
name: See how long we waited
steps:
  - timer1 = startTimer
  - doLongRunningProcess
  - stopTimer: $timer1
  - log:
      - that took %s seconds
      - $timer1.seconds
```

## JSON (de)serialization

The `toJson` function will convert from an object to string, while `fromJson` will
convert from a string to an object (or throw an error)

```yaml
name: Convert from string to object
functions:
  getObject: return { prop: 43 }
steps:
  - obj = getObject
  - jsonString = toJson: $obj
  - log: $jsonString #logs "{prop:43}"
  - newObj = fromJson: $jsonString 
  - log: $newObj.prop #logs "43"
```
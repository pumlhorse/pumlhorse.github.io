---
title: Flow Control
layout: lesson
---

## Conditionals

Pumlhorse wouldn't be much of a scripting language if it couldn't support basic things like "if" statements, would it?

```yaml
name: Check whether it's the weekend
functions:
  get_today:
    - return new Date().getDay()
steps:
  - dayOfWeek = get_today
  - if:
      value: ${ dayOfWeek == 5 }
      is true:
        - log: Congratulations, today is Friday!
      is false:
        - if:
            value: ${ dayOfWeek == 6 || dayOfWeek == 0}
            is true:
              - log: Congratulations, it's the weekend!
            is false:
              - log: Hang in there, you'll make it! 
```

As you can see, the `if` function takes a few parameters, `value`, `is true`, and `is false`. `value` is always required,
and at least one of `is true` and `is false` is required. These two parameters take an array of steps that will run if
the condition holds.

## Loops

Another common programming construct is the loop. Pumlhorse loves loops so much it has three kinds!

The `repeat` loop will run a series of steps a certain number of times.

```yaml
name: Get back to Kansas
steps:
  - repeat:
      times: 3
      steps:
        - log: There's no place like home!
```

The `for` loop lets you iterate over an array. We can use this to simplify the "days of the week" example from Lesson 2.

```yaml
name: Print the days of the week
steps:
  - days_of_week = value:
      - Monday
      - Tuesday
      - Wednesday
      - Thursday
      - Friday
      - Saturday
      - Sunday
  - for:
      each: day
      in: $days_of_week
      steps:
        - log: $day
```

Note that the `each` property specifies the variable name we want to use inside the loop.

The `scenarios` loop lets you perform the same steps under different conditions.

```yaml
name: Validate password errors
steps:
  - scenarios:
      base: # optional, provides a base set of values for every scenario
        username: jsmith
      cases:
        too short: #first scenario
          password: bad
          error: Password is too short
        too long: #second scenario
          password: this_is_too_long_of_a_password_for_our_puny_system_to_handle
          error: Password is too long
        no special characters:
          password: Password123
          error: Password must contain a special character
      steps:
        - result = login:
            username: $username
            password: $password
        - isFalse: $result.isSuccess
        - areEqual:
            expected: $error
            actual: $result.error
```

In this case, we have three cases to test: "too short, "too long", and "no special characters". Each of these scenarios
sets the password and the expected error. All three cases inherit from the "base" case, which sets the username.

## Parallel Processing

As we discussed in the first lesson, Pumlhorse runs steps one by one, waiting for each step to finish before starting the next.
The `parallel` function allows us to run multiple steps at a time.

```yaml
name: Run parallel steps
steps:
  - parallel:
      - postUpdateToFacebook: This is my new status!
      - postUpdateToTwitter: This is my new tweet!
      - postUpdateToInstagram: This is a picture of my cat!
  - log: All done!
```

This script uses the (imaginary) functions `postUpdateToFacebook`, `postUpdateToTwitter`, and `postUpdateToInstagram`. 
Since these functions might take awhile and don't depend on each other, we can call them at the same time. However,
the `log` call will not run until all three `postUpdate` functions finish.

## Early exit

Occasionally you will want to stop the script without having to throw an error.

```yaml
name: Exit early
steps:
  - log: nothing to do!
  - end
  - log: this step never runs
```

## Cleaning up

Occasionally, your scripts will have steps that need to be taken when the script is done. For instance,
if you create test records in the database, you may need to delete those test records. However, you want
to make sure that those steps are executed even if an error occurs. In that case, you would add it to the
`cleanup` section. Each step in the `cleanup` section is guaranteed to run.

```yaml
name: Run cleanup steps
steps:
  - addRecordToDatabase
  - addAnotherRecord
  - fail # script will stop running steps here
  - addThirdRecord # This won't run
cleanup:
  - deleteThirdRecord 
  - deleteSecondRecord
  - deleteFirstRecord
```

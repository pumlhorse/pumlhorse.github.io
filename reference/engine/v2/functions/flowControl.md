---
title: Flow Control
layout: functionv2
---

## `if`

Conditionally run steps. At least one of `is true` and `is false` is required. You do not need to specify both

```yaml
- if:
    value: ${2 + 2 == 4}
    is true:
      - log: Two plus two is four!
      - log: Success!
    is false:
      - log: Well, something's wrong here...
      - error: Failure!
```

## `for`

Run `steps` for each item in the `in` parameter (which must be an array).

```yaml
- for:
    each: person
    in: $people
    steps:
      - log: Got $person.first_name $person.last_name
```

## `repeat`

Runs `steps` a given number of `times`.

```yaml
- repeat:
    times: 3
    steps:
      - log: There's no place like home!
```

## `scenarios`

Runs `steps` for a set of `cases`.

```yaml
- scenarios:
    cases:
      password is empty:
        username: jsmith
        password: null
      password is too short:
        username: jsmith
        password: f
      password is too easy:
        username: jsmith
        password: hunter2
    steps:
      - set_password:
          username: $username
          password: $password
```

You can also specify a `base` case that is applied to all `cases`.

```yaml
- scenarios:
    base:
      username: jsmith
    cases:
      password is empty:
        password: null
      password is too short:
        password: f
      password is too easy:
        password: hunter2
    steps:
      - set_password:
          username: $username
          password: $password    
```

## `parallel`

Runs multiple steps "concurrently".

```yaml
- parallel:
    - getUserName: $userId
    - getAddressForUser: $userId
    - getPhoneNumberForUser: $userId
```

Of course, it's important to remember that JavaScript (which Pumlhorse runs on) is single-threaded. Thus, the steps will only appear to run concurrently if there is some sort of thread suspension, like I/O.

## `run`

Runs another Pumlhorse script.

```yaml
- run: anotherScript.puml
# Pass parameters to the script
- run:
    script: logUserIn.puml
    with:
      username: jsmith
      password: hunter2
```

This function can come in handy for building reusable components. In the example above, `logUserIn.puml` could be used across many scripts that need to log a user in.

You can also assign the result of another script to a variable.

**addNumbers.puml**
```yaml
name: Add two numbers
expect:
  - number1
  - number2
steps:
  - sum = add:
      - $number1
      - $number2
```

**myScript.puml**
```yaml
name: My script
steps:
  - addNumbersResult = run:
      script: addNumbers.puml
      with:
        number1: 43
        number2: 11
  - log: 43 + 11 = $addNumbersResult.sum
```
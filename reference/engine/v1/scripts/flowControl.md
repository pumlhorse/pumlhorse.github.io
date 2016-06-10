---
title: Flow Control
layout: reference
---
# Flow Control

Many scripts are just a list of steps that run once, in order, and then the script completes.
However, sometimes you will want to do things like loop through an array, or 
run a series of steps multiple times.

## Loop through an array

```yaml
name: Loop through users
steps:
  - users = ${[ "jsmith", "aturing", "bnewhart" ]}
  - for:
      each: user # the variable each item will be assigned to
      in: $users # the array
      steps:
        - log: Working with user $user
        - doSomethingWithUser: $user
  - log: All done with the list of users!    
```

## Repeat steps a set number of times

```yaml
name: Make Bill Murray a better man
steps:
  - repeat:
      times: 12403
      steps:
        - practicePiano
        - makeIceSculpture
        - catchFallingKid
```

## Perform steps under multiple conditions

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

## Notes about scope

"Scope" can be thought of as the current state of data. As you make changes to variables,
you are changing values on the scope.

All of the functions described above use an isolated scope. That means that any changes made
to the scope in the inner `steps` are lost once the function is done. Similarly, each loop has
a scope isolated from the other loops

```yaml
name: Show how scope is isolated
steps:
  - index = 0
  - log: Starting index is $index
  - index = ${index + 1}
  - log: Index is now $index
  - for:
      each: val
      in: ${ [1, 22, 44] }
      steps:
        - index = ${index + 1}
        - log: Index is $index, val is $val
  - log: Ending index is $index
  # Logs the following:
  # Starting index is 0
  # Index is now 1
  # Index is 2, val is 1
  # Index is 2, val is 22
  # Index is 2, val is 44
  # Ending index is 1
``` 
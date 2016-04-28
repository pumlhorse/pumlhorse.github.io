---
title: Pumlhorse v1
layout: reference
---
# Introduction

Pumlhorse is a scripting framework built on top of Node.js. It reads `.puml` "scripts", runs
the operations in the script, and outputs the information. It can be run from a command line interface (CLI)
or via a UI (such as the Visual Studio Code extension).

A `.puml` file is written in [YAML](http://www.yaml.org/) format. This should make it familiar with anyone who has worked with JSON data.
However, it has better readability due to the reduced dependency on curly braces, quotation marks, and commas.
Instead, it uses whitespace to organize information.

## Anatomy of a script

Every script requires two things: a name, and at least one *step*. A step can be as simple as logging "Hello world!", but it can be
much more complex. Let's start with the simple case.

```yaml
name: My first 'Hello World!' script
steps:
  - log: Hello world!
```

The `name` property should be pretty obvious, but for those new to YAML, 
the `steps` property is defined as an array. This array has one item, namely the
`log` function. We provide "Hello world!" as a parameter to `log`.

```yaml
name: My second 'Hello World!' script
steps:
  - log: Hello world!
  - log: I'm enjoying my YAML experience!
```

Now we have two logging calls. Steps are run in the order that they appear,
so we'll always see "Hello world!" first.

There are a few reserved properties in PUML that you will learn about later, 
but everything else is available for use. That is, if you wanted to put some metadata
in your script, you could do something like the following:

```yaml
name: My second 'Hello World!' script
description: This writes a couple sweet messages to the console
author: John Smith
# Log some sweet messages
steps:
  - log: Hello world!
  - log: I'm enjoying my YAML experience!
last updated: April 24th, 2016
```

None of this metadata will affect how the script runs. Note that it can be placed
before or after the steps, and the property name can contain spaces. You can
also use YAML comments (placing text after the `#` sign) without affecting the outcome of the script

## Passing multiple parameters

Fortunately, there's more to Pumlhorse than just logging messages. Let's say that
we have a `login` function that will log us in to a system. As you would expect, it has two parameters:
`username`, and `password`.

```yaml
name: Login to the system
steps:
  - login:
      username: jsmith
      password: hunter2
  - log: I'm logged in!
```

It's as simple as that. If the login fails, then the script will stop and we won't
log "I'm logged in!".

## Using variables

Of course, you might want to do something with your login session. Upon a successful login,
we get user information (first name, last name, and email address)

```yaml
name: Get login user info
steps:
  - myUser = login:
      username: jsmith
      password: hunter2
  - log: I am $myUser.firstName $myUser.lastName ($myUser.emailAddress)
```

This will output `I am John Smith (jsmith@example.org)`. In the login step, we're now
assigning the result to `myUser`, which we can then reference later through `$myUser`

## Logging
Pumlhorse contains multiple functions to add log messages

```yaml
name: Basic logging functions
steps:
  - log: Here's a simple log message
  - warn: Something might be wrong
  - error: Something is definitely wrong!
```

## Making assertions

Often your scripts will need to verify information before continuing. This is essential for
test cases, but even utility scripts may need to ensure that things are in a good state.
For this, Pumlhorse provides a set of assertions.

```yaml
name: Get login user info
steps:
  - myUser = login:
      username: jsmith
      password: hunter2
  - areEqual:
      expected: jsmith
      actual: $myUser.username
  - isTrue: $myUser.successfullyLoggedIn
  - log: I am $myUser.firstName $myUser.lastName ($myUser.emailAddress)
```

There are more assertions available than just `areEqual` and `isTrue. Please see the assertions section

## Cleaning up

As stated before, if any step fails, the rest of the steps will be ignored. However, 
sometimes you have tasks you need to complete before exiting the script. For example,
some tests do some sort of data setup and tear-down. You can use the `cleanup` section for these tasks.
They will still run in order, but they will all run (even if another cleanup task fails)

```yaml
name: Run a cleanup task
steps:
  # Data setup
  - createUser:
      name: testUser
  - somethingThatFails # this step fails
cleanup:
  - removeUser: testUser # this will always run
```
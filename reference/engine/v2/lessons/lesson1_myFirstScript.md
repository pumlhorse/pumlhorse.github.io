---
title: My First Pumlhorse Script
layout: lesson
---

## What is a script?

A Pumlhorse script consists of a list of instructions, also known as _steps_. Pumlhorse runs these steps
in order, from first to last, and will only run one at a time (with the exception of _parallel_ steps, but more on that later).
If a step causes an error, then the script will stop and no further steps will be run.

A step could:

* Print a message
* Assign a value
* Download a file

Because Pumlhorse is built on top of Node, you can pretty much do anything you want. However, in the true spirit of programming,
our first script will print out "Hello world!"

## Creating a script

Pumlhorse scripts are [YAML](http://yaml.org) files that end in the `.puml` extension. The YAML format provides a clear structure
to the script, at the expense of the flexibility in formatting that other languages have.

For our first script, we'll create a file named `hello_world.puml` (see [Getting Started](./lesson0_gettingStarted.md) for
editor information). The first thing every Pumlhorse script needs is a name.

```yaml
name: Hello world!
```

Just having a name doesn't seem very exciting, but let's see what happens if we try to run it. From your command line,
use the following command: `pumlhorse hello_world.puml`

You should see something like the following:

```
Script does not contain any steps
1 scripts run, 0 failures
Total time: 29.933ms
```

Naming things is fun and all (about as fun as cache invalidation), but let's see if we can't make this script do something.
Make the following changes to your script.

```yaml
name: My first Pumlhorse script
steps:
  - log: Hello, world!
```

In YAML syntax, we've added a new property "steps" and declared it an array. This array has a single item in it, namely our
log call. If we run the command again, we should see different output:

```
--------------
## My first Pumlhorse script - C:\pumlhorse_scripts\hello_world.puml ##
Hello, world!
1 scripts run, 0 failures
Total time: 28.233ms
```

That's better! There's a little extra information, but we can see the log call output. But what if we want to print out something else?

```yaml
name: My first Pumlhorse script
steps:
  - log: Hello, world!
  - warn: Pumlhorse can be pretty fun
  - error: No error, I just like red text
```

Now our script has three steps that output text. `warn` will output in yellow text and `error` text will be red. Calling `error` will not
stop the script from running.

What if we want to add more information to a script? Maybe a name isn't enough detail. Well, we can add a `description` to the script too.
In fact, we can add any property we want.

```yaml
name: My first Pumlhorse script
description: >
             This is the first lesson in writing Pumlhorse, 
             and frankly I couldn't be more entertained
date created: January 1st, 2017
author: John Smith
steps:
  - log: Hello, world!
```

Continue to the next lesson to see what else you can do in your script.
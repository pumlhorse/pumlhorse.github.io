---
title: Using Variables
layout: lesson
---

# Using Variables

Most of the time we will want to save data and then act on it. We can save this data into variables
and then access them later.

```yaml
name: Print a name
steps:
  - my_name = Alan Turing
  - log: Hello, nice to you meet you. My name is $my_name
```

Pretty simple, right? We save the value "Alan Turing" to the variable `my_name`. Then when we want to access it
later, we reference it by putting a `$` in front of it. This is useful for steps that return a value.

```yaml
name: Print the current date
functions:
  get_date:
    - return new Date();
steps:
  - right_now = get_date
  - log: It's currently $right_now
```

This might take a bit of unpacking. The `functions` property is new to us. It allows us to add custom functions per script.
For now, just know that there's a function called "get_date" that returns the current date.

Notice how the syntax looks very similar to the previous script? Pumlhorse is smart enough to know that "Alan Turing" is just a value,
but `get_date` is a function that should be called. It returns the current date and is stored in the variable `right_now`.

What if we want to use more complex values than just strings?

```yaml
name: Print employee information
steps:
  - employee = value:
      first_name: Edsger
      last_name: Dijkstra
      birth:
        date: May 11, 1930
        location: Rotterdam, Netherlands
  - log: Employee $employee.first_name $employee.last_name was born on $employee.birth.date in $employee.birth.location
```

In the first step, we assign an object value to the `employee` variable. Then we print that information out using JavaScript
object reference syntax.

What about arrays?

```yaml
name: Print the days of the week
steps:
  - days_of_week = value:
      - Monday
      - Tuesday
      - Wednesday
      - You see where this is going
  - log: $days_of_week[0]
  - log: $days_of_week[1]
  - log: $days_of_week[2]
  - log: $days_of_week[3]
  # Yes, there's any easier way to do this. We'll get there later
```

Let me guess, basic arithmetic?

```
name: Add two numbers
steps:
  - x = value: 42
  - y = value: 8958
  - log: It's over ${ x + y }!
```

In this log step, we use a new syntax. Rather than just getting a variable, we're adding two of them together.
The `${}` syntax will perform certain code inside the curly braces (those familiar with [AngularJS](https://angularjs.org)
will recognize it as an Angular expression). We also don't need the `$` symbol to mark variables inside the curly braces.

Well, that just about covers it. Just kidding, go to the next tutorial!
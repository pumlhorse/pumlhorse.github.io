---
title: Miscellaneous
layout: functionv2
---

# Miscellaneous

The following are functions that don't fit in any other section.

## `convertText`

Converts text from one encoding to another. Typically used for base64 encoding/decoding.

```yaml
- inBase64 = convertText:
    text: My text value
    to: base64
- originalValue = convertText:
    text:
    from: base64
```

If not specified, parameters `to` and `from` are set to "utf-8".

## `date`

Generates a new date from a string. If no string is specified, it returns the current date

```yaml
- newYear = date: 1/1/2017
- now = date
- log: New year was $newYear, now it's $now
```

The result of `date` is a JavaScript [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object. This allows you to use the built in functionality.

```yaml
- newYear = date: 1/1/2017
- areEqual:
    expected: 2017
    actual: $newYear.getFullYear()
```

## `prompt`

This function prompts the reader to input a value. This is presented differently depending on where the script is running (Command-Line, Visual Studio Code extension, etc.), and may not always be available.

```yaml
# Option 1
- prompt:
    ask: What is your username?
    for: username
# Option 2
- password = prompt: What is your password?
- log: Your username is $username and your password is $password.length characters
```

In the above example, the user is prompted for `username`. If the script is run with a [context](../context.md) where the `username` value is already set, it will use that value and skip prompting the user.

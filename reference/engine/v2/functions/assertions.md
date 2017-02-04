---
title: Assertions
layout: functionv2
---

The following assertion functions allow you to validate your data. If an assertion fails, the script will stop.

## `isTrue`

Asserts that the parameter is true.

```yaml
- isTrue: $someBooleanValue
```

You can also assert on expressions

```yaml
- isTrue: ${ 2 + 2 == 4}
```

## `isFalse`

Asserts that the parameter is false. Similar to `isTrue`

## `areEqual`

Asserts that the `expected` and `actual` parameters are equal.

```yaml
- areEqual:
    expected: 4
    actual: $twoPlusTwo
```

You can also compare objects.

```yaml
- areEqual:
    expected:
      first_name: John
      last_name: Smith
      age: 23
      address:
        city: Los Angeles
        state: California
    actual: $person
```

When comparing objects, you can also specify the `partial` flag. If true, it will only compare the properties in `expected`.

```yaml
- areEqual:
    expected:
      first_name: John
      address:
        city: Los Angeles
        state: California
    actual: $person
    partial: true
```

## `areNotEqual`

Asserts that the `expected` and `actual` parameters are **not** true. Similar to `areEqual`, except it does not
have the `partial` parameter.

## `isNull`

Asserts that the parameter is null.

```yaml
- isNull: $myValue
```

## `isNotNull`

Asserts that the parameter is **not** null. Similar to `isNull`.

## `isEmpty`

Asserts that the parameter (which must be an array) is empty.

```yaml
- isEmpty: $myArray
```

## `isNotEmpty`

Asserts that the parameter (which must be an array) is **not** empty. Similar to `isEmpty`.

## `contains`

Asserts that the parameter `in` (which must be an array) contains `value`. Similar to `areEqual`, except it will search the array
until it finds a value that passes.

```yaml
- contains:
    value:
      first_name: John
      last_name: Smith
      age: 23
      address:
        city: Los Angeles
        state: California
    in: $people      
```

When comparing objects, you can also specify the `partial` flag. If true, it will only compare the properties in `value`.

```yaml
- contains:
    value:
      first_name: John
      last_name: Smith
      address:
        city: Los Angeles
    in: $people
    partial: true  
```

## `fail`

Deliberate fails the script.

```yaml
- fail
```

You can also pass a message for the failure.

```yaml
- fail: I gave up
```
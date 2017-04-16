---
title: Math
layout: functionv2
---

# Math

Pumlhorse provides a handful of basic math functions. For example, the `add` function adds an array of numbers.

```yaml
- val = 99
- sum = add:
    - 43
    - 11
    - $val
- log: The sum is $sum # logs "The sum is 153"
```

You can also write this using the `+` operator.

```yaml
- val = 99
- sum = +:
    - 43
    - 11
    - $val
- log: The sum is $sum # logs "The sum is 153"
```

The same patterns apply for `subtract` (`-`), `multiply` (`*`), and `divide` (`/`). Note that the order of array items matters for `subtract` and `divide`. For instance:

```yaml
- val1 = subtract:
    - 19
    - 5
    - 7
- val2 = subtract:
    - 5
    - 7
    - 19
```

In this example, `$val1` would be 7 (19 - 5 -7), whereas `$val2` would be -21 (5 - 7 - 19).

## `square`

Returns the square of a single number

```yaml
- val = square: 5
# $val = 25
```

## `stats` Module

The stats module contains some basic statistics functions. You must explicitly include it in your scripts

```yaml
name: Find the average
modules:
  - stats # Must include stats module
steps:
  - avg = average: 
      - 4
      - 19
      - 21
      - 5
      - 88
  - areEqual:
      expected: 27.4
      actual: $avg
```

The functions `average`, `median`, `min`, and `max` all take an array of numbers. `min` and `max` will return the minimum and maximum value in the array, respectively.

In addition to accepting an array of numbers, you can pass an array of objects and a `field` expression. The `field` describes what value on the object to use in the calculation.

```yaml
name: Find the median square miles
steps:
  # Populate an array of states
  - states = value:
      - name: California
        geography:
          squareMiles: 163696
          highestPointInFeet: 14505
      - name: Alaska
        geography:
          squareMiles: 663268
          highestPointInFeet: 20310
      - name: South Carolina
        geography:
          squareMiles: 32020
          highestPointInFeet: 3560
      - name: Iowa
        geography:
          squareMiles: 56272.81
          highestPointInFeet: 1671
  - mid = median: 
      values: $states
      field: geography.squareMiles
  - areEqual:
      expected: 4
      actual: $mid
```
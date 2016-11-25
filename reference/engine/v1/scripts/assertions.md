---
title: Assertions
layout: reference
---
# Assertions

Assertions are used to stop the script if something unexpected occurs.

For brevity's sake, these examples use the inline JavaScript notation. Please review that 
section for more information.  

## Is True/False

These assertions take one parameter and fail if the value is not true (`isTrue`)
or not false (`isFalse`).

```yaml
name: Test isTrue and isFalse
steps:
  - isTrue: ${5 == 7 - 2} #passes
  - isFalse: ${Math.PI == 3} #passes
  - isTrue: ${2 + 2 == 5} #fails
  - isFalse: ${3 < 2} #fails
```

## Is (Not) Null

These assertions take one parameter and fail if the value is not null (`isNull`)
or null (`isNotNull`).

```yaml
name: Test isNull and isNotNull
steps:
  - isNull: $variableThatDoesntExist #passes
  - isNotNull: this is a string that isnt null #passes
  - isNull: 12345 #fails
  - isNotNull: $variableThatDoesntExist #fails
```

## Are (Not) Equal

These assertions have two parameters: "expected" and "actual".

```yaml
name: Test areEqual and areNotEqual
steps:
  - areEqual:
      expected: 4
      actual: ${2 * 2} #passes
  - areNotEqual:
      expected: 11
      actual: ${1 + 1} #passes      
```

### Comparing objects

Sometimes it is more useful to compare at the object level, rather than performing
many assertions at once. Let's say that `getUser` returns 
`{ username: "jsmith", firstName: "John", lastName: "Smith" }` 

```yaml
name: Compare objects
steps:
  - person = getUser
  - areEqual:
      expected:
        firstName: John
        lastName: Smith
        username: jsmith
      actual: $person #passes
  - areNotEqual:
      expected:
        firstName: Mary
        lastName: Smith
        username: msmith #passes
  - areEqual:
      expected:
        firstName: John
      actual: $person #fails, not all properties match
```

Note that the order of properties does not matter. Also, all properties
must be present (as seen in the third assertion), unless you use the `partial`
property.

```yaml
name: Compare partial objects
steps:
  - person = getUser
  - areEqual:
      expected:
        firstName: John
      actual: $person
      partial: true #passes, all provided properties match
```

## Is (Not) Empty

When working with arrays, you may want to do a quick check that it does or
does not have values.

```yaml
name: Check if items are empty
steps:
  - myEmptyArray = ${ [] }
  - isEmpty: $myEmptyArray #passes
  - myFilledArray = ${ [1, 2, 3] }
  - isNotEmpty: $myFilledArray #passes
```

## Contains

Another common operation with arrays is checking for a certain value

```yaml
name: Check for item in array
steps:
  - myArray = ${ [1, 2, 3] }
  - contains:
      array: $myArray
      value: 3 #passes
  - contains:
      array: $myArray
      value: 42 #fails
```

Similar to `areEqual`, you can perform this check with objects. Let's say that `listUsers`
returns two user objects for "John Smith" and "Alan Turing"

```yaml
name: Check for object in array
steps:
  - users = listUsers
  - contains:
      array: $users
      value:
        firstName: Alan
        lastName: Turing #passes
  - contains:
      array: $users
      value:
        firstName: John
      partial: true #passes
```

Again, we can use the `partial` property to restrict matching to the data we
care about.
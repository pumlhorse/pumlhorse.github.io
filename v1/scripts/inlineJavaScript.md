---
title: Inline JavaScript
layout: reference
---
#Inline JavaScript

In some cases, you may want to use JavaScript inside your 
scripts. Pumlhorse fully supports custom JavaScript functions, as well 
as a subset of JavaScript expressions in steps.

## Custom functions

```yaml
name: Declare JavaScript functions inline
functions:
  getMyInfo: return { name: 'John Smith', age: 25 }
  logMyInfo:
    # list parameters first 
    - age
    - name
    # list function body last
    - console.log('My name is ' + name + ' and I am ' + age + ' years old')
steps:
  - myInfo = getMyInfo
  - logMyInfo:
      name: $myInfo.name
      age: $myInfo.age
  # logs "My name is John Smith and I am 25 years old"
```

For those familiar with JavaScript syntax, these declarations are equivalent to:
```javascript
var getMyInfo = new Function("return { name: 'John Smith', age: 25 }")
var logMyInfo = new Function("age", "name", "console.log('My name is ' + name + ' and I am ' + age + ' years old')")
```

##Inline expressions

As shown in the "Assertions" section in the Introduction, we can use inline expressions for
basic math operations or variable assignments. The expression is written like `${my_expression}`,
replacing `my_expression` with the actual expression.

```yaml
name: Compute squares
steps:
  - for:
      each: num
      in: ${ [1, 2, 3, 4, 5] }
      steps:
        - log: ${ num * num }
    # Logs "1", "4", "9", "16", and "25"
```

Note that when referencing variables inside an expression, you don't need to use the `$` prefix.

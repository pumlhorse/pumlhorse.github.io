---
title: Scope schema
layout: referencev2
---

## Using Scope

Each function call in a script has access to a `scope` item. The scope is where 
variables are stored. When you run `username = jsmith`, that sets the `username` property
on the `scope` to "jsmith".

To access the `scope`, your function just needs to include a `$scope` parameter. From there you can get or set scope values.

## Variables

Some variables are automatically set on a scope.

### `scriptId`

When Pumlhorse runs, it assigns each script a Universally/Globally Unique Identifier (UUID/GUID). This is used to correlate events back to the script. A script's ID changes every time it is run.

```yaml
- log: Script ID is: $scriptId
```

The scope also has its own set of functions. These are preceded with the `_` symbol.

## External functions

### `_id`

This function generates a new UUID.

```yaml
- log: $_id() # logs something like "4396d060-d836-4069-bbf5-77b32ac4e35f"
```

You could also call it like so:

```yaml
- myId = _id
- log: $myId # logs something like "4396d060-d836-4069-bbf5-77b32ac4e35f"
```

### `end`

This function signifies that the script should stop early but not be considered an error.

```yaml
- log: This is printed
- end
- log: This is never reached
```

## Internal functions

The following functions are not commonly used in a script definition, but rather in the JavaScript modules
that can be called from a script.

### `_new`

This function creates a new scope based off of the current scope. Consider the following pseudocode:

```javascript
function myFunction($scope) {
    $scope.x = 42;
    $scope.obj = { val: 'hello!'};
    log(currentScope); // Logs { x: 42, obj: { val: 'hello!' } }
    var newScope = $scope._new();
    newScope.x = 11;
    newScope.y = 99;
    newScope.obj.val = 'goodbye!';
    log(newScope); // Logs { x: 11, y: 99, obj: { val: 'goodbye!' } }
    log($scope); // Logs { x: 42, obj: { val: 'goodbye!' } }
}
```

As you can see, the shallow changes to the scope (changing `x`, adding `y`) do not apply to `$scope`.
However, changes to nested objects (like `obj.val`) _do_ apply to `$scope`.

You can also specify data to be added to the new scope, analogous to a "stack" in programming languages.

```javascript
function myFunc($scope) {
    $scope.x = 42;
    log($scope); // Logs { x: 42 }
    var newScope = $scope._new({x: 11, y: 99});
    log(newScope); // Logs { x: 11, y: 99 }
    log($scope); // Logs { x: 42 }
}
```

### `_runSteps(steps: Step[])`

This function instructs Pumlhorse to perform a set of steps. For instance, the `repeat` function takes two parameters:
`times` (which signifies how many loops to run), and `steps` (which signifies what should happen
in each loop). Each loop, the function calls `_runSteps` with the `steps` array.

### `_cleanup(cleanupStep: Step)`

Adds a step to the top of the Cleanup stack, running _before_ all of the current cleanup tasks.

### `_cleanupAfter(cleanupStep: Step)`

Adds a step to the bottom of the Cleanup stack, running _after_ all of the current cleanup tasks.

### `_emit(eventName: string, eventInfo: any)`

Emits an event with an optional payload

### `_module(moduleName: string)`

Retrieves the specified module, if it exists.
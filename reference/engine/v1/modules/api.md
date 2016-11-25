---
title: Pumlhorse Module API
layout: reference
---

# Pumlhorse API

## Module API

Declare a module:
`pumlhorse.module(module_name)`

Returns a `ModuleDeclaration`

### ModuleDeclaration
Provides functions to register a Pumlhorse module

#### .function(functionName, functionDeclaration, [functionOptions])

* functionName (`String`) - The user-visible name for this function. Does not need to match the name of the 
 function implementation
* functionDeclaration (`FunctionDeclaration`)
* functionOptions (`FunctionOptions`) - _optional_

### FunctionDeclaration

A function declaration can be a reference to the function implementation or an array.

```javascript
pumlhorse.module("customModule")
    .function("myFunction", function () { return "Declared inline" })
    .function("myFunction2", myFunction2)
    .function("functionWithArray", ["parameter1", "parameter2", anotherFunction])
    
    function myFunction2() {
        return "Declared elsewhere"
    }
    
    function anotherFunction(val1, val2) {
        return "The parameter names don't have to match"
    }
```

The `functionWithArray` declaration should be familiar to angular developers. It must contain names for all parameter in order, followed by the function implementation.
The names do not need to match, which allows you to minify your modules, if needed. In the above case,
the user calling `functionWithArray` in a Pumlhorse script will use `parameter1` and `parameter2`, which
will be assigned to `val1` and `val2` respectively.


### FunctionOptions 

All `FunctionOptions` properties are optional. 

* `passAsObject` - If true, all parameters passed to the function will be combined into a single object. For instance, the `value` function
uses this.

```yaml
name: Use passAsObject
modules:
  - customModule
steps:
  - value:
      val1: 123
      val2: abc
      nested:
        val3: true    
```

```javascript
pumlhorse.module("customModule")
    .function("getWholeObject", getWholeObject, {
        passAsObject: true    
    })
    
    function getWholeObject() {
        return arguments[0]; //returns { val1: 123, val2: "abc", nested: { val3: true } }
    }
```

* `deferredParameters (String[])` - List of parameter names whose evaluation should be deferred. In some cases, you will want to defer evaluation of a certain parameter. For instance, the repeat function has a `steps` parameter, which should not be evaluated until that step is run.

```yaml
steps:
  - loopTimes = 4
  - repeat:
      times: $loopTimes
      steps: #innerSteps
        - myName = Joseph
        - log: $myName
```

When PumlHorse parses the repeat step, it should evaluate $loopTimes, but not steps. The parser sees the steps as an array of parameters to be 
passed to a function. The first inner step would be run again to assign 'Joseph' to myName, but the second step would have been evaluated 
before that assignment, so `$myName` would be undefined.

## Using Scope
Every function call has access to the script scope via the `this` object. This contains any variables that have been set 
as well as all available module functions. It also contains the helper methods described later.

When writing custom module functions, you'll frequently want to assign `this` to a local parameter, especially when dealing with promises.

```javascript
    function doSomethingAsync() {
        var scope = this; //Store a local instance of 'this'
        
        return doAsync()
            .then(function () {
                scope.didAsync = true;
                //Accessing 'this' here would not give you the scope
            })
    }    
```

## Scope Methods

The `Scope` object contains multiple methods that can help when writing custom modules.


* `$scriptId()` - Returns the unique ID for this script. It will be a different value each time the script is run
* `$runSteps(steps, [scope])` - Runs the given array of steps under the given scope. This will often coincide with the delayed evaluation.
If no scope is provided, the current scope is used.
property of `FunctionOptions`
* `$new([newStack])` - Builds a new scope based on the current scope with the values in `newStack` (optional)

```javascript
function runInNewScope() {
    var currentScope = this;
    currentScope.testVal = "outside scope"
    var newScope = currentScope.$new({
        testVal: "inside scope"
    })
    
    var steps = [{ log: "$testVal" }]
    
    return currentScope.$runSteps(steps, newScope) //Logs "inside scope"
        .then(function () {
            return currentScope.$runSteps(steps, currentScope) //Logs "outside scope"
        })
}
```

* `$emit(eventName, [eventArgs])` - Raises the 'eventName' event with the given arguments (optional). If the event does not have any subscribers, it will be ignored.
Events can be subscribed to via the 'listen' method on the script.
* `$module(moduleName)` - Returns the module registered with the given name (or throws an error if it hasn't been registered). Module functions can also be 
accessed through the scope, but it is possible that they are stored under a custom namespace. The `$module` function ignores any 
namespaces.
* `$cleanup(Function)` - This method allows you to add cleanup tasks to the script. Cleanup tasks are always run, regardless of whether all steps or previous 
cleanup tasks succeed. Cleanup tasks also have access to scope. **Note**: tasks will be added to the beginning of the list. This makes it simple to do things
like add dependent SQL records and delete them easily (without having to involve cascading deletes)   

```javascript
function openDbConnection(connectionString) {
    var scope = this;
    var connection = openConnection(connectionString);
    
    scope.$cleanup(function () {
        connection.close();
    })
}    
``` 

* `$cleanup.push(Function)` - The same as `$cleanup()`, except the task is added to the end of the list. 
* `$_` - Provides access to the [underscorejs](http://underscorejs.org/) helper library.
* `$Promise` - Provides access to the [bluebirdjs](http://bluebirdjs.com/) promise library. Note that you
are not forced to use this library for promises.
* `$id()` - Returns a new unique ID, `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` where `x` is a hexidecimal value

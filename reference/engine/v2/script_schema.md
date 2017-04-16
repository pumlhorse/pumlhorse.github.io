---
title: Script Schema
layout: referencev2
---

# Script Schema

This is the schema for `.puml` scripts. The sections can be declared in any order, but it is good practice to use this order.

```yaml
name: Name of script
functions:
  <function_definition>
modules:
  - <module_definition>
expects:
  - <variable_name>
steps:
  - <step>
cleanup:
  - <cleanup_step>
```

## `name` _(required)_

Each script must have a name, though it does not need to be unique.

## `functions`

Occasionally, you may wish to move repetitive code into its own function. This can improve the readability of the script steps. For example, your script could include a function to convert a string to base64 (ignoring the fact that the built in `convert` function does this):

```yaml
name: Convert some text
functions:
  toBase64:
    - input # each parameter is an array item
    - return new Buffer(input, 'utf8').toString('base64') # Function declaration goes last
steps:
  - nameInBase64 = toBase64: John Smith
  - emailInBase64 = toBase64: jsmith@example.org
  - log: Why not log some base64 values? $nameInBase64, $emailInBase64
```

## `modules`

Scripts can reference Pumlhorse modules to use additional functionality. For instance, if many scripts use the same functions, it may make sense to move them to a module. Also, features like database access are implemented in external modules.

```yaml
name: Reference external modules
modules:
  - myModule
```

The above example will load the "myModule" module from the appropriate `node_modules` or `puml_modules` folder. It assumes that both the file and the module are named "myModule".

### Reference a module with a different filename

```yaml
name: Reference a module with a different name
modules:
  - myModuleName: myFileName
```

### Reference a module with a specific path

```yaml
name: Reference a module with a specific path
modules:
  - myModule: ../../myFile.js
```

### Reference a module with a namespace

Many times you may want to load a module into a namespace to avoid function name conflicts, or to provide clarity.

```yaml
name: Use namespace modules
modules:
  - twitter = myTwitterModule
  - facebook = myFacebookModule
steps:
  - twitter.post: This is my tweet!
  - facebook.post: This is my status!
```

## `expects`

Some scripts may depend on having certain values passed into them via `context`s. It's a good practice to declare these in the `expects` section. This serves multiple purposes; it ensures that preconditions are met before running the script (e.g. don't create a user without `password` set). It also gives the person reading the script an idea of what is needed.

```yaml
name: Login user
expects:
  - username
  - password
  - url
steps:
  - http.post: 
      url: $url/login
      data:
        username: $username
        password: $password
```

## `steps` _(required)_

Each script must have at least one step.

## `cleanup`

The `cleanup` section is similar to `steps`, except that each `cleanup` step will run regardless of whether the previous step succeeded. This is used perform any necessary actions at the end of the script, like deleting test data.
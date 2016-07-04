---
title: Using modules
layout: reference
---
# Using modules

Pumlhorse supports using external modules to complement the standard functionality. These come in two types: Node, and Pumlhorse.
Node modules are anything you would find from [NPM](https://www.npmjs.org). They might not be fully compatible with Pumlhorse scripts 
(for instance, they may depend on callbacks instead of Promises). Pumlhorse modules are specifically crafted to be compatible.

## Importing a Node module

Pumlhorse can import any Node module by using the `import` function and assigning it to a variable.

```yaml
name: Import Node module
description: Import a plain Node module (not a Pumlhorse module)
steps:
  # Log OS information using the Node 'os' module
  - osMod = import: os
  - log: OPERATING SYSTEM: $osMod.type() $osMod.arch()
  - log: CPU CORES: $osMod.cpus().length
  - log: TOTAL MEMORY: ${ osMod.totalmem() / 1000000 } MB
  # Import a custom Node module (examples/node_modules/plainNodeModule.js)
  - custom = import: plainNodeModule
  - currentDate = ${ Date() }
  - log: $custom.getModuleInfo(currentDate)
```

## Pumlhorse Modules

_Want to create your own custom module? See the [custom modules](../modules/customModules) section._

To use a Pumlhorse module, you need to reference it in the `modules` section of the script.

```yaml
name: Use my custom module
modules:
  - pumlhorse_module_name: node_module_name   
steps:
  - #do something
```

This would attempt to resolve "node_module_name" in the following locations:

* Globally installed `node_modules` folder
* `node_modules` folders along the `require` dependency resolution path **relative to the executing directory**
(where `pumlhorse` was called, not where the `.puml` file lives
* `puml_modules` folder in the `.puml` file's directory or parent directories

Once "node_module_name" is loaded, Pumlhorse will then load the "pumlhorse_module_name" module.

For example, let's say a user created an NPM package called `i_love_pumlhorse` that defines three Pumlhorse modules: 
`facebook_stuff`, `twitter_things`, and `instagram_bits`. If a script only wants to interact with Facebook and Twitter,
it could do the following:

```yaml
name: Post something to Facebook
modules:
  - facebook_stuff: i_love_pumlhorse
  - twitter_things: i_love_pumlhorse
steps:
  - postUpdateToFacebook: I'm using Facebook!
  - postUpdateToTwitter: I'm using Twitter too!
```

If your Pumlhorse module has the same name as the Node module, you can use this shortcut:

```yaml
name: Use a shortcut
modules:
  - cool_module # Loads "cool_module" Node module and uses the "cool_module" Pumlhorse module
```

## Direct path

You can also load a module by using an absolute path

```yaml
name: Use a file with an absolute path
modules:
  - my_module: C:/puml_modules/my_local_module.js
```

or a relative path

```yaml
name: Use a file with a relative path
modules:
  - my_module: ../../puml_modules/my_local_module.js
```

## Namespaces

Sometimes you may wish to assign a namespace to a module, for clarity's sake.
This can help if multiple modules have similar functions.

```yaml
name: Post something to Facebook
modules:
  - fb = facebook_stuff: i_love_pumlhorse
  - tw = twitter_things: i_love_pumlhorse
steps:
  # Both modules have "postUpdate" functions, so we need to use the namespace
  - fb.postUpdate: I'm using Facebook!
  - tw.postUpdate: I'm using Twitter too!
```

For information on creating custom modules, see the [custom modules](../modules/customModules) page
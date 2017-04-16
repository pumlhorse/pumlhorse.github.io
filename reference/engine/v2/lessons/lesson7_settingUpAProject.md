---
title: Setting Up a Project
layout: lesson
---

# Setting Up a Project

Typically, Pumlhorse scripts are one part of a larger project. There is no required structure for such a project, but this lesson describes one possible setup. For demonstration purposes, we will build a test suite for an imaginary website for recipes.

This imaginary site has webpages that we want to test, as well as an HTTP API. We'll organize everything under the `RecipeSite` folder.

```
RecipeSite
|___contexts
|   |___context.local.yaml
|   |___context.dev.yaml
|   |___context.prod.yaml
|___tests
|   |___ui
|   |   |___login
|   |   |   |___successfulLogin.puml
|   |   |   |___badPassword.puml
|   |   |   |___userDoesNotExist.puml
|   |   |___search
|   |   |___createRecipe
|   |   |___puml_modules
|   |___api
|       |___recipe
|       |   |___create_invalidData_returns400.puml
|       |   |___delete_recipeExists_returns204.puml
|       |___puml_modules
|___RecipeSite.local.pumlprofile
|___RecipeSite.dev.pumlprofile
|___RecipeSite.prod.pumlprofile
```

Directly under the RecipeSite folder, we have the `contexts` and `tests` directories and a few `.pumlprofile` files. **Context** files contain data that can change across environments or other use cases. Usually this consists of URLs, database connection strings, usernames, etc. The `tests` directory contains the actual tests, separated into `ui` and `api` folders. The `.pumlprofile` files are **Profile** definitions, which are similar to contexts in that they differentiate behavior between use cases. In this case, the profiles could be as simple as including the appropriate context file. However, it could be the case that we don't want certain tests to run in production. Perhaps we want to reduce the number of scripts that run at one time to avoid overloading a certain server. These profile files can help facilitate that.

Underneath the `ui` and `api` folders, we have the actual `.puml` files. There's plenty of flexibility for defining folder structure and naming conventions, so just go with what makes sense for your team.

## Custom modules

In the example above, you can see `puml_modules` folders. These contain [custom modules](../modules.md) pertinent to the test suites. If you are writing your own custom module and you want it to live in the project (as opposed to a separate repository), it's best to use the `puml_modules` folder, rather than the typical `node_modules` folder. This will help make sure it gets checked in to source control correctly, and provides better clarity. You can still use external modules via the `node_modules` folders and [npm](https://npmjs.org) alongside `puml_modules` folders. **Note:** If you have the same module in both folders, Pumlhorse's module resolution algorithm will give preference to `puml_modules` over `node_modules`.

The advantage to using `puml_modules` or `node_modules` folders is that you don't have to worry about providing a path to the file. Pumlhorse will search for the file in those folders automatically using the Node resolution method. That is, in the script `myTests/RecipeSite/tests/ui/login/successfulLogin.puml`, it will look in the following folders (in order):

* `myTests/RecipeSite/tests/ui/login/puml_modules`
* `myTests/RecipeSite/tests/ui/login/node_modules`
* `myTests/RecipeSite/tests/ui/puml_modules`
* `myTests/RecipeSite/tests/ui/node_modules`
* `myTests/RecipeSite/tests/puml_modules`
* `myTests/RecipeSite/tests/node_modules`
* `myTests/RecipeSite/puml_modules`
* `myTests/RecipeSite/node_modules`
* `myTests/puml_modules`
* `myTests/node_modules`

In general, you should keep the modules at the lowest level possible (closest to the scripts that use it)

## Partial scripts

An alternative (or addition) to custom modules is using "partial" scripts. These scripts serve as reusable components that can be used across many scripts. See the [run](../functions/flowControl#run) documentation for more details.
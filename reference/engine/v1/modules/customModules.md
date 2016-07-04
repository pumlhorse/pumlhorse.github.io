---
title: Custom Modules
layout: reference
---
# Creating a custom module

If you find yourself writing the same function over and over in your scripts, you
might consider moving them to a module. All of Pumlhorse's functions are written as
modules.

To write a module, create a new JavaScript file and copy your functions there.
Then register your module with Pumlhorse as such:

```javascript
var mod = pumlhorse.module("myModule")
    .function("myFunc", myFunc)
    .function("myFunc2", myFunc2)

//export as a Node.js module
module.exports = mod.asExport()

function myFunc() {
    //snip
}

function myFunc2() {
    //snip
}
```

Now you need to place your file someplace where pumlhorse can reach it.

 * If you are using an absolute or relative path, you can place it anywhere you would like
 * If you are using a node module with a single file, you can simply place it in a `node_modules` folder along the resolution path.
 * If you are using a node module with multiple files, you will need to place it in its own folder with whatever name you
 want for the module. You will also need to create a `package.json` for it.
   * For example, we want to create "cool_module", so we put our code into `app.js`.
   * We create a folder `cool_module` and move `app.js` into it.
   * `app.js` needs `library.js`, so we move that into the new folder as well.
   * We then create a file `package.json` in the same folder. In `package.json`, 
   we write the following: `{ "main": "app.js" }`
   * Finally, we move the `cool_module` folder into a `node_modules` folder in the same directory
   as our scripts.

### Directory structure

As mentioned earlier, Pumlhorse will look at parent directories to find a node module. This allows you to
share modules across many scripts, or limit it to a smaller set. Here is a sample directory structure.

```
pumlhorse_scripts
├───login
│   │   login_test1.puml
│   │   login_test2.puml
│   │   login_test3.puml
│   │
│   └───node_modules
│       └───login_helpers
│               app.js
|               library.js
│               package.json
│
├───node_modules
│       site_helpers.js
│
└───user_management
        user_management_test1.puml
        user_management_test2.puml
        user_management_test3.puml
```

In this example, the scripts in the `login` directory will have access to both 
the `login_helpers` and `site_helpers` modules. The `user_management` tests will only
have access to `site_helpers`.


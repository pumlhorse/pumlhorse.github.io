---
title: Profile File Schema
layout: referencev2
---

# Profile File Schema

This is the schema for Pumlhorse profile files (`.pumlprofile`). Profile files are in YAML format.

* **include** - This array lists the files and/or directories to run
* **contexts** - The context file(s) to use
* **isRecursive** - Whether or not to search directories recursively
* **maxConcurrentFiles** - Maximum number of scripts to run in parallel.
* **isVerbose** - If true, Pumlhorse will display more detail about errors.
* **modules** - Pumlhorse modules to automatically include in each script.

Here is an example file:

```yaml
include:
  - one_file.puml
  - another_file.puml
  - a_script_directory
  - another_directory
contexts:
  - dev_urls.yaml
  - dev_db_strings.yaml
isRecursive: true
maxConcurrentFiles: 5
isVerbose: true
modules:
  - name: myModule
    path: .path/to/Module
  - name: myModule2
    path: .path/to/Module2
```


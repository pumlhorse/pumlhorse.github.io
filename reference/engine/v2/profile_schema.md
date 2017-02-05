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
```


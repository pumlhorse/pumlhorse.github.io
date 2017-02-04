---
title: Advanced Command Line Usage
layout: lesson
---

## Advanced Command Line Usage

Up until now we haven't done much with the Command Line Interface (CLI) except run a single file. However, the CLI
is capable of a lot more.

## Run multiple files

* Run multiple files: `pumlhorse file1.puml file2.puml file3.puml`
* Run all `.puml` files in directory(s): `pumlhorse my_scripts_dir my_other_scripts_dir`
* Run all `.puml` files in a directory and all its sub-directories: `pumlhorse my_scripts_dir --recursive`

## Run with a given set of data

Pumlhorse also supports _context files_. These are data files that can be accessed by scripts.
For instance, it's common to extract things like server names or connection strings into configuration files

**context.dev.yaml**

```yaml
urlBase: http://dev.example.org
dbName: dev.myDatabase.name
```

**myScript.puml**

```yaml
name: Use data from context file
steps:
  - response = http.get: $urlBase/myEndpoint
  - connectToDatabase: $dbName
```

We can then run this (hypothetical) script with the context file: `pumlhorse myScript.puml -c context.dev.yaml`
Conceivably, you would have separate context.*.yaml files for other environments.

If you prefer, context files can also be in JSON format.

## Script queueing

If you run a large number of scripts at a time, you may wish to control how many to run concurrently. For example,
you may wish to avoid overloading a website or hanging database connections.

`pumlhorse my_big_script_dir --max-concurrent 4` will run all the `.puml` files in `my_big_script_dir`, but only four at a time.

If you do not specify this, the default value is _15_

## Profiles

With this much customizability, using the command line can be overwhelming. Thus, Pumlhorse allows the use of _profiles_
to manage run configurations. A profile is a YAML file that specifies the options above. It must end in the extension
".pumlprofile". See the full schema [here](../profile_schema.md)

To run Pumlhorse with a profile, use the following syntax: `pumlhorse --profile my_profile.pumlprofile`



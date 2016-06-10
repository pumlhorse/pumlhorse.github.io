---
title: Command Line
layout: reference
---

# Command-line Interface

Pumlhorse supplies a command-line interface (CLI) for running .puml files. To install the CLI, run `npm install -g pumlhorse`

## Usage

`pumlhorse <files_or_dirs> [-rv] [-c <context_file>] [--profile <profile_file>] [--max-concurrent <number>] [--sync]`


| Command | Description |
| --------------------- |
| `pumlhorse` | Run all `.puml` files in the current directory |
| `pumlhorse somedir` | Run all `.puml` files in `someDir` |
| `pumlhorse myScript.puml` | Run `myScript.puml` |
| `pumlhorse somedir myScript.puml` | Run all `.puml` files in `someDir` and `myScript.puml` |
| `pumlhorse -v` | Print the current version |

## Recursively searching directories

| Command | Description |
| --------------------- |
| `pumlhorse somedir -r` | Run all files in `somedir` and its sub-directories (recursive). |


**Note** be aware of what directories you're using when using `-r`. If you run it on a folder that has a lot 
of files or folders that don't contain .puml files, it will unnecessarily take more time to run.

## Running synchronously

By default, Pumlhorse will run all scripts asynchronously. This means that if one script is waiting for a long running
process -- an HTTP GET, for example -- other scripts can run at the same time. The output will be shown in order
of the scripts completing. If you care about the order in which the scripts are run, you can use the `--sync` option
to run scripts synchronously.

`pumlhorse firstScript.puml secondScript.puml thirdScript.puml --sync`

## Limiting concurrent scripts

If you want to limit the number of scripts that run at one time (say, to avoid overloading a service or database), 
you can use the `--max-concurrent <max_number_of_scripts>` option.

`pumlhorse myScriptDirectory --max-concurrent 5`
        
## Running with contexts

When running a script, you can also pass in a context - a collection of initial variables. For example, you could
pass in configuration options like:

* URL of API to hit, which changes between environments
* Number of times to run part of a test
* Login credentials

These values are then referenced like normal variables in the script. To use a context with the CLI, use the `-c fileName`
option. JSON (.json) and YAML (.yml) files are supported. 

`pumlhorse myScript1.puml myScript.2.puml -c context_PROD.json`

### Multiple contexts

Occasionally, it may make sense to split your context into multiple files. Pass additional contexts in the same way.

`pumlhorse myScript1.puml -c context1.yml context2.yml`

Note that values will be applied in the order that context files are given. That is, if both context files above contain a "username" value, 
context2.yml will override the value in context1.yml

## Running a profile

All of the above CLI parameters can be packaged into a "profile". This profile is a .pumlprofile file, and would look something like this:

```yaml
include: # array of .puml files and/or directories to run
  - file1.puml
  - scriptDir
contexts: # array of .json or .yml files to use as the context
  - context1.yml
  - context2.json
filters: # array of .js files to use as filters (see the Filters page for info)
  - filter1.js
  - filter2.js
recursive: <bool>
synchronous: <bool>
maxConcurrent: <number>
```

To pass a profile, use the `--profile <profile_name>` option

`pumlhorse --profile myProfile.pumlprofile`

You can still pass additional parameters when using a profile. 
If you pass files or directories or context files, they will be added to the profile. 
The `-r`, `--sync`, and `--max-concurrent` options will override any values in the profile
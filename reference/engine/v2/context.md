---
title: Context
layout: referencev2
---

# Context

A `context` is a set of data that is passed to one or more scripts. Typically, this is data that can vary from situation to situation, like URLs for various environments. You may have a `loginUrl` that could be https://dev.login.example.org, https://staging.login.example.org, etc.

Multiple contexts can be passed at a time, which can be helpful for separating different pieces of data.

## Example

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

If you prefer, context files can also be `.json` files.
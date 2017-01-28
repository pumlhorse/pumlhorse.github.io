---
title: HTTP Functions
layout: lesson
---

One of the primary reasons for creating Pumlhorse was to easily interact with web-based APIs. Thus Pumlhorse
comes with HTTP functions baked in. These functions provide the ability to perform web requests (GET, POST, PUT, etc.)
and to interact with the responses.

```yaml
name: Perform a basic GET
steps:
  - response = http.get: http://www.google.com/
  - http.isOk: $response # Verify we got back a 200 OK
  - log: $response.body # Print out the body of the response we got
```

Many web APIs will return data in JSON form, so Pumlhorse gives an easy way to read and write it.

```yaml
name: Get JSON data from response
steps:
  - response = http.post:
      url: http://www.example.org/someApiEndpoint
      data: # this object will be transformed to JSON
        prop1: value1
        obj1:
          nestedData: value2
    # Performs a POST with a payload of '{ "prop1": "value1", "obj1": { "nestedData": "value2" }}'
  - http.isOk: $response
  - jsonBody = http.body: $response # Reads $response.body and transforms it to an object
  - log: $jsonBody.someValueInResponse
```

In the first example, we just passed a URL to the `get` function. In the second example, 
we had a payload to send, so we specified the `url` and `data` parameters for the `post` function.
In fact, `get`, `post`, `put`, and `delete` all accept these parameters. For `get` and `delete` 
(which do not pass paylods), the `data` parameter will be transformed to a query string.

```yaml
name: Pass query string parameters
steps:
  - response = http.get:
      url: http://www.example.org/noSuchPage
      data:
        page: 14
        search: some search term
  # Performs GET to http://www.example.org?noSuchPagepage=14&search=some%20search%20term
  - http.isNotFound: $response # Asserts that the previous call returned a 404 Not Found
```

In addition to the `url` and `data` parameters, we can pass `headers`. Pumlhorse will provide basic
headers like `Content-Length`, `User-Agent`, and `Host`. You can pass additional headers, like `Authorization`.

```yaml 
name: Pass some headers
steps:
  - response = http.get:
      url: http://www.example.org/myAccountInfo
      headers:
        Authorization: Basic cHVtbGhvcnNlOmh1bnRlcjI=
```

See the HTTP module reference for all the available functions
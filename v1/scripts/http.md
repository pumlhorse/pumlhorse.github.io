---
title: HTTP methods
layout: reference
---
# HTTP methods

Pumlhorse contains functions to help with JSON-based HTTP requests.
It supports the following methods:

* `http.get`
* `http.put`
* `http.post`
* `http.delete`
* `http.head`
* `http.options`
* `http.patch`

Each method takes the following parameters:

| Parameter | Description |
|-----------|-------------|
| `url` | **(Required)** The destination URL, without the query string if `data` is passed |
| `data` | For `GET`, `DELETE`, `HEAD`, and `OPTIONS`, this will be transformed into the query string. Otherwise, it will be sent as a JSON body |
| `headers` | Any additional headers needed. |

Each method also returns a `Response` object:

| Property | Description |
| -------- | ----------  |
| `statusCode` | HTTP status code (e.g. 200, 400, 404, etc) |
| `statusMessage` | HTTP status message (e.g. "OK", "Bad Request", "Unauthorized") |
| `headers` | Response headers |
| `body` | String content of the response |

Most of the time, the response body is JSON. The `http.body` function will attempt
to convert it to an object.

There is also a set of assertions for HTTP responses

| Function | Description |
| ---------------------- |
| `http.isInformational` | Verifies that the response code is between 100 and 199 |
| `http.isSuccess` | Verifies that the response code is between 200 and 299 |
| `http.isRedirect` | Verifies that the response code is between 300 and 399 |
| `http.isError` | Verifies that the response code is between 400 and 599 |
| `http.isOk` | Verifies that the response code is 200 |
| `http.isNotModified` | Verifies that the response code is 304 |
| `http.isBadRequest` | Verifies that the response code is 400 |
| `http.isUnauthorized` | Verifies that the response code is 401 |
| `http.isForbidden` | Verifies that the response code is 403 |
| `http.isNotFound` | Verifies that the response code is 404 |
| `http.isNotAllowed` | verifies that the response code is 405 |

## Examples

```yaml
name: Get user info
steps:
  - searchUsersResponse = http.get:
      url: https://www.example.org/api/findUser
      data:
        search: jsmith
        page: 2
  # Issues GET request to https://www.example.org/api/findUser?search=jsmith&page=2
  - http.isOk: $searchUsersResponse
  - userList = http.body: $searchUsersResponse
  - contains:
      array: $userList.users
      value:
        username: jsmith
        email: jsmith@example.org
```

```yaml
name: Add user fails if user already exists
steps:
  - addUserResponse = http.post:
      url: https://www.example.org/api/user
      data:
        firstName: Alan
        lastName: Turing
        email: aturing@example.org
      headers:
        Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyaWQiOjQyLCJ1c2VybmFtZSI6ImVhc3Rlci5lZ2cifQ.U4nejjGFvXSERKtVWrgXXytXqe9oqdg8ws1AyLCp4o0
  #Issues POST request to https://www.example.org/api/user with payload of {"firstName":"Alan","lastName":"Turing","email":"aturing@example.org"}
  #Also includes a JSON Web Token in the Authorization header
  - http.isBadRequest: $addUserResponse
  - body = http.body: $addUserResponse
  - contains:
      array: $body.errors
      value: User already exists
```

## Going further
When developing tests against web APIs, you'll frequently need to support multiple environments.
The easiest way to accomplish this is with context/profile files. You can then reference the base URL
just like any other variable

```yaml
name: Use a variable context
steps:
  - http.get:
      url: $baseApiUrl/user/findUser # baseApiUrl evaluates to "https://www.example.org/api"
      data:
        search: jsmith
        page: 2 
```

---
title: HTTP Functions
layout: functionv2
---

## HTTP Functions

All HTTP functions are contained in the `http` namespace, thus they are prefixed with the notation `http.`.

## Request functions

The following functions perform HTTP requests and return responses. 

* `get`
* `post`
* `put`
* `delete`
* `patch`
* `options`
* `head`

All functions take the same parameters

#### Parameter `url`

Provides the address for the request. This is the only required parameter for the function.

#### Parameter `data`

Data to be sent in the request. For methods with bodies (i.e. `post`, `put`, `patch`), this will be serialized
as JSON. For methods with query strings (i.e. `get`, `delete`, `head`, `options`), this will be converted to 
query string parameters.

```yaml
  # Sends GET request to http://www.example.org?val1=test
- http.get:
    url: http://www.example.org
    data:
      val1: test
  # Sends POST request to http://www.example.org with body {"val1":"test"}
- http.post:
    url: http://www.example.org
    data:
      val1: test
```

#### Parameter `headers`

Key-value pairs for headers to be sent in the request

```yaml
# Sends GET request with Authorization header set to "Basic cHVtbGhvcnNlOmh1bnRlcjI="
- http.get:
    url: http://www.example.org
    headers:
      Authorization: Basic cHVtbGhvcnNlOmh1bnRlcjI=
```

**Note**: For requests that don't require the `data` or `headers` parameters,
you can use the following single-line shortcut:

```yaml
- http.get: http://www.example.org
```

## Response functions

The above functions will return an object containing response data. The following functions interact
with that response data.

### `dump`

This function takes a response as its only parameter and prints out information about the request.
This can be helpful for debugging.

```yaml
- response = http.get: http://www.example.org
- http.dump: $response
```

### `body`

For responses that contain a JSON payload, this function can read that payload and convert it to an object.

```yaml
- response = http.get: http://www.example.org/myUser
- body = http.body: $response
- log: My username is $body.username
```

If the response's payload is not JSON, it can be read as a string via like so:

```yaml
response = http.get:
  url: http://www.example.org/myUser
  headers:
    Content-type: text/xml
bodyAsString = $response.body
```

The `body` function is really just a helper for the following calls:

```yaml
- response = http.get: http://www.example.org/myUser
- body = fromJson: $response.body
```

### Response Code Assertions

The following functions are assertions for the HTTP response code. If the response's code
is invalid, an assertion error will be raised and execution will stop.


Function | Valid code(s)
---------|----------
 `isInformational` | 100-199 
 `isSuccess` | 200-299 
 `isOk` | 200
 `isRedirect` | 300-399
 `isNotModified` | 304
 `isError` | 400-599
 `isBadRequest` | 400
 `isUnauthorized` | 401
 `isForbidden` | 403
 `isNotFound` | 404
 `isNotAllowed` | 405 

```yaml
- response = http.get: http://www.example.org/missingPage
- http.isNotFound: $response
- log: Verified response was a 404
```
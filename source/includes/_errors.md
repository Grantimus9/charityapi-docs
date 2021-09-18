# Errors


> Success with "data" key:

```json
{
  "data": {
    "successful": "response",
    "goes": "here"
  }
}
```

> Error with "error" key:

```json
{
  "error": {
    "some": "error",
    "information": "here"
  }
}
```

Successful responses will always be JSON and include a "data" key.

Data and recoverable errors will be provided in an "error" key.

### HTTP Error Codes

Errors will include proper HTTP status codes.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong or missing. The API key should be included in an HTTP header called 'apikey'.
403 | Forbidden -- Forbidden
404 | Not Found -- The specified resource could not be found.
500 | Internal Server Error -- We had a problem with our server. Our error logging has caught it. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later. (note: this should never happen)

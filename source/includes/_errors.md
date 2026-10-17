# Errors

## Error Response

> Validation error:

```json
{
  "message": "An error occurred while validating the input.",
  "code": "validation_error",
  "details": {
    "email": ["Enter a valid email address."]
  },
  "status_code": 400
}
```

> Expired or invalid token:

```json
{
  "message": "An error occurred while processing the request.",
  "code": "token_not_valid",
  "details": "Token is invalid or expired",
  "status_code": 403
}
```

> Error raised by an endpoint:

```json
{
  "message": "Group not found",
  "code": "group_not_found_error",
  "details": "Group with id 1001 not found.",
  "status_code": 422
}
```

Errors returned by the API have this structure:

| Attribute     | Type                         | Description                                                         |
|---------------|------------------------------|---------------------------------------------------------------------|
| `message`     | _string_                     | Short description of the error.                                     |
| `code`        | _string_                     | Machine-readable code. Use it, not `message`, in your error handling. |
| `details`     | _string_, _object_ or _list_ | More information. For `validation_error`, an object keyed by field name, or a list. |
| `status_code` | _integer_                    | The HTTP status code, repeated.                                     |

A few endpoints answer an invalid request with a `400` whose body is a plain JSON string instead of this object.

## HTTP Status Codes

| Status | Codes | Meaning |
|--------|-------|---------|
| 400 | `validation_error`, or a code specific to the endpoint | The request is invalid. `details` says which field is wrong. |
| 403 | `token_not_valid` | The access token is expired or invalid. Refresh it, see [Handling expired tokens](#handling-expired-tokens). |
| 403 | `not_authenticated` | The `Authorization` header is missing. |
| 403 | `authentication_failed` | Wrong API key or pass key at `/token/`. |
| 403 | `permission_denied`, or a code specific to the endpoint | Your account is not allowed to do this, for example a user management call from an account that is not an admin. |
| 404 | `not_found` | The object does not exist in your active group, or the page number is out of range. |
| 405 | `method_not_allowed` | The endpoint does not support this HTTP method. |
| 406 | `not_acceptable` | The `Accept` header asks for a format other than JSON. |
| 409 | A code specific to the endpoint | The request conflicts with existing data. |
| 413 | `request_body_too_large` | The request body is larger than 128 MB. |
| 422 | A code specific to the endpoint, for example `group_not_found_error` or `bigquery_job_failed` | The request is valid but cannot be processed. `bigquery_job_failed` means the computation failed; retry later. |
| 429 | `throttled` | Too many requests. Wait for the number of seconds in the `Retry-After` header. See [Rate limits](#rate-limits). |
| 500 | `server_error` | Unexpected server error. Try again later. |
| 502, 504 | none | Returned by the gateway, not the API. The body may not be JSON. A request that runs longer than 300 seconds fails with one of these. |
| 503 | `attribute_config_unavailable` | The service is temporarily unavailable. Try again later. |

The endpoints in this reference do not return `401`. Authentication errors use `403`.

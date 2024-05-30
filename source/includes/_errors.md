# Errors
## HTTP Status Codes
The Allphins API uses the following HTTP error codes:


| Error Code | Meaning                                                                                   |
|------------|-------------------------------------------------------------------------------------------|
| 400        | Bad Request -- Your request is invalid.                                                   |
| 401        | Unauthorized -- Your API token is wrong.                                                  |
| 403        | Forbidden -- You do not have access to this object.                                       |
| 404        | Not Found -- The specified object could not be found.                                     |
| 405        | Method Not Allowed -- You tried to access an object with an invalid method.               |
| 406        | Not Acceptable -- You requested a format that isn't json.                                 |
| 422        | Unprocessable Content -- We are unable to process the request.                            |
| 429        | Too Many Requests -- You're requesting too many object.                                   |
| 500        | Internal Server Error -- We had a problem with our server. Try again later.               |
| 503        | Service Unavailable -- We're temporarily offline for maintenance. Please try again later. |

## Error Response

In addition, every request in error respect the following structure:


| Attribute     | Type      | Description                        |
|---------------|-----------|------------------------------------|
| `message`     | _string_  | Message describing the error.      |
| `code`        | _string_  | Unique code to identify the error. |
| `details`     | _string_  | More details if available.         |
| `status_code` | _integer_ | HTTP response code.                |

> Example

```json
{
  "message": "Group not found",
  "code": "group_not_found_error",
  "details": "Group not found",
  "status_code": 422
}
```

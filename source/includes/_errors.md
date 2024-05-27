# Errors

The Allphins API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API token is wrong.
403 | Forbidden -- You do not have access to this object.
404 | Not Found -- The specified object could not be found.
405 | Method Not Allowed -- You tried to access a object with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The object requested has been removed from our servers.
429 | Too Many Requests -- You're requesting too many object.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

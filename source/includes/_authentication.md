# Authentication

> Send the access token in every request:

```shell
curl https://api.allphins.com/api/v1/users/ \
  -H "Authorization: Bearer YourAccessToken"
```

> Replace `YourAccessToken` with your access token.

The API uses JSON Web Tokens. Exchange your API key and pass key for two tokens:

| Token | Lifetime | Use |
|-------|----------|-----|
| Access token | 15 minutes | Sent in the `Authorization` header of every request. |
| Refresh token | 1 day | Exchanged for a new access token at `/token/refresh/`. |

Send the access token in a header that looks like this:

`Authorization: Bearer YourAccessToken`

### Handling expired tokens

An expired, invalid or missing access token returns HTTP `403`, not `401`:

- `403` with code `token_not_valid`: the token is expired or invalid. Get a new access token from `/token/refresh/` and retry the request.
- `403` with code `not_authenticated`: the `Authorization` header is missing.

When the refresh token itself has expired (after 1 day), `/token/refresh/` also returns `403` `token_not_valid`. Request a new pair of tokens from `/token/` with your API key and pass key.

> Response for an expired or invalid token:

```json
{
  "message": "An error occurred while processing the request.",
  "code": "token_not_valid",
  "details": "Token is invalid or expired",
  "status_code": 403
}
```

The token endpoints accept up to 20 requests per minute from the same IP address. See [Rate limits](#rate-limits).

## Generate Token

Returns a new access token and refresh token for your API credentials.

> Example request:

```shell
curl https://api.allphins.com/api/v1/token/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"api_key": "your_api_key", "pass_key": "your_pass_key"}'
```

> Example response:

```json
{
  "access": "string.string.string",
  "refresh": "string.string.string"
}
```

### HTTP Request

`POST https://api.allphins.com/api/v1/token/`

### Payload

| Argument   | Type     | Required | Description    |
|------------|----------|----------|----------------|
| `api_key`  | _string_ | yes      | Your API key.  |
| `pass_key` | _string_ | yes      | Your pass key. |

A missing `api_key` or `pass_key` returns `400` `validation_error`. Wrong credentials return `403` `authentication_failed`.

## Refresh Token

Returns a new access token for a valid refresh token. The refresh token is not renewed: it expires 1 day after it was issued, whatever the number of refreshes.

> Example request:

```shell
curl https://api.allphins.com/api/v1/token/refresh/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"refresh": "string.string.string"}'
```

> Example response:

```json
{
  "access": "string.string.string"
}
```

### HTTP Request

`POST https://api.allphins.com/api/v1/token/refresh/`

### Payload

| Argument  | Type     | Required | Description         |
|-----------|----------|----------|---------------------|
| `refresh` | _string_ | yes      | Your refresh token. |

An expired, invalid or blacklisted refresh token returns `403` `token_not_valid`.

## Verify Token

Checks whether a token is valid.

> Example request:

```shell
curl https://api.allphins.com/api/v1/token/verify/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"token": "string.string.string"}'
```

> Example response:

```json
{}
```

If the token is valid, the response is an empty JSON object with status `200`.

If the token is invalid or expired, the response is `403` with code `token_not_valid`.

### HTTP Request

`POST https://api.allphins.com/api/v1/token/verify/`

### Payload

| Argument | Type     | Required | Description                               |
|----------|----------|----------|-------------------------------------------|
| `token`  | _string_ | yes      | The access token or refresh token to check. |

## Blacklist Token

Revokes a refresh token. It can no longer be used at `/token/refresh/`.

An access token cannot be blacklisted. Access tokens already issued stay valid until they expire, at most 15 minutes later.

> Example request:

```shell
curl https://api.allphins.com/api/v1/token/blacklist/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"refresh": "string.string.string"}'
```

> Example response:

```json
{}
```

### HTTP Request

`POST https://api.allphins.com/api/v1/token/blacklist/`

### Payload

| Argument  | Type     | Required | Description                     |
|-----------|----------|----------|---------------------------------|
| `refresh` | _string_ | yes      | The refresh token to blacklist. |

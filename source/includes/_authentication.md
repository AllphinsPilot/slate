# Authentication


> To authorize, use this code:


```shell
curl https://api.allphins.com/api/v1/assets/
  -H "Authorization: Bearer YourAccessToken"
```

> Make sure to replace `YourAccessToken` with your API token.

Allphins follows recommendations from OAuth2 and uses access tokens and refresh tokens to manage access to the API.
You need to dynamically generate a short-lived access token and a long-lived refresh token that will be used to refresh your access token.

Allphins expects the API token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer YourAccessToken`

Allphins also provides endpoints to blacklist and verify a token. This is useful if you want to revoke access to a token before it expires or if you want to test your current token.




## Generate Token
Generates a new access token and refresh token based on user credentials.

The access token is valid for 1 hour and the refresh token is valid for 1 day.

> Example request:

```shell
curl https://api.allphins.com/api/v1/token/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"api_key": "your_api_key", "pass_key": "your_pass_key"}'
```

> Example Response:

  ```json
  {
    "access": "string.string.string",
    "refresh": "string.string.string"
  }
  ```

### HTTP Request

`POST https://api.allphins.com/api/v1/token/`

### Payload

| Argument           | Type     | Required | Description    |
|--------------------|----------|----------|----------------|
| `api_key`          | _string_ | yes      | Your API key.  |
| `pass_key`         | _string_ | yes      | Your pass key. |


## Refresh Token

> Example request:

```shell
curl https://api.allphins.com/api/v1/token/refresh/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"refresh": "string.string.string"}' 
```

> Example Response:

  ```json
  {
    "access": "string.string.string"
  }
  ```
Generates a new access token based on a refresh token.

The new access token is valid for 1 hour.


### HTTP Request

`POST https://api.allphins.com/api/v1/token/refresh/`

### Payload
| Argument  | type     | Required | Description        |
|-----------|----------|----------|--------------------|
| `refresh` | _string_ | yes      | Your refresh token |


## Verify Token
> Example request:

```shell
curl https://api.allphins.com/api/v1/token/verify/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"token": "string"}' 
```

> Example Response:

  ```json
  { }
  ```
Verifies if a token is valid.

If the token is valid, it returns an empty json object.

If the token is invalid or expired, it returns a 401 error.

### HTTP Request

`POST https://api.allphins.com/api/v1/token/verify/`

### Payload
| Argument | type     | Required | Description                |
|----------|----------|----------|----------------------------|
| `token`  | _string_ | yes      | The access token to verify |


## Blacklist Token
> Example request:

```shell
curl https://api.allphins.com/api/v1/token/blacklist/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"refresh": "string"}' 
```

> Example Response:

  ```json
  { }
  ```

Blacklists a refresh token. An access token cannot be blacklisted.

### HTTP Request

`POST https://api.allphins.com/api/v1/token/blacklist/`

### Payload
| Argument   | type     | Required | Description                    | 
|------------|----------|----------|--------------------------------|
|  `refresh` | _string_ | yes      | The refresh token to blacklist | 

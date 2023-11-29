# Authentication


> To authorize, use this code:


```shell
curl https://app.allphins.com/api/v1/assets/
  -H "Authorization: Bearer YourAccessToken"
```

> Make sure to replace `ENTERYOURAUTHTOKEN` with your API token.

Allphins follows recommendations from OAuth2 and uses access tokens and refresh tokens to manage access to the API.
You need to dynamically generate a short-lived access token and a long-lived refresh token that will be used to refresh your access token.

Allphins expects the API token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer YourAccessToken`

Allphins also provides endpoints to blacklist and verify a token. This is useful if you want to revoke access to a token before it expires or if you want to test your current token.




## Generate Token
Generates a new access token and refresh token based on user credentials.

The access token is valid for 5 minutes and the refresh token is valid for 1 day.

> Example request:

```shell
curl https://app.allphins.com/api/v1/token/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"username": "string", "password": "string"}'
```

> Example Response:

  ```json
  {
    "access": "string",
    "refresh": "string"
  }
  ```

### HTTP Request

`POST https://app.allphins.com/api/v1/token/`

### Payload

Argument | type   | Description | Mandatory
--------- |--------|-------------| -----------
username | string | username    | yes
password | string | password    | yes

## Refresh Token

> Example request:

```shell
curl https://app.allphins.com/api/v1/token/refresh/ \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"refresh": "string"}' 
```

> Example Response:

  ```json
  {
    "access": "string"
  }
  ```
Generates a new access token based on a refresh token.

The new access token is valid for 5 minutes.


### HTTP Request

`POST https://app.allphins.com/api/v1/token/refresh/`

### Payload
Argument | type   | Description        | Mandatory
--------- |--------|--------------------| -----------
refresh | string | Your refresh token | yes


## Verify Token
> Example request:

```shell
curl https://app.allphins.com/api/v1/token/verify/ \
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

`POST https://app.allphins.com/api/v1/token/verify/`

### Payload
Argument | type   | Description                | Mandatory
--------- |--------|----------------------------| -----------
token | string | The access token to verify | yes


## Blacklist Token
> Example request:

```shell
curl https://app.allphins.com/api/v1/token/blacklist/ \
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

`POST https://app.allphins.com/api/v1/token/blacklist/`

### Payload
Argument | type   | Description               | Mandatory
--------- |--------|---------------------------| -----------
refresh | string | The refresh token to blacklist | yes

# Users

## Get the current account

Returns the account that owns the access token, with its groups and its active group.

> Example request:

```shell
curl https://api.allphins.com/api/v1/users/ \
  -H "Authorization: Bearer YourAccessToken"
```

> Example response (abridged):

```json
{
  "pk": 42,
  "email": "api-energy@mycompany.com",
  "role": "technical_account",
  "family_id": "a6188aa6-bd57-41d1-8015-f0b6ee88cdec",
  "group_id": 1000,
  "group_name": "My Company Energy",
  "lob_active": "energy_v2",
  "placement_type": "treaty",
  "display_currency": "USD",
  "group_pool": [
    {
      "group_id": 1000,
      "line_of_business": "energy_v2",
      "placement_type": "treaty"
    },
    {
      "group_id": 1003,
      "line_of_business": "terror_v2",
      "placement_type": "treaty"
    }
  ]
}
```

### HTTP Request

`GET https://api.allphins.com/api/v1/users/`

The response is a single object, not a list. The main fields are:

| Field              | Description |
|--------------------|-------------|
| `pk`               | The id of the account. Use it to switch the active group. |
| `role`             | `admin`, `user`, `read_only` or `technical_account`. |
| `family_id`        | The id of your company, used by the user management endpoints. |
| `group_id`         | The active group. Every request is scoped to it. |
| `lob_active`       | The line of business of the active group. |
| `placement_type`   | The placement type of the active group: `treaty` or `direct_fac`. |
| `group_pool`       | The groups the account can use, with their line of business and placement type. |

The response contains more fields, used by the Allphins web application.

## Switch the active group

Changes the active group of the account. Every later request made with the account, with any access token, uses the new group.

> Example request:

```shell
curl https://api.allphins.com/api/v1/users/42/ \
  -X PATCH \
  -H "Authorization: Bearer YourAccessToken" \
  -H "Content-Type: application/json" \
  -d '{"group_active_id": 1003}'
```

The response is the account, in the same format as [Get the current account](#get-the-current-account), with `group_id` set to the new group.

### HTTP Request

`PATCH https://api.allphins.com/api/v1/users/:id/`

#### Path variable

`id`: the `pk` of your own account, from `GET /users/`.

### Payload

| Argument          | Type      | Required | Description |
|-------------------|-----------|----------|-------------|
| `group_active_id` | _integer_ | yes      | A `group_id` from your `group_pool`. |

### Errors

| Status | Code                    | Cause |
|--------|-------------------------|-------|
| 403    | `user_update_failed`    | `id` is not your own account. |
| 422    | `group_not_found_error` | The group is not in your `group_pool`. |

## Get the active group

> Example request:

```shell
curl https://api.allphins.com/api/v1/user_group/ \
  -H "Authorization: Bearer YourAccessToken"
```

> Example response:

```json
{
  "id": 1000,
  "name": "My Company Energy"
}
```

Returns the id and name of the active group.

### HTTP Request

`GET https://api.allphins.com/api/v1/user_group/`

## User management

These endpoints let an administrator of your company:

* list the lines of business and users of the company
* invite a collaborator
* change the role, lines of business or status of a user

They require an account with the `admin` role. Any other account, including a technical (API) account, gets `403` `permission_denied`.

Your `family_id` is in the response of [Get the current account](#get-the-current-account).

### Roles

| Role                | Description |
|---------------------|-------------|
| `admin`             | Administrator. Can use the user management endpoints. |
| `user`              | Standard user. |
| `read_only`         | Read-only user. |
| `technical_account` | Account used for API access. |

## Retrieve the company

Returns the lines of business of your company and its users.

> Example request:

```shell
curl https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/ \
  -H "Authorization: Bearer YourAccessToken"
```

> Example response:

```json
{
  "name": "My Company",
  "available_lobs": [
    {
      "group_id": 1000,
      "line_of_business": "energy_v2",
      "placement_type": "treaty"
    },
    {
      "group_id": 1001,
      "line_of_business": "aviation_war",
      "placement_type": "treaty"
    }
  ],
  "users": [
    {
      "id": 2,
      "first_name": "Alice",
      "last_name": "Doe",
      "email": "alice.doe@mycompany.com",
      "role": "user",
      "group_pool": [
        {
          "group_id": 1000,
          "line_of_business": "energy_v2",
          "placement_type": "treaty"
        }
      ],
      "is_active": true,
      "date_joined": "2025-09-10"
    }
  ]
}
```

### HTTP Request

`GET https://api.allphins.com/api/v1/families/:family_id/`

#### Path variable

`family_id`: the id of your company.

### Response

| Field            | Description |
|------------------|-------------|
| `name`           | The name of the company. |
| `available_lobs` | The groups of the company: `group_id`, `line_of_business` and `placement_type`. |
| `users`          | The users of the company, newest first. Same fields as [List users](#list-users). |

## List users

> Example request:

```shell
curl https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/users/ \
  -H "Authorization: Bearer YourAccessToken"
```

Returns the users of your company as a list, newest first. Each user has these fields:

| Field         | Type      | Description |
|---------------|-----------|-------------|
| `id`          | _integer_ | The id of the user. |
| `first_name`  | _string_  | First name. |
| `last_name`   | _string_  | Last name. |
| `email`       | _string_  | Email address. |
| `role`        | _string_  | See [Roles](#roles). |
| `group_pool`  | _list_    | The groups the user can access. |
| `is_active`   | _boolean_ | `false` for a deactivated user, or an invited user who has not completed registration. |
| `date_joined` | _string_  | Date the user was created, `YYYY-MM-DD`. |

### HTTP Request

`GET https://api.allphins.com/api/v1/families/:family_id/users/`

## Invite a collaborator

Creates an inactive user and sends them an email with a link to complete their registration.

> Example request:

```shell
curl https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/users/ \
  -X POST \
  -H "Authorization: Bearer YourAccessToken" \
  -H "Content-Type: application/json" \
  -d '{"email": "new-user@mycompany.com", "role": "user", "groups": [1000, 1001]}'
```

> Example response (status 201):

```json
{
  "id": 3,
  "first_name": "",
  "last_name": "",
  "email": "new-user@mycompany.com",
  "role": "user",
  "group_pool": [
    {
      "group_id": 1000,
      "line_of_business": "energy_v2",
      "placement_type": "treaty"
    },
    {
      "group_id": 1001,
      "line_of_business": "aviation_war",
      "placement_type": "treaty"
    }
  ],
  "is_active": false,
  "date_joined": "2025-10-22"
}
```

### HTTP Request

`POST https://api.allphins.com/api/v1/families/:family_id/users/`

### Payload

| Argument     | Type      | Required | Description |
|--------------|-----------|----------|-------------|
| `email`      | _string_  | yes      | Email of the collaborator. Must not belong to an existing user. |
| `role`       | _string_  | yes      | See [Roles](#roles). |
| `groups`     | _list_    | yes      | Ids of the groups the user can access. They must belong to your company. |
| `first_name` | _string_  | no       | First name. |
| `last_name`  | _string_  | no       | Last name. |

An email already in use or an unknown role returns `400`. A group outside your company returns `400` `user_creation_failed`.

## Update a user

Changes the role, lines of business, name or status of a user. Send only the fields to change.

> Deactivate a user:

```shell
curl https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/users/2/ \
  -X PATCH \
  -H "Authorization: Bearer YourAccessToken" \
  -H "Content-Type: application/json" \
  -d '{"is_active": false}'
```

> Set the lines of business of a user:

```shell
curl https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/users/2/ \
  -X PATCH \
  -H "Authorization: Bearer YourAccessToken" \
  -H "Content-Type: application/json" \
  -d '{"groups": [1000, 1001]}'
```

> Example response:

```json
{
  "id": 2,
  "first_name": "Alice",
  "last_name": "Doe",
  "email": "alice.doe@mycompany.com",
  "role": "user",
  "is_active": true,
  "group_pool": [
    {
      "group_id": 1000,
      "line_of_business": "energy_v2",
      "placement_type": "treaty"
    },
    {
      "group_id": 1001,
      "line_of_business": "aviation_war",
      "placement_type": "treaty"
    }
  ]
}
```

### HTTP Request

`PATCH https://api.allphins.com/api/v1/families/:family_id/users/:user_id/`

#### Path variables

`family_id`: the id of your company.

`user_id`: the id of the user.

### Payload

| Argument     | Type      | Required | Description |
|--------------|-----------|----------|-------------|
| `is_active`  | _boolean_ | no       | `false` deactivates the user, `true` reactivates them. |
| `groups`     | _list_    | no       | Ids of the groups the user can access. Replaces the current list. Every id must belong to your company. |
| `role`       | _string_  | no       | See [Roles](#roles). |
| `first_name` | _string_  | no       | First name. |
| `last_name`  | _string_  | no       | Last name. |

The response contains `id`, `first_name`, `last_name`, `email`, `role`, `is_active` and `group_pool`. It has no `date_joined`.

A user who is not in the company returns `404` `not_found`.

# User management

It's possible to manage users from the API. Possible actions are:
* invite a collaborator to Allphins
* add/remove available line of business for any user.
* deactivate/reactivate any user

## Retrieve list of users
This endpoint returns the list of your available line of business and the list of users with their current configuration.

### HTTP Request

`GET https://api.allphins.com/api/v1/families/:family_id/`

To know your family id, please contact Allphins.

> Example request:

```shell
curl https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> Response:

```json
  {
    "name": "Cyber Client 1 2024",
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
      },
      {
        "group_id": 1002,
        "line_of_business": "casualty",
        "placement_type": "treaty"
      },
      {
        "group_id": 1003,
        "line_of_business": "terror_v2",
        "placement_type": "treaty"
      }
    ],
    "users": [
      {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe",
        "email": "",
        "role": "read_only",
        "group_pool": [
          {
            "group_id": 1000,
            "line_of_business": "energy_v2",
            "placement_type": "treaty"
          }
        ],
        "is_active": false,
        "date_joined": "2025-06-21"
      },
      {
        "id": 2,
        "first_name": "Alice",
        "last_name": "Doe",
        "email": "neosoft_2@neosoft.fr",
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
          },
          {
            "group_id": 1002,
            "line_of_business": "casualty",
            "placement_type": "treaty"
          }
        ],
        "is_active": true,
        "date_joined": "2025-09-10"
      }
    ]
  }
```

### URL Arguments

#### Path variable
`family_id`: The id of your family

## Invite a collaborator to Allphins
Used to invite a collaborator to Allphins. They will receive in their mailbox a link to complete the registration process.

> Example request:

```shell
curl 'https://api.allphins.com/api/v1/families/a6188aa6-bd57-41d1-8015-f0b6ee88cdec/users/' \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  -X POST \
  - '{"email":"new-user@mycompany.com","role":"user","groups":[1000,1002]}'
```

> Response

```json
  {
    "id": 3,
    "first_name": "",
    "last_name": "",
    "email": "new-user@mycompany.com",
    "role" :"user",
    "group_pool": [
      {
        "group_id":1000,
        "line_of_business":"energy_v2",
        "placement_type":"treaty"
      },
      {
        "group_id":1002,
        "line_of_business":"casualty",
        "placement_type":"treaty"
      }
    ],
    "is_active": false,
    "date_joined": "2025-10-22"
  }
```
### HTTP Request

`POST https://api.allphins.com/api/v1/families/:family_id/users/`

### URL Arguments

#### Path variable
`family_id`: The id of your family

### Payload

| Argument | Type    | Required  | Description                                           |
|----------|---------|-----------|-------------------------------------------------------|
| `email`  | _str_   | yes       | The email of the collaborator.                        |
| `role`   | _str_   | yes       | The role of the user, `read_only`, `user` or `admin`. |
| `groups` | _list_  | yes       | The list of groups id the user has access to          |


## Deactivate or reactivate a user
Used to deactivate or reactivate a user.

> Example request:

```shell
curl 'https://stg.allphins.com/api/v1/families/378c72c7-0259-48aa-99fd-2112db28d384/users/2/' \
  -X PATCH \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  - '{"is_active":false}'
```

> Response:

```json
{
  "id": 2,
  "first_name": "Alice",
  "last_name": "Doe",
  "email": "neosoft_2@neosoft.fr",
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
    },
    {
      "group_id": 1002,
      "line_of_business": "casualty",
      "placement_type": "treaty"
    }
  ],
  "is_active": false,
  "date_joined": "2025-09-10"
}
```
### HTTP Request

`PATCH https://api.allphins.com/api/v1/families/:family_id/users/:user_id/`

### URL Arguments

#### Path variable
`family_id`: The id of your family

`user_id`: The id of the user

### Payload

| Argument           | Type      | Required | Description                      |
|--------------------|-----------|----------|----------------------------------|
| `is_active`        | _boolean_ | yes      | Deactivate or reactivate an user |


## Add or remove a line of business to a user
Used to add or remove lines of business to a user.

> Example request

```shell
curl 'https://stg.allphins.com/api/v1/families/378c72c7-0259-48aa-99fd-2112db28d384/users/2/' \
  -X PATCH \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  - '{"groups": [1000, 1001]}'
```

> Response:

```json
{
  "id": 2,
  "first_name": "Alice",
  "last_name": "Doe",
  "email": "neosoft_2@neosoft.fr",
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
  "is_active": true,
  "date_joined": "2025-09-10"
}
```

### HTTP Request

`PATCH https://api.allphins.com/api/v1/families/:family_id/users/:user_id/`

### URL Arguments

#### Path variable
`family_id`: The id of your family

`user_id`: The id of the user

### Payload

| Argument | Type   | Required | Description                                  |
|----------|--------|----------|----------------------------------------------|
| `groups` | _list_ | yes      | The list of groups id the user has access to |

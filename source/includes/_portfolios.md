# Portfolios

The portfolio object links policies to a list of risks. It belongs to a client and has a specific line of business.

## Portfolio Object Attributes

| Attribute       | Type                                                                    | Description                                                                                                                                                                                                                                                           |
|-----------------|-------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`            | _UUID_                                                                  | Unique identifier for the object.                                                                                                                                                                                                                                     |
| `name`          | _string_                                                                | Name of the portfolio.                                                                                                                                                                                                                                                |
| `created_at`    | _timestamp_                                                             | Creation date.                                                                                                                                                                                                                                                        |
| `updated_at`    | _timestamp_                                                             | Last update date.                                                                                                                                                                                                                                                     |
| `transaction`   | _string_                                                                | Type of transaction: `pre_inward`, `inward`, `selfward`, `outward`.                                                                                                                                                                                                   |
| `renewal_date`  | _string_                                                                | Next renewal date.                                                                                                                                                                                                                                                    |
| `premium`       | _int_                                                                   | Premium in USD.                                                                                                                                                                                                                                                       |
| `max_exposure`  | _float_                                                                 | Maximum exposure on the portfolio (based on enterd policies).                                                                                                                                                                                                         |
| `client`        | _int_                                                                   | ID of the client.                                                                                                                                                                                                                                                     |
| `client_name`   | _string_                                                                | Name of the client                                                                                                                                                                                                                                                    |
| `timeline`      | _string_                                                                | Status of the portofolio (expired, active, etc...)                                                                                                                                                                                                                    |
| `renewal`       | _string_                                                                | ID of the next portfolio.                                                                                                                                                                                                                                             |
| `portfolio_class` | _string_                                                                | Line of business of the portfolio.                                                                                                                                                                                                                                    |
| `year_of_account` | _int_                                                                   | Year of account.                                                                                                                                                                                                                                                      |
| `datasources`   | _list[dict]_                                                            | List of all uploaded datasources.                                                                                                                                                                                                                                     |
|     | _id_ <br/>_status_<br/>_original_filename_                              | UUID of the datasource<br/>Status of the datasource<br/>Original filename of the datasource                                                                                                                                                                           |
| `policies`      | _list[dict]_                                                            | List of the policies.                                                                                                                                                                                                              |
|        | _id_<br/>_name_<br/>_type_<br/>_start_date_<br/>_end_date_<br/>_status_ | Id of the policy.<br/>Name of the policy.<br/>Type of the policy (`quota_share`, `excess_of_loss`, `direct`)<br/>Start date of the policy<br/>End date of the policy<br/>Status of the policy (`quote`, `written`, `expired`, `declined`, `not_taken_up`, `work_in_progress`, `deleted`) |


## Create a portfolio

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/`

### Payload

| Parameter         | Type | Description                             |
| ----------------- |-------|-----------------------------------------|
| `name`            | _str_ | Name of the portfolio                   |
| `portfolio_class` | _str_ | Line of business of the portfolio       |
| `year_of_account` | _int_ | Year of account                         |
| `client`          | _int_ | Client ID                               |
| `transaction`     | _str_ | Type of transaction (`inward` or `outward`) |

```shell
curl https://api.allphins.com/api/v1/portfolios/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"name":"Client A 2024","portfolio_class":"cyber","transaction":"inward","year_of_account":2024,"client":1234}'
  ````

## Retrieve all portfolios

This endpoint retrieves all portfolios of a given user. This will also return portfolios shared by other users with the current user.

```shell
curl https://api.allphins.com/api/v1/portfolios/?label=in_force \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "Cyber Client 1 2024",
    "created_at": "2024-05-20T07:52:55.491729Z",
    "updated_at": "2024-05-23T14:31:10.591036Z",
    "renewal_date": "2024-12-31T00:00:00Z",
    "premium": 9500,
    "max_exposure": 190000,
    "policies": [
      {
        "id": 1234,
        "name": "Policy 1",
        "type": "excess_of_loss",
        "start_date": "2023-01-01",
        "end_date": "2025-05-19",
        "status": "quote"
      },
      {
        "id": 1235,
        "name": "Policy 2",
        "type": "excess_of_loss",
        "start_date": "2024-01-01",
        "end_date": "2024-12-31",
        "status": "written"
      },
      {
        "id": 1236,
        "name": "Policy 3",
        "type": "",
        "start_date": null,
        "end_date": null,
        "status": "quote"
      }
    ],
    "client": 1,
    "client_name": "Cyber Client 1",
    "timeline": "Active",
    "renewal": null,
    "portfolio_class": "cyber",
    "year_of_account": 2024,
    "transaction": "inward",
    "datasources": [
      {
        "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "status": "Ready",
        "original_filename": "Risk_list.xlsx"
      }
    ],
    "main_datasource_status": "Ready"
  }
]
```

### HTTP Request

`GET https://api.allphins.com/api/v1/portfolios/`

### Querry Parameters

| Parameter | Description                                                                                                                      |
| --------- |----------------------------------------------------------------------------------------------------------------------------------|
| `label`   | Shortcut to retreive all actives portfolios, set label to 'in_force' (https://api.allphins.com/api/v1/portfolios?label=in_force) |

## Retrieve a portfolio

This endpoint retrieves a specific portfolio. More fields are available through this endpoint.

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

> The above command returns JSON structured like this:

```json
{
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "Cyber Client 1 2024",
  "created_at": "2024-05-20T07:52:55.491729Z",
  "updated_at": "2024-05-23T14:31:10.591036Z",
  "renewal_date": "2024-12-31T00:00:00Z",
  "premium": 9500,
  "max_exposure": 190000,
  "policies": [
    {
      "id": 1234,
      "name": "Policy 1",
      "type": "excess_of_loss",
      "start_date": "2023-01-01",
      "end_date": "2025-05-19",
      "status": "quote"
    },
    {
      "id": 1235,
      "name": "Policy 2",
      "type": "excess_of_loss",
      "start_date": "2024-01-01",
      "end_date": "2024-12-31",
      "status": "written"
    },
    {
      "id": 1236,
      "name": "Policy 3",
      "type": "",
      "start_date": null,
      "end_date": null,
      "status": "quote"
    }
  ],
  "client": 1,
  "client_name": "Cyber Client 1",
  "timeline": "Active",
  "renewal": null,
  "portfolio_class": "cyber",
  "year_of_account": 2024,
  "transaction": "inward",
  "datasources": [
    {
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "status": "Ready",
      "original_filename": "Risk_list.xlsx"
    }
  ],
  "main_datasource_status": "Ready"
}
```

### HTTP Request

`GET https://api.allphins.com/api/v1/portfolios/:id/`

### Query Parameters

| Parameter | Description                         |
| --------- | ----------------------------------- |
| `id`      | The ID of the portfolio to retrieve |

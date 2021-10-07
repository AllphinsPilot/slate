# Portfolios

The portfolio object links policies to a list of risks. It belongs to a client and has a specific line of business.

## Portfolio Object Attributes

| Attribute          | Type        | Description                                                                                                                        |
| ------------------ | ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `id`               | _UUID_      | Unique identifier for the object.                                                                                                  |
| `name`             | _string_    | Name of the portfolio.                                                                                                             |
| `created_at`       | _timestamp_ | Creation date.                                                                                                                     |
| `updated_at`       | _timestamp_ | Last update date.                                                                                                                  |
| `status`           | _integer_   | Processing status of the portfolio (-1: `error`, 1: `unprocessed`, 2: `loading`, 3: `processing`, 3: `enriching`, 4: `processed`). |
| `renewal_date`     | _string_    | Next renewal date.                                                                                                                 |
| `premium`          | _int_       | Premium in USD.                                                                                                                    |
| `risks_count`      | _int_       | Number of risks in the portfolio.                                                                                                  |
| `max_exposure`     | _float_     | Maximum exposure on the portfolio (based on entered structures).                                                                   |
| `structures`       | _list_      | List of policies                                                                                                                   |
| `client`           | _int_       | ID of the client.                                                                                                                  |
| `client_name`      | _string_    | Name of the client                                                                                                                 |
| `timeline`         | _string_    | Status of the portofolio (expired, active, etc...)                                                                                 |
| `renewal`          | _string_    | ID of the next portfolio.                                                                                                          |
| `portfolio_class`  | _string_    | Line of business of the portfolio.                                                                                                 |
| `year_of_account`  | _int_       | Year of account.                                                                                                                   |
| `match_percentage` | _float_     | Percentage of enriched risks.                                                                                                      |
| `sov_files`        | _list_      | List of all the Schedules of Values entered, with the corresponding configuration.                                                 |

## Retrieve all portfolios

This endpoint retrieves all portfolios of a given user. This will also return portfolios shared by other users with the current user.

```shell
curl https://app.allphins.com/api/v1/portfolios/
  -H "Authorization: Token ENTERYOURAUTHTOKEN"
```

> The above command returns JSON structured like this:

```json
[
    {
        "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
        "name": "Cedant A 2020",
        "status": 1,
        "created_at": "2020-10-16T17:20:42.697620",
        "updated_at": "2021-09-27T15:42:11.425037",
        "renewal_date": "2019-12-30T00:00:00",
        "premium": 1000000,
        "risks_count": 1000,
        "max_exposure": 10000000,
        "structures": [
            {
                "id": 1,
                "type": "quota_share",
                "start_date": "2018-12-31T00:00:00",
                "end_date": "2019-12-30T00:00:00",
                "status": "written"
            }
        ],
        "client": 1,
        "client_name": "Client A",
        "timeline": "Expired",
        "renewal": null,
        "portfolio_class": "energy",
        "year_of_account": null
    }
]
```

### HTTP Request

`GET https://app.allphins.com/api/v1/portfolios/`

### URL Parameters

| Parameter | Description                                                                                                                     |
| --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `label`   | Shortcut to retreive all actives portfolio, set label to 'in_force' (https://app.allphins.com/api/v1/portfolios?label=in_force) |

## Retrieve a portfolio

This endpoint retrieves a specific portfolio. More fields are available throudh this endpoint.

```shell
curl https://app.allphins.com/api/v1/portfolios/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee/
  -H "Authorization: Token ENTERYOURAUTHTOKEN"
```

> The above command returns JSON structured like this:

```json
{
    "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
    "name": "Cedant A 2020",
    "status": 1,
    "premium": 10000,
    "match_percentage": 0.99,
    "max_exposure": 10000000,
    "risks_count": 1000,
    "created_at": "2020-10-16T17:20:42.697620",
    "updated_at": "2021-09-27T15:42:11.425037",
    "structures": [
        {
            "description": null,
            "end_date": "2019-12-30T00:00:00",
            "excess_currency": "USD",
            "excess": null,
            "id": 123,
            "limit_currency": "USD",
            "limit": null,
            "portfolio": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
            "premium_100": 100000,
            "reference": null,
            "reinstatement": null,
            "risk_attached": false,
            "share": 0.01358,
            "start_date": "2018-12-31T00:00:00",
            "status": "written",
            "tags": ["Station F"],
            "type": "quota_share",
            "benefits": [],
            "client_id": 1,
            "client_name": "Client A",
            "portfolio_name": "Cedant A 2020"
        }
    ],
    "legacy": null,
    "renewal": null,
    "client": 1,
    "client_name": "Client A",
    "timeline": "Expired",
    "portfolio_type": "direct",
    "sov_files": [
        {
            "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
            "file": "https://secret-url-to-file",
            "config": {},
            "status": "Processed",
            "created_at": "2021-03-05T20:27:07.065600",
            "updated_at": "2021-03-05T20:27:07.065608"
        }
    ],
    "portfolio_class": "energy",
    "year_of_account": null
}
```

### HTTP Request

`GET https://app.allphins.com/api/v1/portfolios/:id/`

### URL Parameters

| Parameter | Description                         |
| --------- | ----------------------------------- |
| `id`      | The ID of the portfolio to retrieve |

## Create a portfolio

### HTTP Request

`POST https://app.allphins.com/api/v1/portfolios/`

### URL Parameters

| Parameter         | Description                       |
| ----------------- | --------------------------------- |
| `name`            | Name of the portfolio             |
| `portfolio_class` | Line of business of the portfolio |
| `year_of_account` | Year of account                   |
| `client`          | Client ID or name                 |

## Edit a portfolio

### HTTP Request

`PATCH https://app.allphins.com/api/v1/portfolios/`

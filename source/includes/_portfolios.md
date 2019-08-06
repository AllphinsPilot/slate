# Portfolios

## Retrieve all portfolios

```shell
curl https://app.allphins.com/api/v1/portfolios/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": "327aac44-8de0-4f49-9fbe-e0ece1dd2d3c",
            "name": "Portfolio ABC",
            "created_at": "2019-07-22T17:25:31.109378",
            "updated_at": "2019-07-22T17:44:51.232439",
            "risks_count": 23627,
            "asset_exposures_count": 3094,
            "completion": 100,
            "file": null,
            "exposure_rules": {
                "layers": [
                    {
                        "shares": 0.2,
                        "limit": 22500000,
                        "excess": 7500000,
                        "name": "Layer 1"
                    },
                    {
                        "shares": 0.2,
                        "limit": 30000000,
                        "excess": 30000000,
                        "name": "Layer 2"
                    }
                ]
            },
            "visible": true,
            "status": 2,
            "users": [
                "Jimy",
                "Jen",
                "Max"
            ],
            "aggregate_gross_exposure": 43888763208.36,
            "average_gross_exposure": 1857568.17236044,
            "max_gross_exposure": 105000000,
            "aggregate_company_exposure": 5062180976.69399,
            "average_company_exposure": 1636128.30533096,
            "max_company_exposure": 10500000
        }
    ]
}
```

This endpoint retrieves all portfolios of a given user. This will also return portfolios shared by other users with the current user.

### The portfolio object

#### Attributes

Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`name` | *string* | Name of the portfolio.
`created_at` | *timestamp* | Creation date.
`updated_at` | *timestamp* | Last update date.
`risks_count` | *integer* | Number of distinct risks in the portfolio.
`asset_exposures_count` | *integer* | Number of distinct risks in the portfolio.
`file` | *file* | File if a file is used to upload the risks.
`completion` | *integer* | Percentage of matched assets from the file (if a file is provided)
`exposure_rules` | *object* | Object describing the exposure rules to compute exposure.
`visible` | *boolean* | Default is `true`, if set to `false` the portfolio is not visible in the Allphins application.
`status` | *integer* | Status of the assets matching (1: `unmatched`, 2: `matched`, 3: `error`).
`users` | *list of strings* | List of users allowed to see the portfolio.
`aggregate_gross_exposure` | *float* | Aggregated gross exposure of the portfolio.
`average_gross_exposure` | *float* | Average gross exposure of the portfolio.
`max_gross_exposure` | *float* | Maximum gross exposure of the portfolio.
`aggregate_company_exposure` | *float* | Aggregate exposure for the company (computed with the `exposure_rules`).
`average_company_exposure` | *float* | Average exposure for the company (computed with the `exposure_rules`).
`max_company_exposure` | *float* | Maximum exposure for the company (computed with the `exposure_rules`).

## Retrieve a portfolio

```shell
curl https://app.allphins.com/api/v1/portfolios/064f9fad-24bd-4091-a782-9bba124cdff2/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "id": "327aac44-8de0-4f49-9fbe-e0ece1dd2d3c",
    "name": "Portfolio ABC",
    "created_at": "2019-07-22T17:25:31.109378",
    "updated_at": "2019-07-22T17:44:51.232439",
    "risks_count": 23627,
    "asset_exposures_count": 3094,
    "completion": 100,
    "file": null,
    "exposure_rules": {
        "layers": [
            {
                "shares": 0.2,
                "limit": 22500000,
                "excess": 7500000,
                "name": "Layer 1"
            },
            {
                "shares": 0.2,
                "limit": 30000000,
                "excess": 30000000,
                "name": "Layer 2"
            }
        ]
    },
    "visible": true,
    "status": 2,
    "users": [
        "Jimy",
        "Jen",
        "Max"
    ],
    "aggregate_gross_exposure": 43888763208.36,
    "average_gross_exposure": 1857568.17236044,
    "max_gross_exposure": 105000000,
    "aggregate_company_exposure": 5062180976.69399,
    "average_company_exposure": 1636128.30533096,
    "max_company_exposure": 10500000
}
```

This endpoint retrieves a specific portfolio.

### HTTP Request

`GET https://app.allphins.com/api/v1/portfolios/:id/`

### URL Parameters

Parameter | Description
--------- | -----------
`id` | The ID of the portfolio to retrieve


## Retrieve portfolio risks

```shell
curl https://app.allphins.com/api/v1/portfolios/064f9fad-24bd-4091-a782-9bba124cdff2/asset_exposures
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "count": 3094,
    "next": "http://35.197.208.182/api/v1/portfolios/327aac44-8de0-4f49-9fbe-e0ece1dd2d3c/asset_exposures/?page=2",
    "previous": null,
    "results": [
        {
            "id": 7065,
            "name": "A12 CPP",
            "assets": [
                {
                    "id": "c4e6a6a9-ec68-4fd3-820b-a0d23dd4b4af",
                    "rank": 1,
                    "name": "A12-CPP",
                    "companies": [
                        {
                            "id": 198818,
                            "name": "Petrogas E&P Netherlands BV",
                            "operating": true,
                            "shares": null,
                            "asset": "c4e6a6a9-ec68-4fd3-820b-a0d23dd4b4af",
                            "company": "14f805cd-f86b-4751-8adb-ddaf0a473701"
                        }
                    ],
                    "status": "In place",
                    "asset_category": "offshore_installation",
                    "asset_type": "Offshore Fixed",
                    "asset_subtype": [
                        "Piled"
                    ],
                    "country": "Netherlands",
                    "country_code": "NL"
                }
            ],
            "stats": {
                "capex_sum": 104.833855,
                "raised_flags": 1
            },
            "policies": [
                {
                    "id": "40a39abd-bdf5-41aa-9964-64d3d0dad095",
                    "name": "A12 CPP",
                    "slip_reference": "SLIPREFERENCE",
                    "policy": "TPL",
                    "gross_exposure": 965382,
                    "policy_normalized": "tpl",
                    "policy_type": "liability_type"
                },
                {
                    "id": "eccba67d-0adc-4a70-8e09-7df8f93f4625",
                    "name": "A12 CPP",
                    "slip_reference": "SLIPREFERENCE",
                    "policy": "S&L",
                    "gross_exposure": 46821.03,
                    "policy_normalized": "s&l",
                    "policy_type": "pd_type"
                }
            ],
            "risks_count": 22,
            "company_exposure": 1495811.434,
            "exposure_rules": {
                "layers": [
                    {
                        "shares": 0.2,
                        "limit": 22500000,
                        "excess": 7500000,
                        "name": "Layer 1"
                    },
                    {
                        "shares": 0.2,
                        "limit": 30000000,
                        "excess": 30000000,
                        "name": "Layer 2"
                    }
                ]
            },
            "portfolio": "327aac44-8de0-4f49-9fbe-e0ece1dd2d3c"
        },
        {},
        {}
    ]
}
```

This endpoint retrieves a list of [`risks`](#exposures) (ex `asset_exposure`) belonging to this portfolio.

### HTTP Request

`GET https://app.allphins.com/api/v1/portfolios/:id/asset_exposure`

<aside class="notice">
  <code>asset_exposure</code> will be changed to <code>risks</code> in the next release.
</aside>

### URL Arguments

Argument | Default | Description
--------- | ------- | -----------
`id` | null | The ID of the portfolio to retrieve.
`max_company_exposure` | null | Max threshold to filter query on company exposure.
`min_company_exposure` | null | Min threshold to filter query on company exposure.


# Portfolios

The portfolio object links policies to a list of risks. It belongs to a client and has a specific line of business.

## Portfolio Object Attributes

| Attribute         | Type                                                                    | Description                                                                                                                                                                                                                                                                              |
|-------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`              | _UUID_                                                                  | Unique identifier for the object.                                                                                                                                                                                                                                                        |
| `name`            | _string_                                                                | Name of the portfolio.                                                                                                                                                                                                                                                                   |
| `created_at`      | _timestamp_                                                             | Creation date.                                                                                                                                                                                                                                                                           |
| `updated_at`      | _timestamp_                                                             | Last update date.                                                                                                                                                                                                                                                                        |
| `transaction`     | _string_                                                                | Type of transaction: `pre_inward`, `inward`, `selfward`, `outward`.                                                                                                                                                                                                                      |
| `renewal_date`    | _string_                                                                | Next renewal date.                                                                                                                                                                                                                                                                       |
| `cedant`          | _int_                                                                   | Id of the cedant policy.                                                                                                                                                                                                                                                                 |
| `premium`         | _int_                                                                   | Premium in USD.                                                                                                                                                                                                                                                                          |
| `max_exposure`    | _float_                                                                 | Maximum exposure on the portfolio (based on enterd policies).                                                                                                                                                                                                                            |
| `client`          | _int_                                                                   | ID of the client.                                                                                                                                                                                                                                                                        |
| `client_name`     | _string_                                                                | Name of the client                                                                                                                                                                                                                                                                       |
| `timeline`        | _string_                                                                | Status of the portofolio (expired, active, etc...)                                                                                                                                                                                                                                       |
| `renewal`         | _string_                                                                | ID of the next portfolio.                                                                                                                                                                                                                                                                |
| `portfolio_class` | _string_                                                                | Line of business of the portfolio.                                                                                                                                                                                                                                                       |
| `year_of_account` | _int_                                                                   | Year of account.                                                                                                                                                                                                                                                                         |
| `datasources`     | _list[dict]_                                                            | List of all uploaded datasources.                                                                                                                                                                                                                                                        |
|                   | _id_ <br/>_status_<br/>_original_filename_                              | UUID of the datasource<br/>Status of the datasource<br/>Original filename of the datasource                                                                                                                                                                                              |
| `policies`        | _list[dict]_                                                            | List of the policies.                                                                                                                                                                                                                                                                    |
|                   | _id_<br/>_name_<br/>_type_<br/>_start_date_<br/>_end_date_<br/>_status_ | Id of the policy.<br/>Name of the policy.<br/>Type of the policy (`quota_share`, `excess_of_loss`, `direct`)<br/>Start date of the policy<br/>End date of the policy<br/>Status of the policy (`quote`, `written`, `expired`, `declined`, `not_taken_up`, `work_in_progress`, `deleted`) |


## Create a portfolio

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/`

### Payload

| Parameter         | Type  | Description                                 |
|-------------------|-------|---------------------------------------------|
| `name`            | _str_ | Name of the portfolio                       |
| `portfolio_class` | _str_ | Line of business of the portfolio           |
| `year_of_account` | _int_ | Year of account                             |
| `client`          | _int_ | Client ID                                   |
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

## Get portfolio analytics

This endpoint returns analytics for a portfolio. Results can be displayed as a table (split by a single dimension) or as a matrix (multi-dimensional). An optional comparison portfolio can be passed to diff metrics between two portfolios.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/:id/analytics/`

### Payload

| Parameter                 | Type         | Required                            | Description                                                                                |
|---------------------------|--------------|-------------------------------------|--------------------------------------------------------------------------------------------|
| `view`                    | _str_        | yes                                 | View format: `table` or `matrix`.                                                          |
| `in_force_date`           | _str_        | yes                                 | In-force date in ISO 8601 format (e.g. `2026-04-30`).                                      |
| `split`                   | _str_        | no (default `attachment_point`)     | Dimension to split the analytics by. See list of accepted values below.                    |
| `filters`                 | _list[dict]_ | yes                                 | List of filters to apply on the portfolio risks. Pass `[]` if no filter is needed. See "Filter attributes" below. |
| `comparison_portfolio_id` | _UUID_       | no                                  | ID of another portfolio to compare against.                                                |

### Accepted `split` values

The accepted values for `split` depend on the line of business of the portfolio.

| Line of business            | Accepted splits                                                                                                                    |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| `Terror direct fac`         | `country`, `state`, `county`, `city`, `zip_code`, `peril`, `occupancy`, `year_built`, `address_precision`, `construction_material` |
| `Political Risk`            | `tenor`, `obligor_v2_country`, `country`, `risk_code`, `lloyds_pr_industry`                                                        |
| `Political Risk direct fac` | `tenor`, `obligor_v2_country`, `country`, `risk_code`, `lloyds_pr_industry`, `trade_direct_status`                                 |
| `Casualty`                  | `attachment_point`, `country`, `naics_sector`, `naics_subsector`, `insured_revenue`                                                |
| `Cyber`                     | `attachment_point`, `country`, `naics_sector`, `naics_subsector`, `insured_revenue`, `cyber_cover_type`, `tech_stack`              |

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/analytics/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"view":"table","in_force_date":"2024-12-31","split":"country","filters":[]}'
```

> The above command returns JSON structured like this (table view):

```json
{
  "data": [
    {"split": "United States", "column1": 1234567, "column2": 89},
    {"split": "France",        "column1":  234567, "column2": 12}
  ]
}
```

> Or, when `view` is `matrix`:

```json
{
  "dimension1": [
    {"split": "value1", "column1": "value2"}
  ],
  "dimension2": [
    {"split": "value3", "column1": "value4"}
  ]
}
```

## Get portfolio EDM analytics

This endpoint returns EDM analytics for a portfolio. Results are returned in three blocks: `current` (the requested portfolio), `comparison` (a second portfolio if `comparison_portfolio_id` is provided, otherwise `null`) and `variation` (the diff between the two).

This endpoint is only available for Terror, Property and Energy Onshore lines of business.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/:id/edm_analytics/`

### Payload

| Parameter                 | Type         | Required | Description                                                                                                          |
|---------------------------|--------------|----------|----------------------------------------------------------------------------------------------------------------------|
| `view`                    | _str_        | yes      | View format: `table` or `matrix`.                                                                                    |
| `split`                   | _str_        | yes      | Dimension to split by. Accepted values depend on `view` (see below).                                                 |
| `filters`                 | _list[dict]_ | yes      | List of filters to apply on the portfolio risks. Pass `[]` if no filter is needed. See "Filter attributes" below.    |
| `comparison_portfolio_id` | _UUID_       | no       | ID of another portfolio to compare against.                                                                          |

### Accepted `split` values

When `view` is `table`, the accepted values for `split` depend on the line of business:

| Line of business    | Accepted splits                                                                                                                                                                                                                             |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Energy Onshore`    | `attachment_point`, `country`, `state`, `county`, `city`, `occupancy`, `geo_resolution`, `distance_to_shore`, `loss_type`, `peril`, `region`, `year_built`, `construction_material_building`, `construction_material_others`, `exposure_set`, `asset_match` |
| `Property`          | `attachment_point`, `country`, `state`, `county`, `city`, `occupancy`, `geo_resolution`, `distance_to_shore`, `loss_type`, `peril`, `region`, `year_built`, `construction_material_building`, `construction_material_others`, `exposure_set`                |
| `Terror`            | `attachment_point`, `country`, `state`, `county`, `city`, `occupancy`, `geo_resolution`, `distance_to_shore`, `loss_type`, `peril`, `region`, `year_built`, `construction_material_building`, `construction_material_others`, `exposure_set`                |
| `Terror direct fac` | `country`, `state`, `county`, `city`, `zip_code`, `peril`, `occupancy`, `year_built`, `address_precision`, `construction_material`                                                                                                          |

When `view` is `matrix`:

`attachment_point`, `geo_resolution`, `location_enrichment_details`.

### Filter attributes

Each entry in the `filters` list is an object with the following shape:

| Field             | Type            | Description                                                                                       |
|-------------------|-----------------|---------------------------------------------------------------------------------------------------|
| `operator`        | _str_           | Comparison operator. Currently `is` (membership in `value`).                                      |
| `attribute`       | _str_           | Attribute name to filter on. See list of supported attributes below.                              |
| `value`           | _list[int]_     | List of accepted attribute IDs.                                                                   |
| `label`           | _list[str]_     | Human-readable labels matching `value` (display only).                                            |
| `attribute_value` | _list[str]_     | Attribute codes matching `value` (display only).                                                  |

Example:

```json
[
  {
    "filters": [
      {
        "operator": "is",
        "attribute": "property_peril",
        "value": [67113764],
        "label": ["Fire (5)"],
        "attribute_value": ["FR"]
      }
    ]
  }
]
```

#### Supported attributes

##### `attribute = "property_peril"`

| ID         | Name                    | Code  |
|------------|-------------------------|-------|
| 67113760   | Earthquake (1)          | EQ    |
| 67113761   | Windstorm (2)           | WS    |
| 67113762   | Winterstorm (3)         | WT    |
| 67113763   | Flood (4)               | FL    |
| 67113764   | Fire (5)                | FR    |
| 67113765   | Terrorism (6)           | TR    |
| 74511196   | Severe Convective Storm | SCS   |
| 117332703  | Smoke                   | SM    |

##### `attribute = "terror_risk_code"`

| ID         | Name         | Code         |
|------------|--------------|--------------|
| 16373262   | CYBER        | cyber        |
| 3164513    | NCBR         | ncbr         |
| 1762605    | SRCC         | srcc         |
| 1762607    | WAR          | war          |
| 1762606    | TERROR       | tr           |
| 75944889   | AVIATION WAR | AVIATION WAR |

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/edm_analytics/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"view":"table","split":"country","filters":[]}'
```

> The above command returns JSON structured like this:

```json
{
  "current": {
    "data": [
      {"split": "United States", "tiv": 1234567},
      {"split": "France",        "tiv":  234567}
    ]
  },
  "comparison": null,
  "variation": null
}
```

> When `view` is `matrix`, each block holds a multi-dimensional object:

```json
{
  "current": {
    "dimension1": [
      {"split": "value1", "column1": "value2"}
    ],
    "dimension2": [
      {"split": "value3", "column1": "value4"}
    ]
  },
  "comparison": null,
  "variation": null
}
```

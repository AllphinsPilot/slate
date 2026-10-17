# Portfolios

A portfolio holds one client's policies and risks for one line of business. Every portfolio belongs to a group, and the API only returns the portfolios of your active group.

## Portfolio Object Attributes

| Attribute               | Type          | Description                                                                                                                         |
|-------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `id`                    | _UUID_        | Unique identifier for the object.                                                                                                   |
| `name`                  | _string_      | Name of the portfolio.                                                                                                              |
| `created_at`            | _timestamp_   | Creation date.                                                                                                                      |
| `updated_at`            | _timestamp_   | Last update of the portfolio record.                                                                                                |
| `transaction`           | _string_      | `inward` or `outward`.                                                                                                              |
| `renewal_date`          | _date_        | Renewal date, `YYYY-MM-DD`.                                                                                                         |
| `next_renewal`          | _date_        | Earliest end date among the portfolio's written policies that have not ended yet.                                                   |
| `year_of_account`       | _int_         | Year of account.                                                                                                                    |
| `client`                | _int_         | ID of the client.                                                                                                                   |
| `client_name`           | _string_      | Name of the client.                                                                                                                 |
| `cedant`                | _int_         | ID of the portfolio's cedant policy, the internal policy that carries the gross exposure of an inward portfolio.                    |
| `premium`               | _int_         | Sum of premium at 100% × share over the portfolio's policies, in the display currency (see below).                                  |
| `max_exposure`          | _float_       | Sum of limit × share over the portfolio's policies, in the display currency (see below).                                            |
| `maximum_line`          | _int_         | Maximum line, in `maximum_line_currency`.                                                                                           |
| `maximum_line_currency` | _string_      | Currency of the maximum line.                                                                                                       |
| `usd_maximum_line`      | _float_       | Maximum line converted to USD.                                                                                                      |
| `total_income`          | _int_         | Total income, in `total_income_currency`.                                                                                           |
| `total_income_currency` | _string_      | Currency of the total income.                                                                                                       |
| `usd_total_income`      | _float_       | Total income converted to USD.                                                                                                      |
| `display_currency`      | _string_      | Currency the portfolio's amounts are shown in.                                                                                      |
| `fx_vector`             | _object_      | FX rates set applied to the portfolio: `id`, `name`, `origin`, `reference_currency`, `is_active`.                                   |
| `timeline`              | _string_      | `Active`, `Partly Active`, `Active - Not renewed`, `Expired`, `Starting Soon`, `Quote`, `Declined`, `No Policy`, or empty.          |
| `renewal`               | _UUID_        | ID of the portfolio that renews this one, if any.                                                                                   |
| `reference`             | _string_      | Free reference.                                                                                                                     |
| `labels`                | _list[dict]_  | Labels of the portfolio: `id`, `name`, `color_background`, `color_font`.                                                            |
| `tags`                  | _list[string]_| Names of the portfolio's labels.                                                                                                    |
| `has_rules`             | _bool_        | Whether one of the policies has sublimit rules.                                                                                     |
| `policies`              | _list[dict]_  | The portfolio's policies, cedant policy excluded. The list returns `id`, `name`, `type`, `start_date`, `end_date`, `status`, `is_interlocking`, `coverage_basis`; retrieving a portfolio returns full [policy](#policies) objects. |

**Only in the list** (`GET /portfolios/`):

| Attribute                | Type          | Description                                                                                                       |
|--------------------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| `datasources`            | _list[dict]_  | Datasources of the portfolio: `id`, `status`, `original_filename`. See [Datasources](#datasources) for the statuses. |
| `main_datasource_status` | _string_      | The least advanced status among the portfolio's datasources (`Error` first, `Ready` last), or `null` without datasources. |
| `data_update_time`       | _timestamp_   | Most recent `update_time` among the portfolio's datasources.                                                      |
| `mapped_perils`          | _list_        | Perils mapped in the portfolio's datasources.                                                                     |
| `policies_labels`        | _list[dict]_  | Labels of the portfolio and of its policies.                                                                      |
| `section`                | _object_      | Grouping section of the row, `null` unless grouping was asked for.                                                |

**Only when retrieving one** (`GET /portfolios/:id/`):

| Attribute           | Type          | Description                                                                                         |
|---------------------|---------------|-----------------------------------------------------------------------------------------------------|
| `portfolio_class`   | _string_      | Line of business of the portfolio.                                                                  |
| `legacy`            | _UUID_        | ID of the portfolio this one renews, if any.                                                        |
| `risks_ready`       | _bool_        | `true` when every datasource of the portfolio is `Ready` or `Error`.                                |
| `datasources_light` | _list[dict]_  | Datasources of the portfolio: `id`, `filename`, `extension`.                                        |
| `max_policy_date`   | _date_        | Default end date used for risk date simulation.                                                     |
| `tabs`              | _list_        | Tabs the app shows for this portfolio.                                                              |

### Portfolio display currency

`premium` and `max_exposure` are stored in USD and returned in a display currency: the one passed as the `display_currency` query parameter, otherwise the portfolio's own `display_currency`. When the portfolio's FX rates hold no rate for that currency, the amounts stay in USD. Responses may also include a `display_currency` field naming the currency the amounts are in.

## Create a portfolio

```shell
curl https://api.allphins.com/api/v1/portfolios/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"name":"Client A 2024","transaction":"inward","year_of_account":2024,"client":1234}'
```

> The above command returns the created portfolio with a `201`:

```json
{
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "Client A 2024",
  "client": 1234,
  "year_of_account": 2024,
  "maximum_line": null,
  "maximum_line_currency": null,
  "total_income": null,
  "total_income_currency": null,
  "transaction": "inward",
  "reference": null,
  "labels": [],
  "display_currency": "USD"
}
```

The portfolio takes the line of business of your active group. An inward portfolio is created with its cedant policy.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/`

### Payload

| Parameter               | Type        | Required | Description                                                                 |
|-------------------------|-------------|----------|-----------------------------------------------------------------------------|
| `name`                  | _string_    | yes      | Name of the portfolio. Must be unique per transaction within the group.     |
| `transaction`           | _string_    | no       | `inward` (default) or `outward`.                                            |
| `year_of_account`       | _int_       | no       | Year of account.                                                            |
| `client`                | _int_       | no       | Client ID. The client must belong to your active group.                     |
| `reference`             | _string_    | no       | Free reference.                                                             |
| `maximum_line`          | _int_       | no       | Maximum line.                                                               |
| `maximum_line_currency` | _string_    | no       | ISO currency code of the maximum line.                                      |
| `total_income`          | _int_       | no       | Total income.                                                               |
| `total_income_currency` | _string_    | no       | ISO currency code of the total income.                                      |
| `labels`                | _list[int]_ | no       | IDs of the labels to attach.                                                |
| `display_currency`      | _string_    | no       | ISO currency code the portfolio's amounts are shown in. Defaults to `USD`.  |
| `fx_vector_id`          | _UUID_      | no       | ID of a ready FX rates set of your group. Defaults to the group's active one. |

## Retrieve all portfolios

```shell
curl "https://api.allphins.com/api/v1/portfolios/?label=in_force" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "meta": null,
  "results": [
    {
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "Cyber Client 1 2024",
      "created_at": "2024-05-20T07:52:55.491729Z",
      "updated_at": "2024-05-23T14:31:10.591036Z",
      "data_update_time": "2024-05-22T00:00:00Z",
      "renewal_date": "2024-12-31",
      "premium": 9500,
      "max_exposure": 190000.0,
      "policies": [
        {
          "id": 1234,
          "name": "Policy 1",
          "type": "excess_of_loss",
          "start_date": "2024-01-01",
          "end_date": "2024-12-31",
          "status": "written",
          "is_interlocking": false,
          "coverage_basis": "per_event"
        }
      ],
      "client": 1,
      "client_name": "Cyber Client 1",
      "timeline": "Active",
      "renewal": null,
      "year_of_account": 2024,
      "transaction": "inward",
      "maximum_line": null,
      "maximum_line_currency": null,
      "usd_maximum_line": null,
      "total_income": null,
      "total_income_currency": null,
      "usd_total_income": null,
      "datasources": [
        {
          "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "status": "Ready",
          "original_filename": "Risk_list.xlsx"
        }
      ],
      "main_datasource_status": "Ready",
      "has_rules": false,
      "policies_labels": [],
      "mapped_perils": [],
      "cedant": 1233,
      "next_renewal": "2024-12-31",
      "reference": null,
      "tags": [],
      "labels": [],
      "section": null,
      "fx_vector": null,
      "display_currency": "USD"
    }
  ]
}
```

This endpoint returns the portfolios of your active group. It is paginated with `page` and `pageSize` (default 30, maximum 1000), see [Pagination](#pagination).

### HTTP Request

`GET https://api.allphins.com/api/v1/portfolios/`

### Query Parameters

| Parameter                   | Description                                                                                                                         |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `label`                     | `in_force` keeps the portfolios with at least one inward or outward policy whose dates cover today, whatever the policy's status.    |
| `name`                      | Search on the portfolio name and the client name. Every word must match one of them.                                                 |
| `year_of_account`           | Year of account. `year_of_account__in` takes a comma-separated list.                                                                 |
| `transaction`               | `inward` or `outward`. `transaction__in` takes a comma-separated list.                                                               |
| `client`                    | Client ID. `client__in` takes a comma-separated list.                                                                                |
| `timeline`                  | Comma-separated `timeline` values, for example `Active,Starting Soon`.                                                               |
| `portfolios_labels`         | Comma-separated label names.                                                                                                         |
| `ordering`                  | `name` (default), `client__name`, `renewal_date`, `max_exposure`, `next_renewal` or `data_update_time`. Prefix with `-` for descending. |
| `display_currency`          | ISO currency code to return the amounts in. See [Display currency](#portfolio-display-currency).                                               |
| `light`                     | When present, each row only holds `id`, `name`, `client_name` and `tags`.                                                            |
| `page`, `pageSize`          | See [Pagination](#pagination).                                                                                                       |

## Retrieve a portfolio

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/ \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

> The above command returns JSON structured like this:

```json
{
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "Cyber Client 1 2024",
  "premium": 9500,
  "max_exposure": 190000.0,
  "created_at": "2024-05-20T07:52:55.491729Z",
  "updated_at": "2024-05-23T14:31:10.591036Z",
  "policies": [
    {
      "id": 1234,
      "name": "Policy 1",
      "type": "excess_of_loss",
      "start_date": "2024-01-01",
      "end_date": "2024-12-31",
      "status": "written",
      "...": "..."
    }
  ],
  "legacy": null,
  "renewal": null,
  "client": 1,
  "client_name": "Cyber Client 1",
  "timeline": "Active",
  "portfolio_class": "cyber",
  "year_of_account": 2024,
  "transaction": "inward",
  "maximum_line": null,
  "maximum_line_currency": null,
  "usd_maximum_line": null,
  "total_income": null,
  "total_income_currency": null,
  "usd_total_income": null,
  "renewal_date": "2024-12-31",
  "cedant": 1233,
  "tabs": [],
  "risks_ready": true,
  "datasources_light": [
    {
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "filename": "Risk_list.xlsx",
      "extension": "excel"
    }
  ],
  "max_policy_date": "2024-12-31",
  "has_rules": false,
  "tags": [],
  "labels": [],
  "reference": null,
  "fx_vector": null,
  "display_currency": "USD"
}
```

This endpoint retrieves one portfolio of your active group. It accepts the `display_currency` query parameter.

### HTTP Request

`GET https://api.allphins.com/api/v1/portfolios/:id/`

### URL Arguments

| Argument | Description                         |
| -------- | ----------------------------------- |
| `id`     | The ID of the portfolio to retrieve |

## Retrieve the datasources of a portfolio

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/datasources/ \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

This endpoint returns every datasource of the portfolio as a plain list, not paginated. Archives and deleted datasources are left out. Each item is a full [datasource](#datasources) object, including `status` and `update_time`.

### HTTP Request

`GET https://api.allphins.com/api/v1/portfolios/:id/datasources/`

## Get portfolio analytics

This endpoint returns analytics computed on the portfolio's risks, muted risks excluded. Results come as a table (split by one dimension) or as a matrix (attachment point × limit band). An optional comparison portfolio adds the same figures for a second portfolio and the variation between the two.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/:id/analytics/`

`GET` is accepted too, with the same parameters in the query string.

### Payload

| Parameter                 | Type         | Required                        | Description                                                                                         |
|---------------------------|--------------|---------------------------------|-----------------------------------------------------------------------------------------------------|
| `view`                    | _string_     | yes                             | `table` or `matrix`.                                                                                |
| `in_force_date`           | _string_     | yes                             | In-force date, `YYYY-MM-DD`.                                                                        |
| `split`                   | _string_     | no (default `attachment_point`) | Dimension of the table view. See the values below. The matrix view ignores it.                      |
| `filters`                 | _list[dict]_ | no (default `[]`)               | Filter groups on the risks. See [Filter attributes](#filter-attributes).                            |
| `comparison_portfolio_id` | _UUID_       | no                              | ID of another portfolio to compare against.                                                         |
| `display_currency`        | _string_     | no                              | ISO currency code for the amounts. Defaults to the portfolio's display currency.                    |

### Accepted `split` values

The splits offered for each line of business are set by Allphins and can change. They currently are:

| Line of business            | Splits                                                                                                                                       |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| `Terror direct fac`         | `country`, `state`, `county`, `city`, `zip_code`, `peril`, `occupancy`, `year_built`, `address_precision`, `construction_material`            |
| `Political Risk`            | `tenor`, `obligor_v2_country`, `country`, `risk_code`, `lloyds_pr_industry`                                                                  |
| `Political Risk direct fac` | `tenor`, `obligor_v2_country`, `country`, `risk_code`, `lloyds_pr_industry`, `trade_direct_status`                                           |
| `Casualty`                  | `attachment_point`, `country`, `naics_sector`, `naics_subsector`, `insured_revenue`                                                          |
| `Cyber`                     | `attachment_point`, `country`, `naics_sector`, `naics_subsector`, `insured_revenue`, `lloyds_revenue_band`, `cyber_cover_type`, `tech_stack` |

For Terror, Property and Onshore Energy, use [EDM analytics](#get-portfolio-edm-analytics).

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/analytics/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"view":"table","in_force_date":"2024-12-31","split":"country"}'
```

> The above command returns JSON structured like this (table view):

```json
{
  "current": {
    "data": [
      {"split": "United States", "total_premium": 1234567, "total_exposure": 98765432, "policy_count": 89, "rol": 0.0125},
      {"split": "France", "total_premium": 234567, "total_exposure": 12345678, "policy_count": 12, "rol": 0.019}
    ]
  },
  "comparison": null,
  "variation": null
}
```

> When `view` is `matrix`, each metric holds one row per attachment point, with one column per limit band:

```json
{
  "current": {
    "total_premium": [{"split": "(00) Primary", "(01) 0M-0.5M": 0, "...": 0}],
    "total_exposure": [{"split": "(00) Primary", "(01) 0M-0.5M": 0, "...": 0}],
    "policy_count": [{"split": "(00) Primary", "(01) 0M-0.5M": 0, "...": 0}],
    "rol": [{"split": "(00) Primary", "(01) 0M-0.5M": 0, "...": 0}]
  },
  "comparison": null,
  "variation": null
}
```

The table ends with `Grand total` and `Average value` rows. `comparison` and `variation` are `null` without a comparison portfolio. Variations are fractions: `0.1` means +10%. `total_premium` and `total_exposure` are in the display currency; responses may include a `display_currency` field naming it.

## Get portfolio EDM analytics

This endpoint returns analytics computed on the EDM/CEDE locations of the portfolio. Results are returned in three blocks: `current` (the requested portfolio), `comparison` (a second portfolio if `comparison_portfolio_id` is provided, otherwise `null`) and `variation` (the difference between the two).

It applies to Terror, Property and Onshore Energy. For any other line of business it returns a `200` with `{"current": [], "comparison": [], "variation": []}`.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/:id/edm_analytics/`

### Payload

| Parameter                 | Type         | Required | Description                                                                              |
|---------------------------|--------------|----------|------------------------------------------------------------------------------------------|
| `view`                    | _string_     | yes      | `table` or `matrix`.                                                                     |
| `split`                   | _string_     | yes      | Dimension to split by. Accepted values depend on `view` (see below). Others return `400`. |
| `filters`                 | _list[dict]_ | yes      | Filter groups on the risks. Pass `[]` for none. See [Filter attributes](#filter-attributes). |
| `comparison_portfolio_id` | _UUID_       | no       | ID of another portfolio to compare against.                                              |
| `display_currency`        | _string_     | no       | ISO currency code for the amounts. Defaults to the portfolio's display currency.         |

### Accepted `split` values

When `view` is `table`, every line of business accepts `attachment_point`, `excess_band` and `limit_band`. The other splits are set by Allphins per line of business and can change. They currently are:

| Line of business | Splits                                                                                                                                                                                                       |
|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Energy Onshore` | `country`, `state`, `county`, `city`, `occupancy`, `geo_resolution`, `distance_to_shore`, `loss_type`, `peril`, `region`, `year_built`, `construction_material_building`, `construction_material_others`, `exposure_set`, `asset_match` |
| `Property`       | `country`, `state`, `county`, `city`, `occupancy`, `geo_resolution`, `distance_to_shore`, `loss_type`, `peril`, `region`, `year_built`, `construction_material_building`, `construction_material_others`, `exposure_set` |
| `Terror`         | `country`, `state`, `county`, `city`, `occupancy`, `geo_resolution`, `distance_to_shore`, `loss_type`, `peril`, `region`, `year_built`, `construction_material_building`, `construction_material_others`, `exposure_set` |

When `view` is `matrix`: `attachment_point`, `geo_resolution`, `location_enrichment_details` or `profile` (limit band × excess band).

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
      {
        "split": "United States",
        "tiv": 1234567,
        "nb_lines": 120,
        "tiv_percentage": 0.84,
        "nb_lines_percentage": 0.8,
        "unique_location_tiv": 1200000,
        "nb_unique_locations": 100,
        "unique_location_tiv_percentage": 0.83,
        "nb_unique_locations_percentage": 0.79
      }
    ]
  },
  "comparison": null,
  "variation": null
}
```

> When `view` is `matrix`, each block holds one list of rows per statistic (`tiv`, `nb_lines`, ...), each row carrying one value per column:

```json
{
  "current": {
    "tiv": [{"split": "row label", "column label": 1000}],
    "nb_lines": [{"split": "row label", "column label": 3}]
  },
  "comparison": null,
  "variation": null
}
```

`tiv` and `unique_location_tiv` are in the display currency; responses may include a `display_currency` field naming it. `current` is `null` when the portfolio has no matching data.

### Filter attributes

`filters` is a list of filter groups. A risk matches when it matches every filter of at least one group.

| Field       | Type         | Description                                                       |
|-------------|--------------|-------------------------------------------------------------------|
| `filters`   | _list[dict]_ | The filters of the group.                                         |

Each filter is an object:

| Field       | Type        | Description                                         |
|-------------|-------------|-----------------------------------------------------|
| `attribute` | _string_    | Attribute type to filter on, for example `property_peril`. |
| `operator`  | _string_    | `is` keeps the risks carrying one of the values.    |
| `value`     | _list[int]_ | Attribute IDs.                                      |

The IDs of an attribute type are listed by `GET https://api.allphins.com/api/v1/attributes/?type=property_peril`; each item's `value` is the ID to use.

Example:

```json
[
  {
    "filters": [
      {
        "operator": "is",
        "attribute": "property_peril",
        "value": [67113764]
      }
    ]
  }
]
```

## Get the EDM risks of a portfolio

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/edm_risks/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"filters":[],"page":1,"pageSize":100}'
```

This endpoint returns the EDM/CEDE locations of one portfolio, in the same format as [EDM risks](#retrieve-edm-risks). For a line of business without EDM data it returns an empty page.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/:id/edm_risks/`

### Payload

| Parameter          | Type           | Required | Description                                                                         |
|--------------------|----------------|----------|-------------------------------------------------------------------------------------|
| `filters`          | _list[dict]_   | yes      | Filter groups. Pass `[]` for none. See [Filter attributes](#filter-attributes).     |
| `page`             | _int_          | no       | Page number, default 1.                                                             |
| `pageSize`         | _int_          | no       | Page size, default 30. There is no maximum.                                         |
| `q`                | _string_       | no       | Search text.                                                                        |
| `ordering`         | _list[string]_ | no       | Column names to sort by. Prefix with `-` for descending.                             |
| `display_currency` | _string_       | no       | ISO currency code for `gu_exposure_usd`. Defaults to the portfolio's display currency. |

## Get the cedant aggregation of a portfolio

```shell
curl https://api.allphins.com/api/v1/portfolios/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/cedant_agg/ \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -d '{"scenario_list":"yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy","computation_date":"2024-12-31","page":1,"page_size":30}'
```

> The above command returns JSON structured like this:

```json
{
  "count": 250,
  "data": {
    "current": [
      {
        "scenario": 123,
        "scenario_name": "Scenario A",
        "total_gross_loss": 15000000.0,
        "total_tiv": 40000000.0,
        "net_exposure": 2500000.0,
        "ledgers": {},
        "metadata": [],
        "attributes": []
      }
    ],
    "comparison": [],
    "variation": []
  }
}
```

This endpoint returns the portfolio's gross loss per scenario of a scenario list, one page at a time. `count` is the total number of scenarios.

### HTTP Request

`POST https://api.allphins.com/api/v1/portfolios/:id/cedant_agg/`

### Payload

| Parameter                 | Type        | Required | Description                                                                                       |
|---------------------------|-------------|----------|---------------------------------------------------------------------------------------------------|
| `scenario_list`           | _UUID_      | yes      | ID of the [scenario list](#scenarios).                                                            |
| `computation_date`        | _string_    | yes      | Date of the computation, `YYYY-MM-DD`.                                                            |
| `scenario_name`           | _string_    | no       | Keeps the scenarios whose name contains this text.                                                |
| `comparison_portfolio_id` | _UUID_      | no       | ID of another portfolio to compare against. `comparison` and `variation` are empty without it.    |
| `page`                    | _int_       | no       | Page number, default 1.                                                                           |
| `page_size`               | _int_       | no       | Page size, default 30. There is no maximum.                                                       |
| `ordering`                | _string_    | no       | `total_gross_loss`, `total_tiv` or `net_exposure`. Prefix with `-` for descending.                |
| `policies`                | _list[int]_ | no       | Policy IDs used for `net_exposure`. Defaults to the portfolio's written policies live at the date. |
| `display_currency`        | _string_    | no       | ISO currency code for the amounts. Defaults to the portfolio's display currency.                  |

`total_gross_loss`, `total_tiv` and `net_exposure` are in the display currency; responses may include a `display_currency` field naming it.

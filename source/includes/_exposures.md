# Risks

A risk is one exposure row (an asset, a location or an entity) of a [portfolio](#portfolios). Risks imported from Excel files are read with `POST /risks/list_details/`. Locations imported from EDM/CEDE databases (Terror, Property and Onshore Energy) are read with [`POST /risks/edm_list_details/`](#retrieve-edm-risks).

## Risk Object Attributes

| Attribute                 | Type          | Description                                                                                        |
|---------------------------|---------------|----------------------------------------------------------------------------------------------------|
| `id`                      | _int_         | Unique identifier for the object.                                                                  |
| `name`                    | _string_      | Name of the risk.                                                                                  |
| `portfolio_id`            | _UUID_        | ID of the [portfolio](#portfolios).                                                                |
| `portfolio_name`          | _string_      | Name of the portfolio.                                                                             |
| `portfolio_class`         | _string_      | Line of business of the portfolio.                                                                 |
| `portfolio_labels`        | _list[dict]_  | Labels of the portfolio.                                                                           |
| `datasource_id`           | _UUID_        | ID of the [datasource](#datasources) the risk was imported from.                                   |
| `datasource_name`         | _string_      | Original file name of the datasource.                                                              |
| `mute`                    | _bool_        | Whether the risk is muted. Muted risks are left out of the computations.                           |
| `disaggregation`          | _bool_        | Whether the risk is disaggregated.                                                                 |
| `gross_exposure`          | _float_       | Gross exposure, in the display currency (see below). `null` when the exposure comes from coverages. |
| `exposure_usd`            | _float_       | The same gross exposure, always in USD.                                                            |
| `exposure`                | _float_       | Exposure as imported, in `currency_exposure`.                                                      |
| `currency_exposure`       | _string_      | Currency of `exposure`.                                                                            |
| `assured_interest`        | _float_       | Assured interest.                                                                                  |
| `is_assured_interest`     | _bool_        | Whether the imported values are already at the assured interest share.                             |
| `cedant_share`            | _float_       | Cedant share.                                                                                      |
| `limit_100`               | _float_       | Limit at 100%, in `currency_limit`.                                                                |
| `excess`                  | _float_       | Excess, in `currency_excess`.                                                                      |
| `deductible`              | _float_       | Deductible, in `currency_deductible`.                                                              |
| `premium_100`             | _float_       | Premium at 100%, in `currency_premium`.                                                            |
| `limit_100_usd`, `excess_usd`, `deductible_usd`, `premium_100_usd` | _float_ | The same amounts converted to USD.                                       |
| `total_insured_value`     | _float_       | Total insured value, in `currency_tiv`.                                                            |
| `total_insured_value_usd` | _float_       | Total insured value converted to USD.                                                              |
| `currency`, `currency_limit`, `currency_excess`, `currency_deductible`, `currency_premium`, `currency_tiv` | _string_ | Currencies of the imported amounts. |
| `start_date`              | _date_        | Start date.                                                                                        |
| `end_date`                | _date_        | End date.                                                                                          |
| `dates`                   | _json_        | `start_date`, `end_date` and `origin`: the risk's own dates (`risk`), or else the span of its portfolio's policies (`policy`). |
| `attributes`              | _list[dict]_  | Attributes of the risk (see below).                                                                |
| `attributes_array`        | _list[int]_   | IDs of the risk's attributes.                                                                      |
| `coverages`               | _list[dict]_  | Coverages of the risk.                                                                             |
| `extra_fields`            | _json_        | Raw data from the risk import.                                                                     |
| `mapped_data`             | _json_        | Imported values, keyed by the field they were mapped to.                                           |
| `additional_direct_fields`| _json_        | Extra fields of direct risks.                                                                      |
| `amortization`            | _json_        | Amortization data.                                                                                 |
| `transfert`               | _object_      | The portfolio (`id`, `name`) with its policies.                                                    |
| `parent`                  | _int_         | ID of the parent risk, if any.                                                                     |
| `children`                | _list[dict]_  | Child risks, with the same fields except `parent` and `children`.                                  |

Each item of `attributes` has these fields:

| Field             | Type     | Description                                         |
|-------------------|----------|-----------------------------------------------------|
| `id`, `value`     | _int_    | ID of the attribute.                                |
| `type`            | _string_ | Attribute type, for example `country_v2`.           |
| `label`           | _string_ | Human-readable label.                               |
| `attribute_value` | _string_ | Code of the attribute, for example `NO`.            |
| `origin`          | _string_ | How the attribute was set, for example `normalized` or `manual`. |
| `parent`          | _int_    | Parent attribute, if any.                           |

### Risk display currency

`gross_exposure` is stored in USD and returned in a display currency: the `display_currency` body field when given, otherwise your group's display currency. When one of the portfolios on the page has no rate for that currency, the whole page stays in USD. Every other amount is returned as imported or in USD (`*_usd` fields). Responses may also include a `display_currency` field naming the currency of `gross_exposure`.

## Retrieve all risks

```shell
curl https://api.allphins.com/api/v1/risks/list_details/ \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{"portfolios": ["aaaaaaaa-bbbb-4ccc-8ddd-eeeeeeeeeeee"], "filters": [], "page": 1, "pageSize": 100}'
```

> The above command returns JSON structured like this:

```json
{
  "count": 1250,
  "next": "https://api.allphins.com/api/v1/risks/list_details/?page=2",
  "previous": null,
  "meta": null,
  "results": [
    {
      "id": 123456,
      "name": "Sleipner Complex",
      "portfolio_id": "aaaaaaaa-bbbb-4ccc-8ddd-eeeeeeeeeeee",
      "portfolio_name": "Cedant A 2024",
      "portfolio_class": "energy",
      "mute": false,
      "gross_exposure": 100000000.0,
      "exposure_usd": 100000000.0,
      "assured_interest": 1.0,
      "start_date": null,
      "end_date": null,
      "dates": {"start_date": "2024-01-01", "end_date": "2024-12-31", "origin": "policy"},
      "datasource_id": "ffffffff-ffff-ffff-ffff-ffffffffffff",
      "datasource_name": "Risk_list.xlsx",
      "attributes": [
        {
          "parent": null,
          "origin": "normalized",
          "value": 1647,
          "id": 1647,
          "label": "Norway",
          "type": "country_v2",
          "attribute_value": "NO",
          "...": "..."
        }
      ],
      "parent": null,
      "children": [],
      "...": "..."
    }
  ]
}
```

This endpoint returns the risks of your active group.

* It is paginated with `page` and `pageSize` **in the body** (default 30, maximum 1000), see [Pagination](#pagination).
* `results` holds parent risks only. Child risks are nested under their parent's `children`, but `count` includes them, so `count` can be higher than the number of rows you can page through.
* Muted risks are included. Use `only_unmuted` to leave them out.
* Risks appear once their datasource has been enriched: some show while the datasource is still `Calculating`, all once it is `Ready`. Risks with no datasource are always included.
* Technical (API) accounts are limited to 1 request per second on this endpoint, see [Rate limits](#rate-limits).

### HTTP Request

`POST https://api.allphins.com/api/v1/risks/list_details/`

### Body Arguments

| Argument           | Type                | Description                                                                                   |
|--------------------|---------------------|-----------------------------------------------------------------------------------------------|
| `filters`          | _list[dict]_        | Attribute filter groups. See [Filter attributes](#filter-attributes).                         |
| `portfolios`       | _list[UUID]_        | Portfolio IDs, as a list or a comma-separated string.                                         |
| `datasource_id`    | _UUID_              | Keeps the risks of one datasource.                                                            |
| `scenario_id`      | _int_               | Keeps the risks hit by a [scenario](#scenarios).                                              |
| `in_force_date`    | _string_            | `YYYY-MM-DD`. Keeps the risks live at that date.                                              |
| `ids`              | _list[int]_         | Risk IDs.                                                                                     |
| `q`                | _string_            | Search text on the risk name.                                                                 |
| `only_muted`       | _bool_              | Only muted risks.                                                                             |
| `only_unmuted`     | _bool_              | Only unmuted risks.                                                                           |
| `order_by`         | _string_            | Field to sort by. Prefix with `-` for descending.                                             |
| `display_currency` | _string_            | ISO currency code for `gross_exposure`.                                                       |
| `page`, `pageSize` | _int_               | See [Pagination](#pagination).                                                                |

## Retrieve EDM risks

```shell
curl https://api.allphins.com/api/v1/risks/edm_list_details/ \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{"filters": [], "page": 1, "pageSize": 100}'
```

> The above command returns JSON structured like this:

```json
{
  "count": 54000,
  "next": "https://api.allphins.com/api/v1/risks/edm_list_details/?page=2",
  "previous": null,
  "results": [
    {
      "portfolio_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "gu_exposure_usd": 2500000.0,
      "address": "1 Main Street",
      "attributes": [],
      "...": "..."
    }
  ]
}
```

This endpoint returns the EDM/CEDE locations of your active group, across portfolios. Each row holds the location's columns from the EDM/CEDE data, its `portfolio_id` and an `attributes` list. `gu_exposure_usd` is converted to the display currency like `gross_exposure` above, despite its name.

It is available to Terror groups, and to Property and Onshore Energy groups that are not direct & fac; other groups get a `403`. To read one portfolio's locations, use [`POST /portfolios/:id/edm_risks/`](#get-the-edm-risks-of-a-portfolio), which takes the same body.

### HTTP Request

`POST https://api.allphins.com/api/v1/risks/edm_list_details/`

### Body Arguments

| Argument           | Type           | Required | Description                                                                       |
|--------------------|----------------|----------|-----------------------------------------------------------------------------------|
| `filters`          | _list[dict]_   | yes      | Filter groups. Pass `[]` for none. See [Filter attributes](#filter-attributes).   |
| `page`             | _int_          | no       | Page number, default 1.                                                           |
| `pageSize`         | _int_          | no       | Page size, default 30. There is no maximum.                                       |
| `q`                | _string_       | no       | Search text.                                                                      |
| `ordering`         | _list[string]_ | no       | Column names to sort by. Prefix with `-` for descending.                           |
| `display_currency` | _string_       | no       | ISO currency code for `gu_exposure_usd`.                                          |

The response is `{count, next, previous, results}`. Invalid filters return a `400`.

## Retrieve a risk

```shell
curl https://api.allphins.com/api/v1/risks/123456/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint retrieves a specific risk of your active group. Its `gross_exposure` is in USD.

### HTTP Request

`GET https://api.allphins.com/api/v1/risks/:id/`

### URL Arguments

| Argument | Description                    |
| -------- | ------------------------------ |
| `id`     | The ID of the risk to retrieve |

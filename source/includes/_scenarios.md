# Scenarios

A scenario list groups scenarios. A scenario is an accumulation footprint, for example one asset group or one country. Computing a scenario list adds up the losses of the policies you select for each of its scenarios.

All amounts are floats.

## Scenario List Object Attributes

| Attribute                         | Type           | Description                                                                                                                       |
|-----------------------------------|----------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `id`                              | _UUID_         | Unique identifier for the object.                                                                                                 |
| `name`                            | _string_       | Name of the scenario list.                                                                                                        |
| `description`                     | _string_       | Description of the scenario list.                                                                                                 |
| `type`                            | _string_       | `auto` (scenarios are generated from the aggregation keys) or `manual` (scenarios are defined one by one).                        |
| `config`                          | _list[dict]_   | Configuration of the scenario list. An item with `attribute_id: null` is an aggregation key; an item with an `attribute_id` is a filter or a peril. |
| `attributes`                      | _list[string]_ | Attribute types used as aggregation keys.                                                                                         |
| `agg_keys`                        | _list[string]_ | Labels of the aggregation keys.                                                                                                   |
| `agg_attribute_types`             | _list[string]_ | Attribute types of the aggregation keys, read from `config`.                                                                      |
| `metadata_filters`                | _list[string]_ | Attribute types available as scenario metadata for these aggregation keys.                                                       |
| `perils`                          | _list[string]_ | Values of the perils set in `config`.                                                                                             |
| `filters`                         | _list[string]_ | Labels of the filters set in `config`.                                                                                            |
| `filters_config`                  | _list[dict]_   | The `config` items that are filters.                                                                                              |
| `line_of_business`                | _string_       | Line of business of the scenario list.                                                                                            |
| `is_featured`                     | _bool_         | Makes it appear in the 'Featured' section.                                                                                        |
| `is_favorite`                     | _bool_         | Makes it appear in the 'Favorite' section.                                                                                        |
| `scenario_list_section`           | _int_          | ID of the section the scenario list belongs to.                                                                                   |
| `icon`                            | _string_       | URL of the scenario list icon, or `null`.                                                                                         |
| `loss_ratio`                      | _float_        | Loss ratio applied to the scenarios (1 when not set).                                                                             |
| `damage_ratio`                    | _float_        | `loss_ratio` as a percentage. `null` for manual scenario lists.                                                                   |
| `market_loss_basis`               | _float_        | Market loss of a loss-based scenario list, or `null`.                                                                             |
| `market_loss_currency`            | _string_       | Currency of `market_loss_basis` (USD when not set).                                                                               |
| `hull`                            | _float_        | Share of the market loss on hull (0 to 1), or `null`.                                                                             |
| `liability`                       | _float_        | Share of the market loss on liability (0 to 1), or `null`.                                                                        |
| `address_precision_level`         | _list_         | Address precision levels the scenario list is restricted to, or `null`.                                                           |
| `is_config_valid_for_loss_ladder` | _bool_         | Whether the aggregation keys allow a loss ladder export.                                                                          |

## Retrieve all scenario lists

```shell
curl "https://api.allphins.com/api/v1/scenario_lists/?page=1&page_size=1000" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "config": [
        {
          "type": "lloyds_risk_code",
          "condition": "is",
          "attribute_id": null
        }
      ],
      "name": "Per Lloyds Risk Code",
      "description": null,
      "icon": null,
      "is_featured": false,
      "type": "auto",
      "attributes": ["lloyds_risk_code"],
      "line_of_business": "cyber",
      "is_favorite": false,
      "perils": [],
      "filters": [],
      "filters_config": [],
      "agg_keys": ["Lloyd's Risk Code"],
      "agg_attribute_types": ["lloyds_risk_code"],
      "loss_ratio": 1.0,
      "damage_ratio": 100.0,
      "market_loss_basis": null,
      "market_loss_currency": "USD",
      "hull": null,
      "liability": null,
      "scenario_list_section": 12,
      "is_config_valid_for_loss_ladder": false,
      "address_precision_level": null,
      "metadata_filters": []
    }
  ]
}
```

This endpoint returns the scenario lists of your active group.

### HTTP Request

`GET https://api.allphins.com/api/v1/scenario_lists/`

### Query Parameters

| Parameter                | Description                                                                           |
|--------------------------|---------------------------------------------------------------------------------------|
| `page`                   | Page number (default 1).                                                              |
| `page_size`              | Number of scenario lists per page (default 1000, maximum 10000). `pageSize` is ignored. |
| `type`                   | Only return scenario lists of this type: `auto` or `manual`.                          |
| `type__in`               | Comma-separated list of types.                                                        |
| `line_of_business`       | Only return scenario lists of this line of business.                                  |
| `line_of_business__in`   | Comma-separated list of lines of business.                                            |
| `name__icontains`        | Only return scenario lists whose name contains this text (case insensitive).          |

<aside class="notice">Modeling scenario lists are not returned unless you ask for them with <code>type__in=modeling</code>. They are then returned with a different set of fields, not covered here.</aside>

## Retrieve a scenario list

```shell
curl https://api.allphins.com/api/v1/scenario_lists/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint returns one [scenario list](#scenario-list-object-attributes) of your active group. It returns 404 if the scenario list does not exist or belongs to another group.

### HTTP Request

`GET https://api.allphins.com/api/v1/scenario_lists/:id/`

## Compute a scenario list

```shell
curl https://api.allphins.com/api/v1/scenario_lists/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/computeV3/ \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{"policies": [1234, 5678], "computation_date": "2026-01-01", "page": 1, "pageSize": 1000}'
```

> The above command returns JSON structured like this:

```json
{
  "count": 34576,
  "next": "https://api.allphins.com/api/v1/scenario_lists/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/computeV3/?page=2",
  "previous": null,
  "results": [
    {
      "id": 1,
      "scenario_name": "Johan Sverdrup Complex",
      "name": "Johan Sverdrup Complex",
      "attributes": [
        {
          "id": 101,
          "value": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "label": "Johan Sverdrup Complex",
          "type": "asset_group",
          "type_label": "Asset Group"
        }
      ],
      "meta_data": [
        {
          "id": 202,
          "value": "NO",
          "label": "Norway",
          "type": "country",
          "type_label": "Country"
        }
      ],
      "gross_loss": 315626504.18,
      "total_tiv": 2500000000.0,
      "outwards": 50000000.0,
      "net_reinstatement_plus_outward": 249279902.51,
      "reinstatement": 16346601.67,
      "ledgers": {},
      "outwards_reinstatement": 0.0,
      "aggregation_limit": null,
      "is_exceeding_aggregation_limit": false,
      "underlying_accumulation": 1200000000.0
    }
  ]
}
```

This endpoint computes the loss of every scenario of a scenario list for the policies you select. Scenarios are sorted by `gross_loss`, largest first.

### HTTP Request

`POST https://api.allphins.com/api/v1/scenario_lists/:id/computeV3/`

### Payload

All fields are optional. Any other key returns a 400 error.

| Argument                      | Type          | Description                                                                                                                                              |
|-------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `policies`                    | _list[int]_   | IDs of the policies to compute. If missing or empty, the response is empty (`count` 0).                                                                  |
| `computation_date`            | _string_      | Date of the computation (`YYYY-MM-DD`). Only exposure in force on that date counts. If missing, each policy's largest loss over all its dates is used.  |
| `page`                        | _integer_     | Page number, starting at 1 (default 1).                                                                                                                  |
| `pageSize`                    | _integer_     | Number of scenarios per page, at least 1 (default 30, no maximum).                                                                                       |
| `scenario_name`               | _string_      | Only return scenarios whose name contains this text (case insensitive).                                                                                  |
| `filters`                     | _object_      | Exposure filters, see below.                                                                                                                             |
| `metadata_filters`            | _object_      | Scenario metadata filters, see below.                                                                                                                    |
| `exceeding_aggregation_limit` | _bool_        | If `true`, only return scenarios whose loss exceeds their aggregation limit (default `false`).                                                           |
| `display_currency`            | _string_      | ISO currency code for the amounts, for example `EUR`. Defaults to your group's display currency. An unknown code returns 400.                            |
| `transaction`                 | _string_      | Accepted, but has no effect on this endpoint.                                                                                                            |

`filters` and `metadata_filters` take the form `{"filters": [{"operator": "is", "value": [101, 102]}]}`, where `value` is a list of attribute IDs and `operator` is `is` or `is_not`. All filters in the list must match.

* `filters` applies to the exposure. `is` keeps exposure that carries at least one of the attributes. `is_not` drops exposure that carries all of them, so use one `is_not` filter per attribute to exclude several.
* `metadata_filters` applies to the scenario metadata. `is` keeps scenarios that have at least one of the attributes; `is_not` keeps scenarios that have none of them.

A filter with an empty `value` is ignored.

### Pagination

Send `page` and `pageSize` in the JSON body. The `next` and `previous` links in the response put `page` in the URL, which this endpoint ignores: posting to them returns page 1 again. To read every scenario, increase `page` in the body and stop once `page * pageSize` is greater than or equal to the `count` of the first page. A page after the last one returns no results, and its `count` may be 0, so always use the `count` of the first page.

Each request recomputes the whole scenario list before returning one page, so a few large pages are faster overall than many small ones.

### Response

Amounts are in the display currency (the `display_currency` you sent, or your group's setting), except `aggregation_limit`, which is in USD. The response can also contain a `display_currency` key that names the currency used.

| Field                            | Type          | Description                                                                                                                                                         |
|----------------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`                             | _int_         | Scenario ID.                                                                                                                                                        |
| `scenario_name`                  | _string_      | Scenario name.                                                                                                                                                      |
| `name`                           | _string_      | Scenario name (same as `scenario_name`).                                                                                                                            |
| `attributes`                     | _list[dict]_  | Attributes that define the scenario: `id`, `value`, `label`, `type`, `type_label`.                                                                                  |
| `meta_data`                      | _list[dict]_  | Descriptive attributes of the scenario, such as country or location, with the same keys.                                                                            |
| `gross_loss`                     | _float_       | Loss to the selected policies, after their limit, excess and share, summed over the policies.                                                                       |
| `reinstatement`                  | _float_       | Reinstatement premium due on the selected policies for this loss. It reduces the net loss.                                                                          |
| `outwards`                       | _float_       | Recovery from your outward (retrocession) policies for this loss.                                                                                                   |
| `outwards_reinstatement`         | _float_       | Reinstatement premium you owe on your outward policies for that recovery.                                                                                           |
| `net_reinstatement_plus_outward` | _float_       | Net loss: `gross_loss - reinstatement + outwards_reinstatement - outwards`.                                                                                         |
| `underlying_accumulation`        | _float_       | Cedants' own loss for this scenario, across the portfolios of the selected policies, before your policy terms. 0 if none.                                           |
| `total_tiv`                      | _float_       | Total insured value of the exposed risks. It is converted only when all selected policies use the same exchange rate; otherwise it stays in USD.                    |
| `aggregation_limit`              | _float_       | Aggregation limit set on the scenario, in USD, or `null`.                                                                                                           |
| `is_exceeding_aggregation_limit` | _bool_        | `true` if the loss (in USD) is above `aggregation_limit`.                                                                                                           |
| `ledgers`                        | _dict_        | Breakdown of each policy's loss by head of cover, keyed by policy ID. Empty when no breakdown is available.                                                         |

For manual scenario lists, scenarios with no loss are also returned, after the others, with `gross_loss` 0.

## Retrieve the layer composition of a scenario list

```shell
curl "https://api.allphins.com/api/v1/scenario_lists/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/layers_composition/?computation_date=2026-01-01" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
[
  {
    "scenario_id": 1,
    "scenario_name": "Johan Sverdrup Complex",
    "cedant_name": "Cedant A",
    "portfolio_name": "Energy 2026",
    "policy_id": 1234,
    "policy_name": "Energy XL 2026",
    "policy_type": "excess_of_loss",
    "transaction_type": "inward",
    "labels": ["North Sea"],
    "gross_loss": 12500000.0,
    "policy_loss": 25000000.0,
    "client_loss": 80000000.0,
    "status": "written",
    "reference": "REF-001",
    "limit": 25000000.0,
    "excess": 50000000.0,
    "share": 0.5,
    "underlying_inward_policies": []
  }
]
```

This endpoint returns, for every scenario of the scenario list, one row per inward policy with a loss, and one row per outward policy.

<aside class="warning">This endpoint is not paginated. It returns every row for every scenario in one response. For very large scenario lists the request can run past the 300-second server time limit and fail with a server error. It is limited to 2 requests per minute per user.</aside>

### HTTP Request

`GET https://api.allphins.com/api/v1/scenario_lists/:id/layers_composition/`

### Query Parameters

| Parameter          | Description                                              |
|--------------------|----------------------------------------------------------|
| `computation_date` | Date of the computation (`YYYY-MM-DD`). Default: today.  |

All inward policies of your active group are used. Outward rows cover the outward policies in force on the computation date, for each scenario that has an inward loss.

### Response

| Field                        | Type           | Description                                                                                                                              |
|------------------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `scenario_id`                | _int_          | Scenario ID.                                                                                                                             |
| `scenario_name`              | _string_       | Scenario name.                                                                                                                           |
| `cedant_name`                | _string_       | Name of the client (cedant) of the policy's portfolio.                                                                                   |
| `portfolio_name`             | _string_       | Name of the policy's portfolio.                                                                                                          |
| `policy_id`                  | _int_          | Policy ID.                                                                                                                               |
| `policy_name`                | _string_       | Policy name.                                                                                                                             |
| `policy_type`                | _string_       | `direct`, `excess_of_loss`, `quota_share`, `aggregated_stop_loss` or `fac`.                                                              |
| `transaction_type`           | _string_       | `inward` or `outward`.                                                                                                                   |
| `labels`                     | _list[string]_ | Label names. Inward rows include the portfolio's labels; outward rows only the policy's own.                                             |
| `client_loss`                | _float_        | Inward row: loss to the cedant before your policy terms. Outward row: inward loss flowing into the outward policy.                       |
| `policy_loss`                | _float_        | Loss to the policy after its limit and excess, at 100%.                                                                                  |
| `gross_loss`                 | _float_        | `policy_loss` at the policy's share. On an outward row, this is the recovery.                                                            |
| `status`                     | _string_       | Policy status, for example `written` or `quote`.                                                                                         |
| `reference`                  | _string_       | Policy reference.                                                                                                                        |
| `limit`                      | _float_        | Policy limit in USD.                                                                                                                     |
| `excess`                     | _float_        | Policy excess in USD.                                                                                                                    |
| `share`                      | _float_        | Policy share.                                                                                                                            |
| `underlying_inward_policies` | _list[dict]_   | Outward rows: the inward policies the outward policy covers (`policy_id`, `type`, `labels`). Empty on inward rows.                      |

Inward loss amounts are in your group's display currency. This endpoint has no `display_currency` parameter.

## Retrieve the top cedant aggregation per scenario list

```shell
curl "https://api.allphins.com/api/v1/scenario_lists/cedant_top_agg/?computation_date=2026-01-01" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
[
  {
    "scenario_list_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "scenario_list_name": "Per Asset Group",
    "scenario_id": "1",
    "scenario_name": "Johan Sverdrup Complex",
    "exposure": 1200000000.0,
    "portfolio_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portfolio_name": "Energy 2026",
    "portfolio_year_of_account": 2026,
    "client_name": "Cedant A",
    "computation_date": "2026-01-01"
  }
]
```

For each cedant of your active group and each scenario list, this endpoint returns the scenario with the largest cedant loss (`exposure`) on the computation date. Rows are sorted by portfolio ID. Pairs with no loss are not returned.

This endpoint is not paginated. If the date is not a valid `YYYY-MM-DD` date, or the computation fails, it returns an empty list rather than an error. It does not check the line of business of your group.

### HTTP Request

`GET https://api.allphins.com/api/v1/scenario_lists/cedant_top_agg/`

### Query Parameters

| Parameter          | Description                                                                    |
|--------------------|--------------------------------------------------------------------------------|
| `computation_date` | Date of the computation (`YYYY-MM-DD`). Default: today.                        |
| `display_currency` | ISO currency code for `exposure`. Defaults to your group's display currency.   |

The response can also be an object with `display_currency` and `results` keys, where `results` holds the list above.

## Scenario Object Attributes

| Attribute          | Type          | Description                                                                 |
|--------------------|---------------|-----------------------------------------------------------------------------|
| `id`               | _int_         | Unique identifier for the object.                                           |
| `name`             | _string_      | Name of the scenario.                                                       |
| `scenario_list`    | _UUID_        | ID of the scenario list the scenario belongs to.                            |
| `line_of_business` | _string_      | Line of business of the scenario.                                           |
| `config`           | _list[dict]_  | Rules that define the scenario footprint.                                   |
| `attributes`       | _list[dict]_  | Attributes of the scenario: `id`, `value`, `label`, `type`, `type_label`.   |
| `aggregation_limit`| _float_       | Aggregation limit of the scenario in USD, or `null`.                        |

## Retrieve a scenario

```shell
curl https://api.allphins.com/api/v1/scenarios/1/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint returns one [scenario](#scenario-object-attributes). It returns 404 if the scenario does not belong to a scenario list of your active group.

### HTTP Request

`GET https://api.allphins.com/api/v1/scenarios/:id/`

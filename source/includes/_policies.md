# Policies

A policy is an insurance or reinsurance contract. It belongs to a [portfolio](#portfolios), which holds the risks, and carries the financial terms of the contract. The API only returns the policies of your active group, and never deleted ones.

## Policy Object Attributes

| Attribute                              | Type          | Description                                                                                                  |
|----------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------|
| `id`                                   | _int_         | Unique identifier for the object.                                                                            |
| `name`                                 | _string_      | Name of the policy.                                                                                          |
| `type`                                 | _string_      | `quota_share`, `excess_of_loss`, `direct`, `aggregated_stop_loss` or `fac`.                                  |
| `transaction`                          | _string_      | `inward`, `outward` or `cedant`. A `cedant` policy is the internal policy of an inward portfolio.            |
| `status`                               | _string_      | `quote`, `written`, `expired`, `declined`, `not_taken_up`, `work_in_progress`, `submission`, `business_planning`, `nbi_25`, `nbi_50`, `nbi_75`, `nbi_pre_binding` or `endorsement`. |
| `portfolio`                            | _UUID_        | ID of the [portfolio](#portfolios).                                                                          |
| `portfolio_name`                       | _string_      | Name of the portfolio.                                                                                       |
| `start_date`                           | _date_        | Start date, `YYYY-MM-DD`.                                                                                    |
| `end_date`                             | _date_        | End date, `YYYY-MM-DD`.                                                                                      |
| `risk_attached`                        | _bool_        | Whether the policy is risk attaching.                                                                        |
| `share`                                | _float_       | Share of the policy.                                                                                         |
| `premium_100`                          | _float_       | Premium at 100% share, in `premium_currency`.                                                                |
| `premium_currency`                     | _string_      | Premium currency.                                                                                            |
| `usd_premium_100`                      | _float_       | Premium at 100% share converted to USD.                                                                      |
| `limit`                                | _float_       | Limit, in `limit_currency`.                                                                                  |
| `limit_currency`                       | _string_      | Limit currency.                                                                                              |
| `usd_limit`                            | _float_       | Limit converted to USD.                                                                                      |
| `excess`                               | _float_       | Excess, in `excess_currency`.                                                                                |
| `excess_currency`                      | _string_      | Excess currency.                                                                                             |
| `annual_aggregate_deductible`          | _float_       | Annual aggregate deductible.                                                                                 |
| `annual_aggregate_deductible_currency` | _string_      | Currency of the annual aggregate deductible.                                                                 |
| `floating_deductible`                  | _float_       | Floating deductible.                                                                                         |
| `floating_deductible_currency`         | _string_      | Currency of the floating deductible.                                                                         |
| `usd_floating_deductible`              | _float_       | Floating deductible converted to USD.                                                                        |
| `ilw`                                  | _float_       | Industry loss warranty.                                                                                      |
| `ilw_currency`                         | _string_      | Currency of the industry loss warranty.                                                                      |
| `usd_ilw`                              | _float_       | Industry loss warranty converted to USD.                                                                     |
| `rol`                                  | _float_       | Rate on line: `premium_100` / `limit`.                                                                       |
| `combined_ratio`                       | _float_       | Combined ratio.                                                                                              |
| `loss_ratio_cap`                       | _float_       | Loss ratio cap.                                                                                              |
| `coverage_basis`                       | _string_      | `per_event`, `per_policy`, `per_location`, `per_obligor` or `per_risk`.                                      |
| `reinstatement`                        | _list[float]_ | Reinstatement percentages, one per reinstatement.                                                            |
| `reinstatement_type`                   | _string_      | `limited_multiple`, `limited_one`, `unlimited` or `no_reinstatement`.                                        |
| `number_of_reinstatements`             | _int_         | Number of reinstatements, derived from `reinstatement_type` and `reinstatement`.                             |
| `reference`                            | _string_      | Policy reference.                                                                                            |
| `description`                          | _string_      | Policy description.                                                                                          |
| `tags`                                 | _list[string]_| Names of the policy's labels and of its portfolio's labels.                                                  |
| `labels`                               | _list[dict]_  | Labels of the policy and of its portfolio: `id`, `name`, `color_background`, `color_font`.                   |
| `rules`                                | _string_      | Sublimit rules.                                                                                              |
| `encoded_rules_json`                   | _json_        | Sublimit rules in structured form.                                                                           |
| `outward_filter`                       | _json_        | Outward filters.                                                                                             |
| `is_interlocking`                      | _bool_        | Whether the policy is interlocking.                                                                          |
| `program`                              | _int_         | ID of the program the policy belongs to, if any.                                                             |
| `benefits`                             | _list[dict]_  | Policies this one benefits from.                                                                             |
| `recoveries_fed_from`                  | _list[dict]_  | Policies feeding recoveries into this one.                                                                   |
| `additional_direct_fields`             | _json_        | Extra fields of direct policies.                                                                             |
| `policies`                             | _list[dict]_  | The other policies of the same portfolio, cedant policy excluded.                                            |
| `datasources`                          | _list[dict]_  | Datasources of the portfolio: `id`, `status`, `original_filename`.                                           |
| `mapped_perils`                        | _list_        | Perils mapped in the portfolio's datasources.                                                                |
| `fx_vector`                            | _object_      | FX rates set of the portfolio: `name`, `origin`.                                                             |

<aside class="warning">Listing policies and retrieving one do not return quite the same object: <code>attachment_point</code>, <code>client_id</code>, <code>client_name</code>, <code>dates</code>, <code>deployed_limit</code>, <code>portfolio_labels</code> and <code>usd_excess</code> are only returned by <code>GET /policies/</code> and <code>/policies/filter/</code>, while <code>line_of_business</code> is only returned by <code>GET /policies/:id/</code>.</aside>

The list-only fields are:

| Attribute          | Type        | Description                                                                                  |
|--------------------|-------------|----------------------------------------------------------------------------------------------|
| `client_id`        | _int_       | ID of the client of the portfolio.                                                           |
| `client_name`      | _string_    | Name of the client of the portfolio.                                                         |
| `dates`            | _json_      | `start_date` and `end_date` of the policy.                                                   |
| `usd_excess`       | _float_     | Excess converted to USD.                                                                     |
| `deployed_limit`   | _float_     | `usd_limit` × `share`, in the display currency (see below). `null` when the limit is empty or 0. |
| `attachment_point` | _string_    | Attachment point band of the excess, for example `0-5M`.                                     |
| `portfolio_labels` | _list[int]_ | IDs of the portfolio's labels.                                                               |

### Policy display currency

Amounts are returned in their own currency (`limit` in `limit_currency`, and so on) or in USD (`usd_*` fields). The one exception is `deployed_limit` in the list, which is converted to a display currency: the `display_currency` parameter when given, otherwise your group's display currency. When no rate exists for that currency, it stays in USD. Responses may also include a `display_currency` field naming the currency of `deployed_limit`.

## Retrieve all policies

```shell
curl "https://api.allphins.com/api/v1/policies/?status=written&policy_live_at=2024-06-30" \
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
      "id": 123456,
      "name": "Layer 1",
      "type": "excess_of_loss",
      "transaction": "inward",
      "status": "written",
      "portfolio": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "portfolio_name": "Cedant A 2024",
      "client_id": 1,
      "client_name": "Client A",
      "start_date": "2024-01-01",
      "end_date": "2024-12-31",
      "dates": {"start_date": "2024-01-01", "end_date": "2024-12-31"},
      "share": 0.1,
      "limit": 10000000,
      "limit_currency": "USD",
      "usd_limit": 10000000,
      "excess": 10000000,
      "excess_currency": "USD",
      "usd_excess": 10000000,
      "premium_100": 500000,
      "premium_currency": "USD",
      "usd_premium_100": 500000,
      "rol": 0.05,
      "deployed_limit": 1000000,
      "attachment_point": "5-10M",
      "reinstatement": [1.0],
      "reinstatement_type": "limited_multiple",
      "number_of_reinstatements": 1,
      "risk_attached": false,
      "reference": "REF 123",
      "description": "This is a policy description",
      "tags": ["tag1"],
      "labels": [{"id": 1, "name": "tag1", "color_background": "#E0E0E0", "color_font": "#000000"}],
      "benefits": [],
      "recoveries_fed_from": [],
      "policies": [],
      "portfolio_labels": [],
      "datasources": [],
      "mapped_perils": [],
      "fx_vector": null,
      "...": "..."
    }
  ]
}
```

This endpoint is paginated with `page` and `pageSize` (default 30, maximum 1000), see [Pagination](#pagination). Cedant policies are left out unless you filter on `transaction`.

### HTTP Request

`GET https://api.allphins.com/api/v1/policies/`

### Query Parameters

| Parameter          | Description                                                                                                       |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| `status`           | One status. Use `status__in` with a comma-separated list for several.                                             |
| `type`             | One type. `type__in` or `types` take a comma-separated list.                                                      |
| `transaction`      | `inward`, `outward` or `cedant`. `transaction__in` takes a comma-separated list.                                  |
| `portfolio_id`     | Portfolio ID. `portfolio__id__in` takes a comma-separated list.                                                   |
| `labels`           | Comma-separated label names, matched on the policy's or its portfolio's labels.                                   |
| `policy_live_at`   | `YYYY-MM-DD`. Keeps the policies live at that date (start on or before it, end on or after it; empty dates match). |
| `filter_rule`      | `today` (live today) or `previous_1_1` (live on 1 January of this year).                                         |
| `start_date`, `end_date` | `YYYY-MM-DD`. Also accept the `__gte`, `__lte`, `__gt`, `__lt` suffixes.                                    |
| `search`           | Text searched in the portfolio name, reference, description and tags.                                             |
| `has_rules`        | `true` or `false`.                                                                                                |
| `ordering`         | Any policy field. Prefix with `-` for descending. `order_by=rol` or `order_by=-rol` sorts on rate on line.        |
| `display_currency` | ISO currency code for `deployed_limit`.                                                                           |
| `page`, `pageSize` | See [Pagination](#pagination).                                                                                    |

## Filter policies

```shell
curl "https://api.allphins.com/api/v1/policies/filter/?status=written,quote&filter_rule=today&details=true" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> Without `details`, the response is a list of policy IDs:

```json
[123456, 123457]
```

> With `details=true`, the response is a list of policies, with the same fields as `GET /policies/`.

This endpoint returns every matching **inward** policy at once. It is not paginated.

### HTTP Request

`GET https://api.allphins.com/api/v1/policies/filter/`

`POST https://api.allphins.com/api/v1/policies/filter/`

### Parameters

`GET` takes the parameters in the query string; `POST` takes them as a JSON body.

| Parameter          | GET                                   | POST                                     | Description                                                                 |
|--------------------|---------------------------------------|------------------------------------------|-----------------------------------------------------------------------------|
| `status`           | comma-separated list                  | list                                     | Policy statuses.                                                            |
| `types`            | comma-separated list                  | list                                     | Policy types.                                                               |
| `portfolio_id`     | UUID                                  | UUID                                     | Keeps one portfolio.                                                        |
| `policy_live_at`   | `YYYY-MM-DD`                          | `YYYY-MM-DD`                             | Keeps the policies live at that date.                                       |
| `filter_rule`      | `today` or `previous_1_1`             | `today` or `previous_1_1`                | Keeps the policies live today, or on 1 January of this year.                |
| `labels`           | comma-separated label names           | –                                        | Matched on the policy's or its portfolio's labels.                          |
| `label_groups`     | –                                     | list of lists of label IDs               | A policy must match one label of every group. Up to 10 groups of 25 IDs.    |
| `details`          | `true` or `false` (default)           | `true` or `false` (default)              | Return full policies instead of IDs.                                        |
| `display_currency` | ISO code                              | ISO code                                 | Currency of `deployed_limit` when `details` is `true`.                      |

An invalid status, type or date returns a `400`. With `details=true`, responses may come as `{"display_currency": "...", "results": [...]}` instead of a bare list.

## Retrieve a policy

```shell
curl https://api.allphins.com/api/v1/policies/123456/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint retrieves a single policy. Amounts are not converted: they are in their own currency or in USD.

### HTTP Request

`GET https://api.allphins.com/api/v1/policies/:id/`

### URL Arguments

| Argument | Description                      |
| -------- | -------------------------------- |
| `id`     | The ID of the policy to retrieve |

## Create a policy

```shell
curl https://api.allphins.com/api/v1/policies/ \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{
    "portfolio": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
    "name": "Layer 1",
    "type": "excess_of_loss",
    "status": "written",
    "start_date": "2024-01-01",
    "end_date": "2024-12-31",
    "share": 0.1,
    "limit": 10000000,
    "limit_currency": "USD",
    "excess": 10000000,
    "excess_currency": "USD",
    "premium_100": 500000,
    "premium_currency": "USD",
    "reinstatement": [1.0],
    "reinstatement_type": "limited_multiple",
    "risk_attached": false,
    "reference": "REF 123",
    "description": "This is a policy description",
    "benefits": []
  }'
```

### HTTP Request

`POST https://api.allphins.com/api/v1/policies/`

The body takes the writable fields of the policy object. `portfolio`, `share` and `benefits` (an empty list when there are none) are required. `transaction` defaults to `inward` and must match the portfolio's transaction. `status` defaults to `quote`. The `usd_*` fields are computed from the amounts and their currencies.

## Edit a policy

### HTTP Request

`PATCH https://api.allphins.com/api/v1/policies/:id/`

### URL Arguments

| Argument | Description                  |
| -------- | ---------------------------- |
| `id`     | The ID of the policy to edit |

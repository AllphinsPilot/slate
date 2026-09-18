# Policies

A policy is an insurance or reinsurance policy. It's dependent of a portfolio (which contains the risks) and contains all the financial parameters of the policy.

## Policy Object Attributes

| Attribute          | Type       | Description                                                    |
|--------------------|------------|----------------------------------------------------------------|
| `id`               | _int_      | Unique identifier for the object.                              |
| `type`             | _string_   | Policy type: `direct`, `excess_of_loss` or `quota_share`.      |
| `portfolio`        | _string_   | ID of the [`portfolio`](#portfolios) object.                   |
| `premium_100`      | _float_    | Premium at 100% share.                                         |
| `premium_currency` | _string_   | Premium currency.                                              |
| `usd_premium_100`  | _float_    | USD Premium at 100% share.                                     |
| `limit`            | _float_    | Policy limit.                                                  |
| `limit_currency`   | _string_   | Limit currency.                                                |
| `excess`           | _float_    | Policy excess.                                                 |
| `excess_currency`  | _string_   | Excess currency.                                               |
| `start_date`       | _datetime_ | Start date of the policy.                                      |
| `end_date`         | _datetime_ | End date of the policy.                                        |
| `risk_attached`    | _bool_     | Is it a risk attaching policy.                                 |
| `share`            | _float_    | Policy share.                                                  |
| `combined_ratio`   | _float_    | Combined ratio of the policy.                                  |
| `status`           | _string_   | Policy status: `written`, `quote`, `declined`, `not_taken_up`. |
| `reinstatement`    | _float_    | Reinstatement percentage.                                      |
| `reference`        | _string_   | Policy reference.                                              |
| `description`      | _float_    | Policy description.                                            |
| `tags`             | _list_     | List of tags.                                                  |
| `client_id`                            | _int_      | ID of the client the portfolio belongs to.                     |
| `client_name`                          | _string_   | Name of the client the portfolio belongs to.                   |
| `portfolio_name`                       | _string_   | Name of the [`portfolio`](#portfolios) the policy belongs to.  |
| `dates`                                | _json_     | Object representation of the start date and end date.          |
| `benefits`                             | _list_     | List of the benefits.                                          |
| `rules`                                | _list_     | Sublimit rules.                                                |
| `outward_filter`                       | _json_     | Outward filters.                                               |
| `rol`                                  | _float_    | Rate on line of the policy.                                    |
| `usd_limit`                            | _float_    | Limit converted to USD.                                        |
| `usd_excess`                           | _float_    | Excess converted to USD.                                       |
| `annual_aggregate_deductible`          | _float_    | Annual aggregate deductible.                                   |
| `annual_aggregate_deductible_currency` | _string_   | Currency of the annual aggregate deductible.                   |
| `floating_deductible_currency`         | _string_   | Currency of the floating deductible.                           |
| `usd_floating_deductible`              | _float_    | Floating deductible converted to USD.                          |
| `ilw_currency`                         | _string_   | Currency of the industry loss warranty.                        |
| `usd_ilw`                              | _float_    | Industry loss warranty converted to USD.                       |

<aside class="warning">Listing policies and retrieving one do not return quite the same object: <code>attachment_point</code>, <code>client_id</code>, <code>client_name</code>, <code>dates</code>, <code>deployed_limit</code>, <code>portfolio_labels</code> and <code>usd_excess</code> are only returned by <code>GET /policies/</code>, while <code>line_of_business</code> is only returned by <code>GET /policies/:id/</code>.</aside>

## Retrieve all policies

```shell
curl https://api.allphins.com/api/v1/policies/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
[
  {
    "description": "This is a policy description",
    "end_date": "2019-12-30T23:00:00",
    "excess_currency": "USD",
    "excess": 10000,
    "id": 123456,
    "limit_currency": "USD",
    "limit": 10000,
    "portfolio": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
    "premium_100": 10000,
    "reference": "REF 123",
    "reinstatement": 1,
    "risk_attached": false,
    "share": 0.1,
    "start_date": "2018-12-31T23:00:00",
    "status": "written",
    "tags": ["tag1", "tag2"],
    "type": "excess_of_loss",
    "benefits": [],
    "client_id": 1,
    "client_name": "Client A",
    "portfolio_name": "Cedant A 2020"
  }
]
```

### HTTP Request

`GET https://api.allphins.com/api/v1/policies/`

### URL Arguments

No argument

## Retrieve a policy

```shell
curl https://api.allphins.com/api/v1/policies/123456/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint retrieves a single policy.

### HTTP Request

`GET https://api.allphins.com/api/v1/policies/:id/`

### URL Arguments

| Argument | Description                      |
| -------- | -------------------------------- |
| `id`     | The ID of the policy to retrieve |

## Create a policy

```shell
curl
  -d '{
    "description": "This is a policy description",
    "end_date": "2019-12-30T23:00:00",
    "excess_currency": "USD",
    "excess": 1000,
    "limit_currency": "USD",
    "limit": 10000000,
    "portfolio": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
    "premium_100": 1000,
    "reference": "REF 123",
    "reinstatement": 1,
    "risk_attached": false,
    "share": 0.1,
    "start_date": "2018-12-31T23:00:00",
    "status": "written",
    "tags": ["tag1", "tag2"],
    "type": "excess_of_loss",
    "benefits": [],
    "client_id": 1,
  }'
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  https://api.allphins.com/api/v1/policies/
```

### HTTP Request

`POST https://api.allphins.com/api/v1/policies/`

## Edit a policy

### HTTP Request

`PATCH https://api.allphins.com/api/v1/policies/:id/`

### URL Arguments

| Argument | Description                  |
| -------- | ---------------------------- |
| `id`     | The ID of the policy to edit |

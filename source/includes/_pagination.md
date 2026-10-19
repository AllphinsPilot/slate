# Pagination

Endpoints that return many items use one of four pagination styles. The parameter names differ between styles, so check the table at the end of this section for each endpoint.

## Page and pageSize

> GET request:

```shell
curl "https://api.allphins.com/api/v1/policies/?page=2&pageSize=100" \
  -H "Authorization: Bearer YourAccessToken"
```

> POST request, with the page in the body:

```shell
curl https://api.allphins.com/api/v1/risks/list_details/ \
  -X POST \
  -H "Authorization: Bearer YourAccessToken" \
  -H "Content-Type: application/json" \
  -d '{"page": 2, "pageSize": 100}'
```

> Response:

```json
{
  "count": 1250,
  "next": "https://api.allphins.com/api/v1/policies/?page=3&pageSize=100",
  "previous": "https://api.allphins.com/api/v1/policies/?page=1&pageSize=100",
  "meta": null,
  "results": []
}
```

| Parameter  | Default | Description |
|------------|---------|-------------|
| `page`     | `1`     | Page number, starting at 1. `last` returns the last page. |
| `pageSize` | `30`    | Items per page. Maximum `1000`: a larger value returns 1000 items. A value below 1 or not a number falls back to 30. `None` (or `null` in a POST body) returns 100 items. |

On `GET`, send both parameters in the query string. On `POST`, send them in the JSON body. If `page` is in both the body and the query string, the body wins.

The response contains `count` (total number of items), `next` and `previous` (URLs of the adjacent pages, or `null`), `meta` (always `null`) and `results`.

A page number past the last page returns `404` `not_found`.

To read every page, start at `page=1` and increase `page` until `next` is `null`.

On `POST /risks/list_details/`, `count` also includes sub-risks, so it can be larger than the number of risks returned across all pages. Stop when `next` is `null`, not from `count`.

## page and page_size

> Request:

```shell
curl "https://api.allphins.com/api/v1/scenario_lists/?page=1&page_size=500" \
  -H "Authorization: Bearer YourAccessToken"
```

> Response:

```json
{
  "count": 12,
  "next": null,
  "previous": null,
  "results": []
}
```

| Parameter   | Default | Description |
|-------------|---------|-------------|
| `page`      | `1`     | Page number, starting at 1. |
| `page_size` | `1000`  | Items per page. Maximum `10000`. |

Note the snake case `page_size`. The response contains `count`, `next`, `previous` and `results`. A page number past the last page returns `404` `not_found`. Follow `next` until it is `null`.

## page and pageSize in the body of a computation

> Request:

```shell
curl https://api.allphins.com/api/v1/scenario_lists/:id/computeV3/ \
  -X POST \
  -H "Authorization: Bearer YourAccessToken" \
  -H "Content-Type: application/json" \
  -d '{"policies": [101, 102], "page": 1, "pageSize": 100}'
```

> Response:

```json
{
  "count": 250,
  "next": "https://api.allphins.com/api/v1/scenario_lists/:id/computeV3/?page=2",
  "previous": null,
  "results": []
}
```

| Parameter  | Default | Description |
|------------|---------|-------------|
| `page`     | `1`     | Page number, starting at 1. Body only. |
| `pageSize` | `30`    | Items per page. Body only. There is no maximum, but large pages take longer and can exceed the 300-second limit. |

The response contains `count`, `next`, `previous` and `results`.

The server reads `page` and `pageSize` only from the JSON body. The `next` and `previous` URLs add `?page=` to the URL, which the server ignores: following `next` returns the page given in the body, not the next one. Do not use them.

A `page` past the last page returns no results.

To read every page, call `page=1`, read `count`, then call `page=2`, `3`, ... up to `ceil(count / pageSize)`, with the same `pageSize` each time.

## Assets

> Request:

```shell
curl "https://api.allphins.com/api/v1/assets/?page=2" \
  -H "Authorization: Bearer YourAccessToken"
```

> Response:

```json
{
  "results": [],
  "count": 5230
}
```

`GET /assets/` returns 30 assets per page. The page size cannot be changed. Pass `page` in the query string, starting at 1. The response contains `results` and `count` (the total number of matching assets), with no `next` or `previous` link. Read pages until `page × 30 ≥ count`.

## Endpoints

| Endpoint | Pagination |
|----------|------------|
| `GET /policies/` | `page` and `pageSize` |
| `GET /portfolios/` | `page` and `pageSize` |
| `POST /risks/list_details/` | `page` and `pageSize`, in the body |
| `GET /scenario_lists/` | `page` and `page_size` |
| `POST /scenario_lists/:id/computeV3/` | `page` and `pageSize` in the body, links unusable |
| `GET /assets/` | `page` only, 30 per page |
| `GET` and `POST /policies/filter/` | Not paginated: one response with every match |
| `GET /scenario_lists/:id/layers_composition/` | Not paginated: one response with every scenario of the list |
| `GET /scenario_lists/cedant_top_agg/` | Not paginated |
| `GET /portfolios/:id/datasources/` | Not paginated |
| `GET` and `POST /portfolios/:id/analytics/` | Not paginated |
| `POST /portfolios/:id/edm_analytics/` | Not paginated |
| `GET /families/:family_id/`, `GET /families/:family_id/users/` | Not paginated |
| Single objects: `GET /portfolios/:id/`, `GET /policies/:id/`, `GET /risks/:id/`, `GET /assets/:id/`, `GET /reports/:id/`, `GET /users/` | Not paginated |
| Writes: `POST /portfolios/`, `POST` and `PATCH /policies/`, `POST /reports/`, token and user endpoints | Not paginated |

Responses that are not paginated can be large. On a large scenario list, `layers_composition` can exceed the server's 300-second limit and fail. See [Limits](#limits).

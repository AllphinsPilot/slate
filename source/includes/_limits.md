# Limits

## Rate limits

| Scope | Limit |
|-------|-------|
| Authenticated requests, all endpoints | 40 requests per second per account |
| Unauthenticated requests, including `/token/`, `/token/refresh/`, `/token/verify/` and `/token/blacklist/` | 20 requests per minute per IP address |
| `POST /risks/list_details/`, technical (API) accounts | 1 request per second per account |
| `GET /scenario_lists/:id/layers_composition/` | 2 requests per minute per account |
| Creating portfolios (`POST /portfolios/`) | 10 per minute per account |
| Creating scenario lists (`POST /scenario_lists/`) | 10 per minute per account |

The per-second limit on `POST /risks/list_details/` also counts your other API requests. Leave at least one second after any request before calling it.

Access tokens last 15 minutes, so refresh them rather than requesting new tokens: `/token/` shares the 20-per-minute limit with the other token endpoints.

> Response when a limit is reached:

```json
{
  "message": "An error occurred while processing the request.",
  "code": "throttled",
  "details": "Request was throttled.",
  "status_code": 429
}
```

When a limit is reached, the API returns `429` with code `throttled`. The `Retry-After` header gives the number of seconds to wait before the next request.

## Timeouts

A request that runs longer than 300 seconds is stopped by the server and by the gateway. The client then receives a `502` or `504`, whose body may not be JSON.

## Request size

A request body larger than 128 MB returns `413` with code `request_body_too_large`.

## Large results

Some endpoints return the whole result in one response (see [Pagination](#pagination)). On large data sets these responses can be slow and can exceed the 300-second limit:

- `GET /scenario_lists/:id/layers_composition/` returns every scenario of the list. On a large scenario list it can exceed the server's 300-second limit and fail.
- `POST /scenario_lists/:id/computeV3/` has no maximum `pageSize`. Prefer several smaller pages to one very large page.

Send requests one at a time when a response is large, and wait for each response before sending the next.

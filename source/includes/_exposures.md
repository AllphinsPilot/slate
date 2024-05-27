# Risks

## Risk Object Attributes

| Attribute          | Type        | Description                                    |
| ------------------ | ----------- | ---------------------------------------------- |
| `id`               | _int_       | Unique identifier for the object.              |
| `name`             | _string_    | Name of the risk.                              |
| `start_date`       | _timestamp_ | Start date.                                    |
| `end_date`         | _timestamp_ | End date.                                      |
| `gross_exposure`   | _float_     | Gross Exposure in USD                          |
| `portfolio_id`     | _string_    | ID of the [`portfolio`](#portfolios) object.   |
| `portfolio_name`   | _string_    | Name of the [`portfolio`](#portfolios) object. |
| `extra_fields`     | _json_      | Raw data from the risk import.                 |
| `assured_interest` | _float_     | Assured interest.                              |
| `mute`             | _bool_      | Is the risk muted or not.                      |
| `attributes`       | _list_      | List of attributes for this risk.              |

## Retrieve all risks

```shell
curl
  -d '{"filters": {}}'
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  https://app.allphins.com/api/v1/risks/list_details/
```

> The above command returns JSON structured like this:

```json
{
  "count": 100000,
  "next": "https://app.allphins.com/api/v1/risks/list_details/?page=2",
  "previous": null,
  "results": [
    {
      "id": 123456,
      "name": "Sleipner Complex",
      "portfolio_id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
      "portfolio_name": "Cedant A 2020",
      "extra_fields": null,
      "assured_interest": 1.0,
      "mute": false,
      "gross_exposure": 100000000000.0,
      "start_date": null,
      "end_date": null,
      "attributes": [
        {
          "id": 1338,
          "label": "Offshore Installation",
          "value": "offshore_installation",
          "type": "risk_category"
        },
        {
          "id": 1647,
          "label": "Norway",
          "value": "NO",
          "type": "country"
        },
        {
          "id": 2608,
          "label": "-96.0",
          "value": -96.0,
          "type": "elevation"
        },
        {
          "id": 5177,
          "label": "[1.907036, 58.366697]",
          "value": [1.907036, 58.366697],
          "type": "coordinates"
        },
        {
          "id": 156836,
          "label": "Sleipner A",
          "value": "589e508b-a184-4081-af94-2907b5d768c6",
          "type": "asset"
        },
        {
          "id": 156837,
          "label": "Sleipner East Complex",
          "value": "c8c57415-65d0-4308-814f-092dd75826f2",
          "type": "asset_complex"
        },
        {
          "id": 564105,
          "label": "Europe",
          "value": "europe",
          "type": "region"
        }
      ]
    }
  ]
}
```

This endpoint retrieves all the risks, with a 30 items pagination.

### HTTP Request

`POST https://app.allphins.com/api/v1/risks/list_details/`

### URL Arguments

| Argument | Description                          |
| -------- | ------------------------------------ |
| `filter` | Json object to filter the risk query |

## Retrieve a risk

```shell
curl https://app.allphins.com/api/v1/risks/123456/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint retrieves a specific risk.

### HTTP Request

`GET https://app.allphins.com/api/v1/risks/:id/`

### URL Arguments

| Argument | Description                    |
| -------- | ------------------------------ |
| `id`     | The ID of the risk to retrieve |

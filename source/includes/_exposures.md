# Risks

## Retrieve all risks

```shell
curl 
  -d '{"filters": {}}'
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
  https://app.allphins.com/api/v1/exposures/list_details/
```

> The above command returns JSON structured like this:

```json
{
  "count": 253133,
  "next": "https://app.allphins.com/api/v1/exposures/list_details/?page=2",
  "previous": null,
  "results": [
    {
      "id": 6097763,
      "name": "Sleipner Complex",
      "portfolio_id": "b8a75ca9-91fc-43d8-b729-b9c47921e246",
      "portfolio_name": "Portfolio Name Example",
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
          "value": [
            1.907036,
            58.366697
          ],
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
    },
  ]
}
```

This endpoint retrieves all the risks. A risk is a object linking an asset (or a list of assets) and a list of policies (also named Heads of cover or interests). A risk belongs to a portfolio, which contains an `exposure_rules`. The `exposure_rules` allows the computation of the `company_exposure`.


### HTTP Request

`POST https://app.allphins.com/api/v1/exposures/list_details/`

### The risk object

#### Attributes

Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`name` | *string* | Name of the risk.
`assets` | *list of objects* | List of [`assets`](#assets) linked to this risk.
`stats` | *object* | Simple statistics on the risk object.
`policies` | *list of object* | List of policies (aka Heads of cover or Interests).
`risks_count` | *integer* | Number of policies (will be renamed `interest_count` in the next release).
`company_exposure` | *float* | Exposure on this asset computed with the `exposure_rules`.
`exposure_rules` | *object* | Exposure rule inherited from the [`portolio`](#portolios) object.
`portfolio` | *string* | ID of the [`portolio`](#portolios) object.


## Retrieve a risk

```shell
curl https://app.allphins.com/api/v1/exposures/7863/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

This endpoint retrieves a specific risk.

### HTTP Request

`GET https://app.allphins.com/api/v1/exposures/:id/`

### URL Arguments

Argument | Description
--------- | -----------
`id` | The ID of the risk to retrieve

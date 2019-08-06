# Risks (ex-exposures)

## Retrieve all risks

```shell
curl https://app.allphins.com/api/v1/exposures/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "count": 3276,
    "next": "http://35.197.208.182/api/v1/exposures/?page=2",
    "previous": null,
    "results": [
        {
            "id": 7863,
            "name": "Deepwater Frontier (Scrapped / Sold for Scrap)",
            "assets": [
                {
                    "id": "4d767860-5f54-49cc-81be-7e1d4fe2ec0c",
                    "rank": 1,
                    "name": "Deepwater Frontier (Scrapped / Sold for Scrap)",
                    "companies": [
                        {
                            "id": 220631,
                            "name": "Transocean Ltd",
                            "operating": false,
                            "shares": 100,
                            "asset": "4d767860-5f54-49cc-81be-7e1d4fe2ec0c",
                            "company": "2c3060ce-f92d-4083-92a0-5a00040a809f"
                        }
                    ],
                    "status": "In place",
                    "asset_category": "offshore_mobile",
                    "asset_type": "Offshore Rig",
                    "asset_subtype": [
                        "Drillship"
                    ]
                }
            ],
            "stats": {
                "capex_sum": 0,
                "raised_flags": 0
            },
            "policies": [
                {
                    "id": "145915be-fbf4-4dcc-8f25-68193f3dfd79",
                    "name": "Deepwater Frontier (Scrapped / Sold for Scrap)",
                    "slip_reference": "J1209K18CNWG",
                    "policy": "TPL",
                    "gross_exposure": 5374000,
                    "policy_normalized": "tpl",
                    "policy_type": "liability_type"
                },
                {
                    "id": "2c702757-31f5-4700-bbe3-c73bb2a58c61",
                    "name": "Deepwater Frontier (Scrapped / Sold for Scrap)",
                    "slip_reference": "J1209K18DNWG",
                    "policy": "TPL",
                    "gross_exposure": 4375000,
                    "policy_normalized": "tpl",
                    "policy_type": "liability_type"
                }
            ],
            "risks_count": 2,
            "company_exposure": 3361401,
            "exposure_rules": {
                "layers": [
                    {
                        "shares": 0.2,
                        "limit": 22500000,
                        "excess": 7500000,
                        "name": "Layer 1"
                    },
                    {
                        "shares": 0.2,
                        "limit": 30000000,
                        "excess": 30000000,
                        "name": "Layer 2"
                    }
                ]
            },
            "portfolio": "327aac44-8de0-4f49-9fbe-e0ece1dd2d3c"
        }
    ]
}
```

This endpoint retrieves all risks. A risk is a object linking an asset (or a list of assets) and a list of policies (also named Heads of cover or interests). A risk belongs to a portfolio, which contains an `exposure_rules`. The `exposure_rules` allows the computation of the `company_exposure`.

<aside class="warning">
    <code>asset_exposure</code> will be renamed <code>risk</code> in the next API release.
</aside>

### HTTP Request

`GET https://app.allphins.com/api/v1/exposures/`

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

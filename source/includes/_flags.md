# Flags

## Retrieve all flags

```shell
curl https://api.allphins.com/api/v1/flags/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
    "count": 15,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": "282331be-ffd9-40ec-abae-c3a83f24a0d1",
            "name": "Special Design",
            "description": "Flag is red if design is uncommon (e.g. TLP, SPAR...)",
            "long_description": null,
            "tag": "Design",
            "apply_to": "offshore_installation",
            "function": "asset.subtype",
            "operator": "contains",
            "value": [
                "TLP (Tension Leg Platform)",
                "SPAR",
                "FLNG",
                "CT/CPT (Compliant Piled Tower)",
                "GBS"
            ]
        },
        {
            "id": "b2dd1bb5-c5ce-4835-8120-e1e7b5d90679",
            "name": "Hurricane Zone",
            "description": "Flag is red if installation is in a hurricane zone",
            "long_description": null,
            "tag": "Natural Perils",
            "apply_to": "all",
            "function": "asset.location",
            "operator": "is_contained_in",
            "value": {
                "type": "FeatureCollection",
                "features": [
                    {
                        "type": "Feature",
                        "geometry": {
                            "type": "Polygon",
                            "coordinates": [
                                [
                                    [
                                        -158.78042791503978,
                                        18.158135212130983
                                    ],
                                    [
                                        -50.41387798231256,
                                        43.48997548394724
                                    ],
                                    [
                                        -49.17363607619357,
                                        11.113162692355603
                                    ],
                                    [
                                        -173.19829507347131,
                                        5.593490661718184
                                    ],
                                    [
                                        -158.78042791503978,
                                        18.158135212130983
                                    ]
                                ]
                            ]
                        },
                        "properties": {}
                    },
                    {
                        "type": "Feature",
                        "geometry": {
                            "type": "Polygon",
                            "coordinates": [
                                [
                                    [
                                        -182.2860121368075,
                                        5.747767485626247
                                    ],
                                    [
                                        -236.2367408586803,
                                        7.134203171792526
                                    ],
                                    [
                                        -240.2675447954374,
                                        14.588969823795535
                                    ],
                                    [
                                        -237.94208235052912,
                                        25.77093728438028
                                    ],
                                    [
                                        -219.0283187663588,
                                        36.24304781508238
                                    ],
                                    [
                                        -182.2860121368075,
                                        5.747767485626247
                                    ]
                                ]
                            ]
                        },
                        "properties": {}
                    },
                    {
                        "type": "Feature",
                        "geometry": {
                            "type": "Polygon",
                            "coordinates": [
                                [
                                    [
                                        156.80074481383804,
                                        -20.357622554354734
                                    ],
                                    [
                                        166.85076053331872,
                                        -14.113085569317276
                                    ],
                                    [
                                        188.8944207414324,
                                        -16.672332525106984
                                    ],
                                    [
                                        189.27366940186945,
                                        -23.00047054244035
                                    ],
                                    [
                                        178.60738220439478,
                                        -26.359665143491284
                                    ],
                                    [
                                        157.79626495226884,
                                        -25.763467773472865
                                    ],
                                    [
                                        156.80074481383804,
                                        -20.357622554354734
                                    ]
                                ]
                            ]
                        },
                        "properties": {}
                    },
                    {
                        "type": "Feature",
                        "geometry": {
                            "type": "Polygon",
                            "coordinates": [
                                [
                                    [
                                        -74.2124614326016,
                                        37.445060176820206
                                    ],
                                    [
                                        -69.83886594798679,
                                        41.49455648823312
                                    ],
                                    [
                                        -63.71584027781246,
                                        43.74698497204393
                                    ],
                                    [
                                        -51.907141478674696,
                                        46.522937925845184
                                    ],
                                    [
                                        -51.688465208069246,
                                        49.092422149344024
                                    ],
                                    [
                                        -33.21003504670653,
                                        52.74329832003702
                                    ],
                                    [
                                        -32.772672495137634,
                                        9.880943191760206
                                    ],
                                    [
                                        -58.795549111505075,
                                        9.018096159456933
                                    ],
                                    [
                                        -74.2124614326016,
                                        37.445060176820206
                                    ]
                                ]
                            ]
                        },
                        "properties": {}
                    }
                ]
            }
        },
        {
            "id": "e67d4335-5282-46aa-9949-6a89063a9113",
            "name": "Remaining Reserves",
            "description": "Flag is red if installation is producing fields with less than 30% remaining reserves together",
            "long_description": null,
            "tag": "Lifetime",
            "apply_to": "all",
            "function": "asset.remaining_value",
            "operator": "is_less_than_or_equal_to",
            "value": 30
        }
    ]
}
```

This endpoint retrieves all flags.

### HTTP Request

`GET https://api.allphins.com/api/v1/flags/`

### The flag object

A flag is a recipe to compute a specific risk for a given asset. This allows us to automatically compute if an asset is in a hurricane zone, or if the field reserves are low etc...

#### Attributes

Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`name` | *string* | Name of the flag.
`description` | *string* | Short description of the flag.
`long_description` | *string* | Long description of the flag.
`tag` | *string* | Tag of the flag (Lifetime, Design, Natural Perils etc...).
`apply_to` | *string* | Assets concerned by the flag (default is `all`, but can be set for a specific asset category).
`function` | *string* | Function to compute the flag.
`operator` | *string* | Boolean operator.
`value` | *string*, *float*, *integer*, *object* | Value to compare to the result of the function.

<aside class="info">If you want to use locations in your flags (like a hurricane zone) you can build your own zone on the <a target="_blank" href="https://maps.allphins.com/">Allphins zone builder</a>.</aside>

## Retrieve a flag

```shell
curl https://api.allphins.com/api/v1/flags/29647993-4153-4b0e-bbd5-c9239cf79ad5/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
    "id": "29647993-4153-4b0e-bbd5-c9239cf79ad5",
    "name": "Decommission Date",
    "description": "Flag is red if decommissioning date is passed",
    "long_description": null,
    "tag": "Lifetime",
    "apply_to": "all",
    "function": "asset.decommission_year",
    "operator": "is_less_than",
    "value": "now"
}
```

This endpoint retrieves a specific flag.

### HTTP Request

`GET https://api.allphins.com/api/v1/flags/:id/`

### URL Arguments

Argument | Description
--------- | -----------
`id` | The ID of the flag to retrieve.

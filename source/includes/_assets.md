# Assets

## Retrieve all assets

```shell
curl https://app.allphins.com/api/v1/assets/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "count": 24040,
    "next": "http://35.197.208.182/api/v1/assets/?page=2",
    "previous": null,
    "results": [
        {
            "id": "9a5bcef3-4d61-4fe6-be47-d98f849d50e2",
            "rank": 1,
            "name": "015/09-EH",
            "companies": [
                {
                    "id": 200147,
                    "name": "Statoil ASA",
                    "operating": true,
                    "shares": null,
                    "asset": "9a5bcef3-4d61-4fe6-be47-d98f849d50e2",
                    "company": "dafc1887-53aa-4873-8bae-19539a37fd16"
                }
            ],
            "status": "In place",
            "asset_category": "offshore_installation",
            "asset_type": "Subsea",
            "asset_subtype": [
                "Single Subsea Completion"
            ],
            "country": "Norway",
            "country_code": "NO"
        },
        {
            "id": "1e6b7b09-af15-48ce-95e9-d152e2f95c41",
            "rank": 1,
            "name": "015/29a-A01",
            "companies": [
                {
                    "id": 202835,
                    "name": "Chevron North Sea Ltd",
                    "operating": true,
                    "shares": null,
                    "asset": "1e6b7b09-af15-48ce-95e9-d152e2f95c41",
                    "company": "7b6d14c4-7d29-43fa-98af-f5dd6494169b"
                }
            ],
            "status": "In place",
            "asset_category": "offshore_installation",
            "asset_type": "Subsea",
            "asset_subtype": [
                "Single Subsea Completion"
            ],
            "country": "United Kingdom",
            "country_code": "GB"
        }
    ]
}
```

This endpoint retrieves all assets.

### HTTP Request

`GET https://app.allphins.com/api/v1/assets/`

### Query Arguments

Argument | Default | Description
--------- | ------- | -----------
`q` | null | Free text to filter the query on the asset textual information (like name or aliases).
`inplace` | true | If set to false, the result will also include planned and removed assets.
`country` | null | If set to a country id (or a countries ids, comma separated), the result will include assets located in this country (or countries)
`company` | null | If set to a company id (or a companies ids, comma separated), the result will include assets belonging to this company


### The assets objects

Assets in lists contains generic attributes belonging to any asset types (like name, category, country) and does not contains specifics attributes (like pile numbers, flaring, electric generation)

#### Attributes

Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`rank` | *float* | Search rank of the asset between 1 (best rank) and 0 (worst rank).
`name` | *string* | Asset name (usually the most popular).
`companies` | *object* | List of companies owning the asset.
`status` | *string* | Status of the object (in place, planned or removed).
`asset_category` | *string* | Major category of an asset.
`asset_type` | *string* | Asset type.
`asset_subtype` | *list of strings* | Subtype of an asset.
`country` | *string* | Country name.
`country_code` | *string* | Country code.


## Retrieve an asset

```shell
curl https://app.allphins.com/api/v1/assets/9f63a81b-ca34-4043-ada9-dcf41cdfe445
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "id": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
    "rank": 1,
    "country": "Republic of Angola",
    "country_code": "AO",
    "companies": [
        {
            "id": 215691,
            "name": "Chevron",
            "operating": true,
            "shares": 31,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "company": "33bfe93b-47a8-41e9-847d-026cdde1bdd6"
        },
        {
            "id": 215692,
            "name": "Sonangol",
            "operating": false,
            "shares": 20,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "company": "1e42a8ec-c23b-4045-b598-4aceb1ad97f6"
        },
        {
            "id": 215693,
            "name": "Eni",
            "operating": false,
            "shares": 20,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "company": "1067121e-cf9a-40e6-b9c0-894d2ef37994"
        },
        {
            "id": 215694,
            "name": "TOTAL E&P Angola",
            "operating": false,
            "shares": 20,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "company": "16ac1b59-7576-4d91-80d8-3ca1bb0c677a"
        },
        {
            "id": 215695,
            "name": "GalpEnergia",
            "operating": false,
            "shares": 9,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "company": "1a5cda42-faec-4449-bcb9-0a070d1df184"
        }
    ],
    "spatio_temp_position": [
        {
            "id": "c2ebf513-e4c8-4c86-a338-93da830998fe",
            "images": [],
            "name": "Belize",
            "aliases": [
                "Belize"
            ],
            "geo_type": "oil_and_gas_field",
            "comment": null,
            "country": "5c259d85-ab57-406b-b326-0e0401aefd56"
        },
        {
            "id": "d1412f4e-31cc-428d-98af-71d2fb240c0f",
            "images": [],
            "name": "Benguela",
            "aliases": [
                "Benguela"
            ],
            "geo_type": "oil_and_gas_field",
            "comment": null,
            "country": "5c259d85-ab57-406b-b326-0e0401aefd56"
        },
        {
            "id": "9da2a870-2e29-43c3-bb46-e857a9ad7e15",
            "images": [],
            "name": "Lobito",
            "aliases": [
                "Lobito"
            ],
            "geo_type": "oil_and_gas_field",
            "comment": null,
            "country": "5c259d85-ab57-406b-b326-0e0401aefd56"
        },
        {
            "id": "b52b8952-af9c-4ba6-8369-6a4d5b635f74",
            "images": [],
            "name": "Tomboco",
            "aliases": [
                "Tomboco"
            ],
            "geo_type": "oil_and_gas_field",
            "comment": null,
            "country": "5c259d85-ab57-406b-b326-0e0401aefd56"
        }
    ],
    "images": [
        {
            "image": "https://storage.googleapis.com/allphins_storage/asset_images/BBLT-CPT.jpg?Expires=1565126443&GoogleAccessId=239245509350-compute%40developer.gserviceaccount.com&Signature=moXMR0Lk6SuckyaOjWs%2FJ0l10FPb%2Fui57LaQbFkTneKplD%2FI8NNPJcpCJCk1I3DtjBk5WWSX1%2FacVpHMR%2BpoaumLc0BgPFKIPuzMFRkbqqbuEBbK7oMwnNAeIEg9DSWqzc7Rq2OsV3Yj9AFlabEDoZBZN5vvMqTjhZwQdFPMgAZKvrvx1jczrBmyU0ShwvxiwscGdK6y3JZpLEJ6jZz54PlzyvH4JTMfnsrm%2FZd3oAYKf5n%2BaKdQt9TzyFkJEYVmigxIcC3hzHx4Z9CkWEWkJpDTmQgZNtGqhpx4C3nWtMrSPW6GkGOCWtNqkUJ6xskJlYMYYL9Shp5iSyoXWtAMqg%3D%3D",
            "source": null,
            "id": 41
        },
        {
            "image": "https://storage.googleapis.com/allphins_storage/asset_images/BBLT_CPT.PNG?Expires=1565126444&GoogleAccessId=239245509350-compute%40developer.gserviceaccount.com&Signature=nKJOLhXXFnl%2BuItH%2BTv5x1P%2F1KzB1zwCKiot1LM7Bx6HoqdZug6b%2F%2BpuRl0TJVPAe1MvV3zIvfcAQErof79xOD8jr04OTp9zalwryuh70RbFumlI5Wjfci9DeSfVdrWhkn%2BQTxSyb%2BuXVYJbMg5wOX1mOiTdzqad4t7VaoRPt2nDrcDxB7Y1UZCBTmb5uWVvBhF9whFzGdLtg7nXd9u%2B7NMtEqcpZ6zc0%2Bb%2FMKqKSKlbI9gQsnfBTMeA0zValOM9eoiHq5RWJIEAj8fikwAGTYkpPNDZy037m6WbSpN9%2FWmczdBeUZXar5nSEph8yuZUEq0pihTE%2FTg4Z7Hb%2Fk9K9Q%3D%3D",
            "source": "Galp Energia",
            "id": 94
        }
    ],
    "flags": [
        {
            "id": 63991,
            "name": "Bridge",
            "description": "Flag is red if the installation is bridge-linked to another installation",
            "tag": "Aggregation",
            "raised": false,
            "error": false,
            "unknown": true,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "3064b8f8-a32b-440f-934b-1bcd6531a830"
        },
        {
            "id": 63992,
            "name": "Machinery",
            "description": "Flag is red if occupancy indicates heavy machinery (e.g. compression)",
            "tag": "Design",
            "raised": true,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "41f856b4-0496-4618-a23d-08daeb6b8302"
        },
        {
            "id": 165131,
            "name": "Special Design",
            "description": "Flag is red if design is uncommon (e.g. TLP, SPAR...)",
            "tag": "Design",
            "raised": true,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "282331be-ffd9-40ec-abae-c3a83f24a0d1"
        },
        {
            "id": 141944,
            "name": "Hurricane Zone",
            "description": "Flag is red if installation is in a hurricane zone",
            "tag": "Natural Perils",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "b2dd1bb5-c5ce-4835-8120-e1e7b5d90679"
        },
        {
            "id": 176885,
            "name": "High Pressure Reservoir",
            "description": "Flag is red if reservoir pressure > 10 000 psi",
            "tag": "Reservoir",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "632d862e-6299-48f6-a217-f41a83332303"
        },
        {
            "id": 63993,
            "name": "Ice Zone",
            "description": "Flag is red if installation is located in a zone where sea ice or icebergs can be found",
            "tag": "Natural Perils",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "ed9efaff-3c40-43e5-a61c-2450ff5923e8"
        },
        {
            "id": 63995,
            "name": "Terminal/ Storage",
            "description": "Flag is red if installation is a terminal or storing oil/ gas",
            "tag": "Aggregation",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "cd580f23-df35-4b60-a506-3b9333b72ce9"
        },
        {
            "id": 211181,
            "name": "High Temperature",
            "description": "Flag is red if reservoir temperature > 150 degC",
            "tag": "Reservoir",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "54d22c3f-94ed-4f16-9120-fc862bc5be4a"
        },
        {
            "id": 216920,
            "name": "Hydrogen sulfide (H2S)",
            "description": "Flag is red if field H2S concentration > 1% or in case of H2S specific process",
            "tag": "Reservoir",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "7a4c90bb-c0c7-4d5a-8946-4acc60d04323"
        },
        {
            "id": 63994,
            "name": "Decommission Date",
            "description": "Flag is red if decommissioning date is passed",
            "tag": "Lifetime",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "29647993-4153-4b0e-bbd5-c9239cf79ad5"
        },
        {
            "id": 258323,
            "name": "Remaining Reserves",
            "description": "Flag is red if installation is producing fields with less than 30% remaining reserves together",
            "tag": "Lifetime",
            "raised": true,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "e67d4335-5282-46aa-9949-6a89063a9113"
        },
        {
            "id": 281649,
            "name": "Installation Date",
            "description": "Flag is red if installation took place before 1988",
            "tag": "Lifetime",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "62a64c57-9d7a-470d-83df-9a8d8f2788d2"
        },
        {
            "id": 304969,
            "name": "Hub",
            "description": "Flag is red if installation is importing products from 5 or more other installations",
            "tag": "Aggregation",
            "raised": false,
            "error": false,
            "unknown": true,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "704a956e-4b91-46df-8207-28fcdfcbfad4"
        },
        {
            "id": 328241,
            "name": "Subsea",
            "description": "Flag is red in case of subsea system with more than 300 m water depth",
            "tag": "Design",
            "raised": true,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "ad8a9496-fa1a-424d-8132-80eedb862632"
        },
        {
            "id": 141945,
            "name": "High Wave Zone",
            "description": "Flag is red if installation is in a high wave zone",
            "tag": "Natural Perils",
            "raised": false,
            "error": false,
            "unknown": false,
            "asset": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "flag": "60b92624-187a-44a8-911c-9b318cc86d77"
        }
    ],
    "news": [],
    "linked_assets": [
        {
            "dependency_type": "export_to",
            "value": "gas",
            "source_id": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "source_asset": "BB-CPT",
            "source_asset_category": "offshore_installation",
            "target_id": "244c769f-7d7c-4179-a940-2d750540ac57",
            "target_asset": "Nemba SNX",
            "target_asset_category": "offshore_installation"
        },
        {
            "dependency_type": "export_to",
            "value": "oil",
            "source_id": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "source_asset": "BB-CPT",
            "source_asset_category": "offshore_installation",
            "target_id": "36b7797f-31cb-472b-8378-05c30ab147c8",
            "target_asset": "Kokongo A",
            "target_asset_category": "offshore_installation"
        },
        {
            "dependency_type": "export_to",
            "value": "oil & gas",
            "source_id": "ef5d48f8-9c0c-4a04-8050-68f939fe6f0f",
            "source_asset": "BBLT-A",
            "source_asset_category": "offshore_installation",
            "target_id": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "target_asset": "BB-CPT",
            "target_asset_category": "offshore_installation"
        },
        {
            "dependency_type": "export_to",
            "value": "oil & gas",
            "source_id": "601163ec-70b9-444d-813c-51ff3e9f7be9",
            "source_asset": "BBLT-B",
            "source_asset_category": "offshore_installation",
            "target_id": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "target_asset": "BB-CPT",
            "target_asset_category": "offshore_installation"
        },
        {
            "dependency_type": "export_to",
            "value": "oil & gas",
            "source_id": "b930f8a7-5577-4952-9f5c-2c9b66405534",
            "source_asset": "BBLT-C",
            "source_asset_category": "offshore_installation",
            "target_id": "9f63a81b-ca34-4043-ada9-dcf41cdfe445",
            "target_asset": "BB-CPT",
            "target_asset_category": "offshore_installation"
        }
    ],
    "indicators": {
        "raised_flags": 4,
        "flags_number": 15,
        "capex_estimate": 1444.2308
    },
    "production": [],
    "location": {
        "locations": [
            {
                "type": "point",
                "data": [
                    {
                        "coordinates": [
                            11.513333,
                            -5.49,
                            0
                        ]
                    }
                ]
            }
        ],
        "initialViewState": {
            "longitude": 11.513333,
            "latitude": -5.49,
            "zoom": 4
        }
    },
    "name": "BB-CPT",
    "aliases": [
        "Benguela/Belize CPT",
        "BB-CPT",
        "BBLT CPT",
        "Benguela Belize CPT",
        "BB CPT"
    ],
    "source": "IH86",
    "asset_category": "offshore_installation",
    "asset_type": "Offshore Fixed",
    "asset_subtype": [
        "CT/CPT (Compliant Piled Tower)"
    ],
    "installation_date": "2005",
    "installation_date_normalized": "2005-01-16T00:00:00",
    "status": "In place",
    "general_comment": "Subsea > 300m",
    "capex_estimate": 1444.2308,
    "created_at": "2019-01-16T22:10:37.637000",
    "updated_at": "2019-07-20T11:21:53.521327",
    "completeness": 0.714285714285714,
    "validation": false,
    "other_info": null,
    "search_vector": "'bb':2A,9A 'bb-cpt':1A 'bblt':6A 'beliz':8A 'benguela':7A 'benguela/belize':4A 'cpt':3A,5A",
    "occupancy": "Accomodation/Drilling/Production/Separation/Injection/Flaring",
    "piles_number": 12,
    "well_production_number": 24,
    "well_injection_number": 6,
    "weight_tons": 79100,
    "height_meter": null,
    "water_depth_meter": 435.9208984375,
    "construction_material": "Steel",
    "topside_weight_tons": 45000,
    "product": "Oil/Gas",
    "decommission_year": "2042",
    "evacuation": "Helicopter",
    "manned": "yes",
    "manufacturer": "Kiewit",
    "fpso_owner": null,
    "remaining_value": 15,
    "current_production": null,
    "oil_capacity_kbopd": 220,
    "gas_capacity_mscfd": null,
    "total_capacity_kboepd": 220,
    "storage_capacity_mcube": null,
    "pressure": null,
    "electric_generation": null,
    "oil_api": "31.5",
    "gas_oil_ratio": null,
    "flaring": "yes",
    "total_capex": "700.0",
    "total_capex_estimate": null,
    "size_meter": null,
    "closest_harbour": null,
    "type_of_wells": null,
    "number_of_platform_operated_in_country": null,
    "initiated_by": null,
    "terms": []
}
```

This endpoint retrieves a specific asset.

<aside class="warning">Every asset type has its own attributes, for example a <code>offshore_installation</code> have a <code>water_depth</code> but a <code>onshore_installation</code> doesn't.</aside>

### HTTP Request

`GET https://app.allphins.com/api/v1/assets/:id/`

### URL Parameters

Parameter | Description
--------- | -----------
`id` | The ID of the asset to retrieve

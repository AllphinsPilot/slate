# Assets

## Asset Object Attributes

Attribute | Type | Description
--------- | ------- | -----------
`id` | *UUID* | Unique identifier for the object.
`name` | *string* | Asset name (usually the most popular).
`aliases` | *list* | List of possible aliases for this asset.
`companies` | *list* | List of [`companies`](#companies) owning the asset.
`status` | *string* | Status of the object (in place, planned or removed).
`asset_category` | *string* | Major category of an asset.
`asset_type` | *string* | Asset type.
`asset_subtype` | *list of strings* | Subtype of an asset.
`country_code` | *string* | Country code.
`images` | *list of strings* | List of images urls.
`news` | *list of objects* | List of news concerning the asset.
`linked_assets` | *list of objects* | List of assets dependencies.
`indicators` | *object* | Simple statistics on the asset.
`location` | *list of objects* | List of locations.
`installation_date` | *integer* | Installation year.
`installation_date_normalized` | *timestamp* | Installation date.
`general_comment` | *string* | Comment on the asset.
`capex_estimate` | *float* | Estimated replacement value in Million dollars.
`created_at` | *timestamp* | Creation date of the asset.
`updated_at` | *timestamp* | last update on the asset.
`other_info` | *object* | Json with additional data.


**Specific Attributes**

Specific attributes depend on the asset category, here is a list of the possible categories:

* Offshore Installation
* Onshore Installation
* Offshore Mobile
* Onshore Mobile
* Renewable
* Unknown

Every category has its own attributes.

## Retrieve all assets

```shell
curl https://app.allphins.com/api/v1/assets/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "id": "3abba51f-e7fc-4af1-a65a-023d5b47745e",
      "name": "Cosmopolitan Field",
      "companies": [],
      "status": "Operational",
      "aliases": [
        "Cosmopolitan (United States)"
      ],
      "asset_category": "offshore_installation",
      "asset_type": "offshore_field_block",
      "asset_subtype": "st_offshore_field_block",
      "country_code": "US",
      "identification_numbers": []
    },
    {
      "id": "bd28d442-5ffb-4047-9698-b6912ca21e6c",
      "name": "Cosmos Field",
      "companies": [],
      "status": "Operational",
      "aliases": null,
      "asset_category": "offshore_installation",
      "asset_type": "offshore_field_block",
      "asset_subtype": "st_offshore_field_block",
      "country_code": "TN",
      "identification_numbers": []
    },
  ],
  "count": 1000000
}
```

This endpoint retrieves all assets.

### HTTP Request

`GET https://app.allphins.com/api/v1/assets/`

### Query Arguments

Argument | Default | Description
--------- | ------- | -----------
`name` | null | Free text to filter the query on the asset textual information (like name or aliases).


## Retrieve an asset

```shell
curl https://app.allphins.com/api/v1/assets/9f63a81b-ca34-4043-ada9-dcf41cdfe445
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
  "id": "3abba51f-e7fc-4af1-a65a-023d5b47745e",
  "companies": [
    
  ],
  "images": [
    
  ],
  "news": [
    
  ],
  "linked_assets": [
    
  ],
  "indicators": {
    "capex_estimate": null
  },
  "production": [
    
  ],
  "location": {
    "locations": [
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [
            -151.902539,
            59.860441
          ]
        },
        "properties": {
          
        }
      }
    ],
    "initial_view_state": {
      "longitude": -151.902539,
      "latitude": 59.860441,
      "zoom": 8.5
    },
    "is_moving": false,
    "date": null
  },
  "identification_numbers": [
    
  ],
  "name": "Cosmopolitan Field",
  "aliases": [
    "Cosmopolitan (United States)"
  ],
  "asset_category": "offshore_installation",
  "asset_type": "offshore_field_block",
  "asset_subtype": "st_offshore_field_block",
  "occupation": null,
  "installation_date": null,
  "installation_date_normalized": null,
  "status": "Operational",
  "general_comment": null,
  "capex_estimate": null,
  "latitude": "59.860441",
  "longitude": "-151.902539",
  "country_code": "US",
  "created_at": "2020-07-07T20:55:55.744133",
  "updated_at": "2020-12-21T10:47:55.258118",
  "other_info": null,
  "geo_shape": 6545
}
```

This endpoint retrieves a specific asset.

<aside class="warning">Every asset type has its own attributes, for example an <code>offshore_installation</code> has a <code>water_depth</code> but a <code>onshore_installation</code> doesn't.</aside>

### HTTP Request

`GET https://app.allphins.com/api/v1/assets/:id/`

### URL Arguments

Argument | Description
--------- | -----------
`id` | The ID of the asset to retrieve

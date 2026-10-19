# Assets

## Asset Object Attributes

| Attribute                      | Type              | Description                                                         |
|--------------------------------|-------------------|---------------------------------------------------------------------|
| `id`                           | *UUID*            | Unique identifier for the object.                                   |
| `name`                         | *string*          | Asset name (usually the most common one).                           |
| `aliases`                      | *list of strings* | Other names of the asset.                                           |
| `status`                       | *string*          | Status of the asset, for example `Operational`.                     |
| `asset_category`               | *string*          | Major category of the asset (see below).                            |
| `asset_type`                   | *string*          | Asset type.                                                         |
| `asset_subtype`                | *string*          | Asset subtype.                                                      |
| `country_code`                 | *string*          | ISO country code.                                                   |
| `identification_numbers`       | *list of objects* | Identification numbers, such as an IMO number.                      |
| `occupation`                   | *list of strings* | Occupation of the asset.                                            |
| `operator`                     | *string*          | Operator of the asset.                                              |
| `latitude`, `longitude`        | *string*          | Coordinates of the asset.                                           |
| `location`                     | *object*          | Geographic data of the asset.                                       |
| `images`                       | *list of objects* | Images of the asset.                                                |
| `linked_assets`                | *list of objects* | Dependencies with other assets.                                     |
| `production`                   | *list of objects* | Production figures.                                                 |
| `indicators`                   | *object*          | Simple statistics on the asset.                                     |
| `installation_date`            | *string*          | Installation date, as provided by the source.                       |
| `installation_date_normalized` | *timestamp*       | Installation date.                                                  |
| `general_comment`              | *string*          | Comment on the asset.                                               |
| `capex_estimate`               | *float*           | Estimated replacement value in million dollars.                     |
| `parent_id`, `parent_name`     | *UUID*, *string*  | Parent asset, if any.                                               |
| `created_at`                   | *timestamp*       | Creation date of the asset.                                         |
| `updated_at`                   | *timestamp*       | Last update on the asset.                                           |
| `other_info`                   | *object*          | JSON with additional data.                                          |

**Specific Attributes**

Specific attributes depend on the asset category. Categories include:

* `offshore_installation`
* `onshore_installation`
* `offshore_mobile`
* `onshore_mobile`
* `renewable`
* `unknown`

## Retrieve all assets

```shell
curl "https://api.allphins.com/api/v1/assets/?name=cosmo&page=1" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "id": "3abba51f-e7fc-4af1-a65a-023d5b47745e",
      "name": "Cosmopolitan Field",
      "status": "Operational",
      "aliases": [
        "Cosmopolitan (United States)"
      ],
      "asset_category": "offshore_installation",
      "asset_type": "offshore_field_block",
      "asset_subtype": "st_offshore_field_block",
      "country_code": "US",
      "identification_numbers": []
    }
  ],
  "count": 1
}
```

This endpoint searches the asset database. Each item only holds the fields shown above; retrieve an asset for the rest.

Results come 30 at a time. `page` selects the page; `start` and `end` select any slice instead (`start=0&end=100` returns the first 100). `count` is the total number of matches.

### HTTP Request

`GET https://api.allphins.com/api/v1/assets/`

### Query Arguments

Argument | Description
--------- | -----------
`name` or `q` | Free text searched in the asset's names, aliases and other text fields.
`country` | Comma-separated country codes or regions.
`company` | Comma-separated company names.
`class` | Comma-separated categories, types or subtypes.
`status` | Comma-separated statuses.
`ids` | Comma-separated asset IDs.
`page` | Page number, 30 assets per page.
`start`, `end` | Slice of the results to return. Default `0` and `30`.

## Retrieve an asset

```shell
curl https://api.allphins.com/api/v1/assets/3abba51f-e7fc-4af1-a65a-023d5b47745e/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "id": "3abba51f-e7fc-4af1-a65a-023d5b47745e",
  "location": {
    "locations": [
      {
        "type": "Feature",
        "geometry": {
          "type": "Point",
          "coordinates": [-151.902539, 59.860441]
        },
        "properties": {}
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
  "parent_name": null,
  "parent_id": null,
  "images": [],
  "linked_assets": [],
  "indicators": {
    "capex_estimate": null
  },
  "production": [],
  "identification_numbers": [],
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
  "operator": null,
  "general_comment": null,
  "capex_estimate": null,
  "latitude": "59.860441",
  "longitude": "-151.902539",
  "country_code": "US",
  "created_at": "2020-07-07T20:55:55.744133",
  "updated_at": "2020-12-21T10:47:55.258118",
  "other_info": null,
  "...": "..."
}
```

This endpoint retrieves a specific asset.

<aside class="warning">Every asset category has its own attributes, for example an <code>offshore_installation</code> has a <code>water_depth_meter</code> but an <code>onshore_installation</code> doesn't.</aside>

### HTTP Request

`GET https://api.allphins.com/api/v1/assets/:id/`

### URL Arguments

Argument | Description
--------- | -----------
`id` | The ID of the asset to retrieve

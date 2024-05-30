# Scenarios

## Scenario List Object Attributes

| Attribute          | Type                                                 | Description                                                                         |
|--------------------|------------------------------------------------------|-------------------------------------------------------------------------------------|
| `id`               | _UUID_                                               | Unique identifier for the object.                                                   |
| `name`             | _string_                                             | Name of the scenario.                                                               |
| `attributes`       | _list[str]_                                          | Accumulation keys for this scenario list, only if the scenario list is automatic.   |
| `config`           | _list[dict]_                                         | Configuration of the scenario list.                                                 |
|                    | _type: str<br/>condition: str<br/>attribute_id: int_ | Type of attribute.<br/>Condition of matching<br/>Attribute id (for manual scenario) |
| `description`      | _string_                                             | Description of the scenario list.                                                   |
| `manual`           | _bool_                                               | Is the scenario list manual or automatic.                                           |
| `is_featured`      | _bool_                                               | Makes it appear in the 'Featured' section                                           |
| `section`          | _string_                                             | Section of the scenario list                                                        |
| `line_of_business` | _string_                                             | Line of business of the scenario list.                                              |
| `is_favorite`      | _bool_                                               | Makes it appear in the 'Favorite' section                                           |
| `icon`             | _file_                                               | URL to the scenario list icon                                                       |

## Scenario Object Attributes

| Attribute       | Type                                                          | Description                                                                                                     |
|-----------------|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `id`            | _int_                                                         | Unique identifier for the object.                                                                               |
| `name`          | _string_                                                      | Name of the scenario.                                                                                           |
| `attributes`    | _list[dict]_                                                  | List of attributes for this scenario, this list is empty for custom scenarios.                                  |
|                 | _value: str<br/>label: str<br/>type: str<br/>type_label: str_ | Value of the attribute.<br/>Label of the attribute.<br/>Type of the attribute.<br/>Label of the attribute type. |
| `meta_data`     | _list[dict]_                                                  | List of metadata for this scenario.                                                                             |
|                 | _value: str<br/>label: str<br/>type: str<br/>type_label: str_ | Value of the meta-data.<br/>Label of the meta-data.<br/>Type of the meta-data.<br/>Label of the meta-data type. |
| `gross_loss`    | _int_                                                         | Value of the gross loss.                                                                                        |
| `retro`         | _int_                                                         | Value of the retro.                                                                                             |
| `reinstatement` | _int_                                                         | Value of the reinstatement.                                                                                     |

## Retrieve all scenarios list

```shell
curl https://api.allphins.com/api/v1/scenario_lists/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "config": [
        {
          "type": "lloyds_risk_code",
          "condition": "is",
          "attribute_id": null
        }
      ],
      "name": "Per Lloyds Risk Code",
      "description": null,
      "icon": null,
      "is_featured": false,
      "section": "Accumulation per Attribute",
      "manual": false,
      "attributes": [
        "lloyds_risk_code"
      ],
      "line_of_business": "cyber",
      "is_favorite": false
    }
  ]
}
```

### HTTP Request

`GET https://api.allphins.com/api/v1/scenario_lists/`

### URL Arguments

No argument

## Retrieve scenarios for a given scenario list

```shell
curl
  -d '{"policies": [xxxx, xxxx, xxxx], "computation_date": "2023-08-12"}'
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  https://api.allphins.com/api/v1/scenarios_list/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/compute/
```

> The above command returns JSON structured like this:
  
```json
{
  "count": 34576,
  "next": "https://api.allphins.com/api/v1/scenarios_list/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/compute/?page=2",
  "previous": null,
  "results": [
    {
      "id": 1,
      "name": "Johan Sverdrup Complex",
      "attributes": [
        {
          "value": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "label": "Johan Sverdrup Complex",
          "type": "asset_group",
          "type_label": "Asset Group"
        }
      ],
      "meta_data": [
        {
          "value": "NO",
          "label": "Norway",
          "type": "country",
          "type_label": "Country"
        },
        {
          "value": "[2.55203, 58.83641]",
          "label": "[2.55203, 58.83641]",
          "type": "location",
          "type_label": "Location"
        },
        {
          "value": "offshore_installation",
          "label": "Offshore Installation",
          "type": "asset_category",
          "type_label": "Asset Category"
        }
      ],
      "gross_loss": 315626504.18,
      "retro": 0,
      "reinstatement": 16346601.67,
      "timeline": []
    },
    {
      "id": 2,
      "name": "Valhall Complex",
      "attributes": [
        {
          "value": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "label": "Valhall Complex",
          "type": "asset_group",
          "type_label": "Asset Group"
        }
      ],
      "meta_data": [
        {
          "value": "NO",
          "label": "Norway",
          "type": "country",
          "type_label": "Country"
        },
        {
          "value": "[3.39475, 56.27715]",
          "label": "[3.39475, 56.27715]",
          "type": "location",
          "type_label": "Location"
        },
        {
          "value": "offshore_installation",
          "label": "Offshore Installation",
          "type": "asset_category",
          "type_label": "Asset Category"
        }
      ],
      "gross_loss": 304626504.58,
      "retro": 0,
      "reinstatement": 14286302.51,
      "timeline": []
    }
  ]
}
```

This endpoint retrieves all the scenarios of a given scenario list.

### HTTP Request

`POST https://api.allphins.com/api/v1/scenarios_list/:id/compute/`

### URL Arguments

No argument

### Payload

| Argument           | Type       | Required | Description                                                                     |
|--------------------|------------|----------|---------------------------------------------------------------------------------|
| `page`             | _integer_  | no       | Page number (default 1).                                                        |
| `pageSize`         | _integer_  | no       | Size of the page (default 30).                                                  |
| `computation_date` | _string_   | yes      | Date considered for the calculation of the aggregation (format : `YYYY-MM-DD`). |
| `policies`         | _list[id]_ | yes      | List of the policy ids to use for the computation.                              |

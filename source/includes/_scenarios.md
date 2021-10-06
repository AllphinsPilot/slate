# Scenarios

## Scenario List Object Attributes

| Attribute          | Type     | Description                                                                       |
| ------------------ | -------- | --------------------------------------------------------------------------------- |
| `id`               | _UUID_   | Unique identifier for the object.                                                 |
| `name`             | _string_ | Name of the scenario.                                                             |
| `attributes`       | _list_   | Accumulation keys for this scenario list, only if the scenario list is automatic. |
| `description`      | _string_ | Description of the scenario list.                                                 |
| `manual`           | _bool_   | Is the scenario list manual or automatic.                                         |
| `gross_loss`       | _float_  | Maximum possible loss for the scenario list on 'active' portfolios                |
| `is_featured`      | _bool_   | Does it appears in the 'Featured' section                                         |
| `sectios`          | _string_ | Section of the scenario list                                                      |
| `line_of_business` | _string_ | Line of business of the scenario list.                                            |
| `icon`             | _file_   | URL to the scenario list icon                                                     |

## Scenario Object Attributes

| Attribute          | Type     | Description                                                                    |
| ------------------ | -------- | ------------------------------------------------------------------------------ |
| `id`               | _int_    | Unique identifier for the object.                                              |
| `name`             | _string_ | Name of the scenario.                                                          |
| `attributes`       | _list_   | List of attributes for this scenario, this list is empty for custom scenarios. |
| `config`           | _list_   | Configuration file for a custom scenario.                                      |
| `line_of_business` | _string_ | Line of business of the scenario.                                              |
| `meta_data`        | _list_   | List of _extra_ attributes for the scenario.                                   |
| `s_lists`          | _list_   | List of scenario lists to which the scenario belongs to.                       |

## Retrieve all scenarios list

```shell
curl https://app.allphins.com/api/v1/key_lists/
  -H "Authorization: Token ENTERYOURAUTHTOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee",
      "name": "Scenario List Name",
      "description": null,
      "icon": null,
      "is_featured": false,
      "section": "Accumulation per Region",
      "gross_loss": 100000,
      "manual": false,
      "attributes": ["region"],
      "pml": null,
      "line_of_business": "trade"
    }
  ]
}
```

### HTTP Request

`GET https://app.allphins.com/api/v1/key_lists/`

### URL Arguments

No argument

## Retrieve scenarios for a given scenario list

```shell
curl
  -d '{portfolios: "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeee", key_list: "01c5a5ff-0085-491a-bd4b-cec2c0700ce5"}'
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Token ENTERYOURAUTHTOKEN"
  https://app.allphins.com/api/v1/clash_scenarios/scenarios_list/
```

```json
{
  "count": 132,
  "next": "http://localhost/api/v1/clash_scenarios/scenarios_list/?page=2",
  "previous": null,
  "results": [
    {
      "id": 146986,
      "name": "Israel",
      "gross_loss": null,
      "retro": 0,
      "reinstatement": 0,
      "scenario_type": null,
      "timeline": [],
      "attributes": [
        {
          "id": 24309,
          "label": "Israel",
          "value": "IL",
          "type": "country",
          "location": null
        }
      ],
      "meta_data": []
    }
  ]
}
```

This endpoint retrieves all the scenarios of a given scenario list, with the

### HTTP Request

`POST https://app.allphins.com/api/v1/clash_scenarios/scenarios_list/`

### URL Arguments

| Argument     | Description                                      |
| ------------ | ------------------------------------------------ |
| `key_list`   | The ID of the sceanrio list                      |
| `portfolios` | The IDs of the [`portolio`](#portolios) objects. |

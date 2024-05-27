# Countries

## Retrieve all countries

```shell
curl https://api.allphins.com/api/v1/countries/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
    "count": 200,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": "d9c3cd06-5aa3-4fa2-a7a6-d123b5ca3e6e",
            "name": "Angola/Congo Unitized Area",
            "country_code": "XX",
            "region": "Africa",
            "aliases": [
                "Angola/Congo Unitized Area",
            ]
        },
        {
            "id": "2ab08b2d-c8ba-4ad3-9f43-d02c0a850795",
            "name": "Antigua and Barbuda",
            "country_code": "AG",
            "region": "South/Latin America",
            "aliases": [
                "Antigua and Barbuda",
                "Antigua et Barbuda",
                "Antigua-et-Barbuda",
                "Antigua y Barbuda"
            ]
        }
    ]
}
```

This endpoint retrieves all countries.

### HTTP Request

`GET https://api.allphins.com/api/v1/countries/`

### Query Arguments

Argument | Default | Description
--------- | ------- | -----------
`q` | null | Free text for autocompletion.

<aside class="notice">
This query will return <b>ALL</b> countries, without pagination. Since the number of countries is limited this limit the number of API calls to retrieve all countries. 
</aside>

### The country object

#### Attributes

Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`name` | *string* | Prefered name for the country.
`country_code` | *string* | Country code.
`region` | *string* | Region of the country.
`aliases` | *list of string* | List of possible names for the country, includes acronyms.


## Retrieve a country

```shell
curl https://api.allphins.com/api/v1/countries/064f9fad-24bd-4091-a782-9bba124cdff2/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
    "id": "064f9fad-24bd-4091-a782-9bba124cdff2",
    "name": "Venezuela",
    "country_code": "VE",
    "region": "South/Latin America",
    "aliases": [
        "Bolivarian Republic of Venezuela",
        "Estados Unidos de Venezuela",
        "República Bolivariana de Venezuela",
        "República de Venezuela",
        "Republic of Venezuela",
        "United States of Venezuela",
        "Venezuela",
        "Venezuela",
        "Venezuela",
        "Vénézuéla"
    ]
}
```

This endpoint retrieves a specific country.

### HTTP Request

`GET https://api.allphins.com/api/v1/countries/:id/`

### URL Arguments

Argument | Description
--------- | -----------
`id` | The ID of the country to retrieve

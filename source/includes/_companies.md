# Companies

## Retrieve all companies

```shell
curl https://app.allphins.com/api/v1/companies/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "count": 785,
    "next": "http://35.197.208.182/api/v1/companies/?page=2",
    "previous": null,
    "results": [
        {
            "id": "bc687fa2-1781-418d-8316-8f2cec9a3759",
            "name": "ADNOC",
            "children": [
                {
                    "id": "6aeb44c1-a781-4dbd-b548-f06fc2dd36cb",
                    "name": "ADNOC Offshore",
                    "children": [],
                    "title": "ADNOC Offshore",
                    "value": "ADNOC Offshore"
                },
                {
                    "id": "1be87bfa-0780-47b7-a678-392dde9d5493",
                    "name": "ADNOC Onshore",
                    "children": [],
                    "title": "ADNOC Onshore",
                    "value": "ADNOC Onshore"
                },
                {
                    "id": "9360f805-3231-4921-9120-9db3ac926483",
                    "name": "Aj Dhafra Petroleum",
                    "children": [],
                    "title": "Aj Dhafra Petroleum",
                    "value": "Aj Dhafra Petroleum"
                }
            ],
            "title": "ADNOC",
            "value": "ADNOC"
        },
        {
            "id": "9a004215-a122-456c-9254-8ec4d644de21",
            "name": "Anadarko",
            "children": [
                {
                    "id": "8b69fed4-6bbd-406e-bae6-ec54247d517e",
                    "name": "Anadarko Petroleum Corporation",
                    "children": [],
                    "title": "Anadarko Petroleum Corporation",
                    "value": "Anadarko Petroleum Corporation"
                },
                {
                    "id": "d5cde54f-b171-473b-ab84-b7cfbd58e52e",
                    "name": "Kerr-McGee",
                    "children": [],
                    "title": "Kerr-McGee",
                    "value": "Kerr-McGee"
                },
                {
                    "id": "7cb0ba89-29cc-4c9d-abb3-f3d23aa2625c",
                    "name": "Kerr-McGee Ex Westport",
                    "children": [],
                    "title": "Kerr-McGee Ex Westport",
                    "value": "Kerr-McGee Ex Westport"
                },
                {
                    "id": "ec02f64a-8ff9-4798-a851-23e6f990c06c",
                    "name": "Kerr-McGee merged with Anadarko",
                    "children": [],
                    "title": "Kerr-McGee merged with Anadarko",
                    "value": "Kerr-McGee merged with Anadarko"
                },
                {
                    "id": "7f530534-4fbd-496d-a05f-e992027293fc",
                    "name": "Kerr McGee Oil & Gas Corp",
                    "children": [],
                    "title": "Kerr McGee Oil & Gas Corp",
                    "value": "Kerr McGee Oil & Gas Corp"
                },
                {
                    "id": "34257d75-215b-4d69-a981-827626d11167",
                    "name": "Offshore Shelf (Anadarko from Kerr-McGee)",
                    "children": [],
                    "title": "Offshore Shelf (Anadarko from Kerr-McGee)",
                    "value": "Offshore Shelf (Anadarko from Kerr-McGee)"
                }
            ],
            "title": "Anadarko",
            "value": "Anadarko"
        },
        {
            "id": "b81b7d13-c615-469a-99ce-d6b416ab6a65",
            "name": "Aqaba LNG",
            "children": [],
            "title": "Aqaba LNG",
            "value": "Aqaba LNG"
        }
    ]
}
```

This endpoint retrieves all companies.

### HTTP Request

`GET https://app.allphins.com/api/v1/companies/`

### Query Arguments

Argument | Default | Description
--------- | ------- | -----------
`q` | null | Free text for autocompletion.


### The company object

Company objects follow a hierachical organization, a company can have multiple parents.

**Attributes**

Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`stats` | *object* | Simple stats on the company assets.
`name` | *string* | Company name.
`aliases` | *list of string* | List of possible aliases for the company.
`parents` | *list of company objects* | List of parent companies, a company can have multiple parents (in the case of JV).
`nationality` | *string* | Nationality of the company.
`founding_year` | *integer* | Founding year of the company.
`wikipedia_url` | *string* | Url to the wikipedia page of the company.
`market_cap_m_dollars` | *float* | Market capitalization of the company in Million dollars.
`revenue_m_dollars` | *float* | Revenues in Million dollars.
`profit_margin_m_dollars` | *string* | Profit margin in Million dollars.
`logo` | *url* | Url to the logo of the company.
`company_type` | *string* | Type of company (privately held, national company etc..).


## Retrieve a company

```shell
curl https://app.allphins.com/api/v1/companies/bc687fa2-1781-418d-8316-8f2cec9a3759/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> The above command returns JSON structured like this:

```json
{
    "id": "bc687fa2-1781-418d-8316-8f2cec9a3759",
    "stats": {
        "assets": 0
    },
    "parent": [],
    "name": "ADNOC",
    "aliases": [
        "Abu Dhabi National Oil Company"
    ],
    "nationality": null,
    "founding_year": null,
    "wikipedia_url": null,
    "market_cap_m_dollars": null,
    "revenue_m_dollars": null,
    "profit_margin_m_dollars": null,
    "logo": null,
    "company_type": "National Oil Company"
}
```

This endpoint retrieves a specific company.

### HTTP Request

`GET https://app.allphins.com/api/v1/companies/:id/`

### URL Arguments

Argument | Description
--------- | -----------
`id` | The ID of the company to retrieve

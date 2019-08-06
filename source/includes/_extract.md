# Extract

## Entity Extraction

```shell
curl 
  -d '{"text":"Pelican FPSO in Gabon is redeployed."}'
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
  https://app.allphins.com/api/v1/countries/
```

> The above command returns JSON structured like this:

```json
{
    "terms": [
        {
            "id": "",
            "label": "pelican",
            "term_type": "geo",
            "parents": [],
            "synonyms": []
        },
        {
            "id": "1d6f75cd-69ab-4635-bd2c-2be60efd467d",
            "label": "fpso",
            "term_type": "structure",
            "parents": [
                "1f3764ef-b127-47dc-8ed8-46997ade1732",
                "21b18372-9ac6-4a2a-aefc-e49b075d9092",
                "71f45d8f-a70e-4bc4-a760-e6304f61983c",
                "81d6d1fe-1721-4eb1-a60d-4c71b87663d3"
            ],
            "synonyms": [
                "1d6f75cd-69ab-4635-bd2c-2be60efd467d",
                "833613bc-0827-4ea5-9cf7-83a4ca1bd8f1"
            ]
        },
        {
            "id": "",
            "label": "gabon",
            "term_type": "country",
            "parents": [],
            "synonyms": []
        },
        {
            "id": "4b955e9d-2c55-4a76-bb98-ef8fd2b78b0f",
            "label": "redeployed",
            "term_type": "status",
            "parents": [
                "40910956-c30c-46f7-bcd0-54746a9517a4"
            ],
            "synonyms": []
        }
    ],
    "text": "<div class=\"entities\" style=\"line-height: 2.5\">\n<mark class=\"entity\" style=\"background: #722ed1; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone\">\n    pelican\n    <span style=\"font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem\">GEO</span>\n</mark>\n \n<mark class=\"entity\" style=\"background: #fa541c; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone\">\n    fpso\n    <span style=\"font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem\">STRUCTURE</span>\n</mark>\n in \n<mark class=\"entity\" style=\"background: #52c41a; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone\">\n    gabon\n    <span style=\"font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem\">COUNTRY</span>\n</mark>\n is \n<mark class=\"entity\" style=\"background: #eb2f96; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone\">\n    redeployed\n    <span style=\"font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem\">STATUS</span>\n</mark>\n.</div>"
}
```

This endpoint extracts all the terms contained in a raw text. The terms are defined in our industrial ontology.

This service is used to extract information from text (like news or techical specifications).

<aside class="warning">
This service is build with Machine Learning models and Natural Language Processing (NLP), you may encounter classification errors, please report them to the customer support.
</aside>


### HTTP Request

`POST https://app.allphins.com/api/v1/extract/`

### Query Arguments

Argument | Default | Description
--------- | ------- | -----------
`text` | null | Raw text to extract entities from.


### Response Attributes

Attribute | Type | Description
--------- | ------- | -----------
`terms` | *list of objects* | List of terms objects (see below).
`text` | *string* | Raw HTML for entity detection visualization.

<aside class="notice">
    Entity Extraction visualization example from the <code>text</code> response attribute:
    <div class="entities" style="line-height: 2.5">
        <mark class="entity" style="background: #722ed1; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone">
            pelican
            <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem">
                GEO
            </span>
        </mark>
        <mark class="entity" style="background: #fa541c; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone">
            fpso
            <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem">
                STRUCTURE
            </span>
        </mark>
        &nbsp;in&nbsp;
        <mark class="entity" style="background: #52c41a; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone">
            gabon
            <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem">
                COUNTRY
            </span>
        </mark>
        &nbsp;is&nbsp;
        <mark class="entity" style="background: #eb2f96; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em; box-decoration-break: clone; -webkit-box-decoration-break: clone">
            redeployed
            <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; text-transform: uppercase; vertical-align: middle; margin-left: 0.5rem">
                STATUS
            </span>
        </mark>
        .
    </div>
</aside>


### Term object

Terms are identified words from our industrial ontology, a term is a word or a list of consecutives words (token or list of tokens) refering to a speficic object.

A term can be:

* A country
* A company
* A geo-spacial localisation (i.e. an oil field, a wind-farm location, a mining site..)
* An industry related term (specific to each industry) that can describe an asset (or a group of assets), for example:
    * Cardinal
    * Number
    * Letter
    * Insurance
    * Status
    * Material
    * Function
    * Function Acronym
    * Structure
    * Equipment
    * Effluent
    * Pression
    * Manufacturer
    * Policy
    * Other
    * Unknown


Attribute | Type | Description
--------- | ------- | -----------
`id` | *string* | Unique identifier for the object.
`label` | *string* | Label of the term (token or tokens).
`term_type` | *string* | Type of the term (see above).
`parents` | *list of string* | List of the parent's term ids.
`synonyms` | *list of string* | List of the synonyms's term ids.

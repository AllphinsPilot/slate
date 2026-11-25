# Scenario Risks Exports

A scenario risks export lists, in an Excel workbook, the risks of one portfolio that a scenario accumulates. With pre-inward steps, the workbook also breaks the loss of each EDM/CEDE policy down step by step: sites, sublimits, policy coverages, step 3 and gross. It is the same file as the app's "Export" of a scenario, with or without pre-inward steps, in the cedant aggregation view.

Exports are asynchronous, like [risk exports](#risk-exports): you ask for one, then download it once it is ready. Without pre-inward steps an export usually takes under a minute. With them it usually takes 4 to 7 minutes, and can take up to an hour.

Pre-inward steps are computed from EDM/CEDE or AIR data only. When a portfolio has none, or when their computation fails, the export is still produced without them: `breakdown_included` is then `false`, and the workbook's Parameters sheet says why.

## Scenario Risks Export Object Attributes

| Attribute            | Type      | Description                                                                                                       |
| -------------------- | --------- | ----------------------------------------------------------------------------------------------------------------- |
| `id`                 | _UUID_    | Unique identifier for the export. Use it to download the result.                                                   |
| `type`               | _string_  | Always `scenario_risks`.                                                                                           |
| `name`               | _string_  | Set by Allphins: `Scenario risks export [<scenario name>] [<date>]`.                                               |
| `config`             | _json_    | What the export was asked for. See below.                                                                          |
| `status`             | _string_  | `created`, `computing`, `ready` or `error`.                                                                        |
| `description`        | _string_  | Free text.                                                                                                         |
| `download_urls`      | _list_    | A signed URL to the workbook. Empty until `status` is `ready`. Valid for 7 days.                                   |
| `breakdown_included` | _boolean_ | `true` when the workbook has the pre-inward step sheets, `false` when they were asked for but could not be produced, `null` when they were not asked for. |

### Config Attributes

| Attribute           | Type      | Description                                                                                          |
| ------------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| `scenario_id`       | _int_     | Required. ID of a [`scenario`](#scenarios) of your active group.                                      |
| `portfolio_id`      | _UUID_    | Required. ID of a [`portfolio`](#portfolios) of your active group.                                    |
| `include_breakdown` | _boolean_ | Add the pre-inward step sheets. Defaults to `false`.                                                  |
| `computation_date`  | _date_    | `YYYY-MM-DD`. Keep only the risks live at this date. Omit to export every period.                     |

A scenario or portfolio that is not in your active group returns a `400`.

## Start a scenario risks export

```shell
curl https://api.allphins.com/api/v1/reports/
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  -d '{
        "type": "scenario_risks",
        "config": {
          "scenario_id": 20059058,
          "portfolio_id": "641f0542-81d9-40b3-96c5-95b6969bba4f",
          "include_breakdown": true
        }
      }'
```

> The above command returns JSON structured like this:

```json
{
  "id": "eec25dfa-e9ba-4057-83b9-238e7a2a3c9d",
  "name": "Scenario risks export [New Orleans - WAR] [2026-10-22]",
  "description": null,
  "config": {
    "scenario_id": 20059058,
    "portfolio_id": "641f0542-81d9-40b3-96c5-95b6969bba4f",
    "include_breakdown": true
  },
  "type": "scenario_risks",
  "status": "computing",
  "download_urls": [],
  "breakdown_included": null
}
```

The call returns immediately. Keep the `id`: it is the only thing you need to download the result later.

### HTTP Request

`POST https://api.allphins.com/api/v1/reports/`

### Body Arguments

| Argument | Description                                     |
| -------- | ----------------------------------------------- |
| `type`   | Must be `scenario_risks`.                       |
| `config` | What to export. See *Config Attributes* above.  |

## Retrieve a scenario risks export

```shell
curl https://api.allphins.com/api/v1/reports/eec25dfa-e9ba-4057-83b9-238e7a2a3c9d/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> Once it is ready (other fields unchanged):

```json
{
  "id": "eec25dfa-e9ba-4057-83b9-238e7a2a3c9d",
  "status": "ready",
  "download_urls": [
    "https://storage.googleapis.com/.../allphins_export_1446_terror_v2_scenariorisks_New_Orleans_-_WAR_2026-10-22_eec25dfa-e9ba-4057-83b9-238e7a2a3c9d.xlsx?X-Goog-Signature=..."
  ],
  "breakdown_included": true
}
```

Poll this endpoint until `status` is `ready`, backing off between calls: once a minute is plenty. A `status` of `error` means the export failed; start a new one, or contact support.

When you asked for pre-inward steps, check `breakdown_included` before reading the step sheets.

### HTTP Request

`GET https://api.allphins.com/api/v1/reports/:id/`

### URL Arguments

| Argument | Description                      |
| -------- | -------------------------------- |
| `id`     | The ID of the export to retrieve |

## Download the workbook

```shell
curl -o scenario_risks.xlsx "https://storage.googleapis.com/...?X-Goog-Signature=..."
```

The URL is signed and leads straight to cloud storage: no authentication header is needed on it. It expires 7 days after it was issued, and every call to `GET /reports/:id/` issues a fresh one.

The workbook has these sheets:

| Sheet                                                                    | Content                                                                                                                                |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `EDM CEDE Risks`, `Excel Risks`                                          | The risks of the scenario, one sheet per kind of data the portfolio holds. `Policy Id` is the EDM/CEDE policy ID, not an Allphins policy ID. |
| `Parameters`                                                             | Portfolio, scenario list, scenario, computation date, the scenario's filters and, when they were asked for, what became of the pre-inward steps: `Included`, `Not available: the portfolio has no processed EDM/AIR datasource`, `Not included: the computation failed` or `Not included: the computation returned no steps`. |
| `Sites Loss`, `Sublimit Loss`, `Policy Coverage Loss`, `Step 3 Loss`, `Gross Loss` | With pre-inward steps only. The loss after each step, per EDM/CEDE policy (`Policyid`, `Accgrpid`). |

# Risk Exports

A risk export extracts the underlying EDM/CEDE locations of a book as [Parquet](https://parquet.apache.org/) files.

Extracts are asynchronous: a filtered slice takes seconds, a whole book can take hours, so you ask for one, then download it once it is ready. Nothing is streamed through the API — the files are written to cloud storage and you fetch them directly, so the size of an extract is never limited by a request timeout.

Exports are only available for lines of business backed by EDM/CEDE data: Terror, Property and Onshore Energy. Asking for one on any other line of business returns a `400`.

## Export Object Attributes

| Attribute       | Type     | Description                                                                 |
| --------------- | -------- | --------------------------------------------------------------------------- |
| `id`            | _UUID_   | Unique identifier for the export. Use it to download the result.             |
| `type`          | _string_ | Always `edm_risk_export`.                                                    |
| `name`          | _string_ | Free text, shown in the app.                                                 |
| `config`        | _json_   | The filters the extract was asked for. See below.                            |
| `status`        | _string_ | `created`, `computing`, `ready` or `error`.                                  |
| `download_urls` | _list_   | One signed URL per Parquet file. Empty until `status` is `ready`.            |

### Config Attributes

Every filter is optional, and they combine: an extract narrowed by both a scenario and an asset returns the risks matching both.

| Attribute       | Type     | Description                                                                                          |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------- |
| `portfolio_ids` | _list_   | [`Portfolio`](#portfolios) IDs. Omit to extract every **active** portfolio — those carrying a written policy still in force. Naming portfolios explicitly returns them whatever their state, so quotes and future books stay reachable. |
| `scenario_id`   | _int_    | ID of a [`scenario`](#scenarios): keeps the risks it accumulates.                                     |
| `assets`        | _list_   | Asset labels. An unknown label fails the export rather than being ignored.                            |
| `asset_groups`  | _list_   | Asset group labels. Same rule.                                                                        |
| `columns`       | _list_   | Column names to keep. Omit for every column. Narrowing them makes an extract considerably smaller.     |

## Start an export

```shell
curl https://api.allphins.com/api/v1/reports/
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
  -d '{
        "name": "EDM risks export",
        "type": "edm_risk_export",
        "config": {
          "portfolio_ids": ["443cb85b-eb01-4c68-9c7b-b3edbfa17153"],
          "assets": ["Sleipner"]
        }
      }'
```

> The above command returns JSON structured like this:

```json
{
  "id": "0d929dd4-a399-4773-9042-267491296a99",
  "name": "EDM risks export",
  "type": "edm_risk_export",
  "config": {
    "portfolio_ids": ["443cb85b-eb01-4c68-9c7b-b3edbfa17153"],
    "assets": ["Sleipner"]
  },
  "status": "computing",
  "download_urls": []
}
```

The call returns immediately, whatever the size of the extract. Keep the `id`: it is the only thing you need to download the result later.

### HTTP Request

`POST https://api.allphins.com/api/v1/reports/`

### Body Arguments

| Argument | Description                                    |
| -------- | ---------------------------------------------- |
| `type`   | Must be `edm_risk_export`.                     |
| `name`   | Free text, shown in the app.                   |
| `config` | The filters. See *Config Attributes* above.    |

## Retrieve an export

```shell
curl https://api.allphins.com/api/v1/reports/0d929dd4-a399-4773-9042-267491296a99/
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> While the extract is still running:

```json
{
  "id": "0d929dd4-a399-4773-9042-267491296a99",
  "status": "computing",
  "download_urls": []
}
```

> Once it is ready:

```json
{
  "id": "0d929dd4-a399-4773-9042-267491296a99",
  "status": "ready",
  "download_urls": [
    "https://storage.googleapis.com/.../allphins_export_%5B1840%5D_%5Bterror_v2%5D_edmrisks_%5B2026-09-17%5D_%5B0d929dd4%5D-000000000000.parquet?X-Goog-Signature=...",
    "https://storage.googleapis.com/.../allphins_export_%5B1840%5D_%5Bterror_v2%5D_edmrisks_%5B2026-09-17%5D_%5B0d929dd4%5D-000000000001.parquet?X-Goog-Signature=..."
  ]
}
```

Poll this endpoint until `status` is `ready`. Back off between calls — an extract can run for hours, and there is nothing to gain from asking every second.

A `status` of `error` means the extract failed; the reason is in the notification in the app.

### HTTP Request

`GET https://api.allphins.com/api/v1/reports/:id/`

### URL Arguments

| Argument | Description                      |
| -------- | -------------------------------- |
| `id`     | The ID of the export to retrieve |

## Download the files

```shell
curl -o risks-000000000000.parquet "https://storage.googleapis.com/...?X-Goog-Signature=..."
```

The URLs are signed and lead straight to cloud storage — no authentication header is needed on them, and nothing goes through the API.

An extract is split into several files, which is how Parquet is normally written. Read the directory rather than the files one by one, and every reader will treat them as a single dataset:

```shell
python -c "import pandas as pd; print(pd.read_parquet('./data').shape)"
```

Each row is one location and one loss type, and carries the ID of the portfolio it belongs to along with every field of the underlying exposure file.

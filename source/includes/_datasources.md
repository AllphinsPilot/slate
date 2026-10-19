# Datasources

A datasource is a file uploaded to Allphins, such as an Excel risk list or an EDM/CEDE database, usually attached to a [portfolio](#portfolios). The API only returns the datasources of your active group, and never deleted ones.

## Datasource Object Attributes

| Attribute           | Type        | Description                                                                                          |
|---------------------|-------------|------------------------------------------------------------------------------------------------------|
| `id`                | _UUID_      | Unique identifier for the object.                                                                    |
| `original_filename` | _string_    | Name of the uploaded file.                                                                           |
| `status`            | _string_    | Processing status (see below).                                                                       |
| `nature`            | _string_    | What the file holds, for example `Risks`, `Policy`, `Structure`, `FX Rates`.                         |
| `extension`         | _string_    | File format, for example `Excel` or `SQL`.                                                           |
| `provider`          | _string_    | Source of the file, for example `EDM` or `Customer`.                                                 |
| `portfolio_id`      | _UUID_      | ID of the portfolio the datasource is attached to, or `null`.                                        |
| `user`              | _string_    | Full name of the user who uploaded the file.                                                         |
| `created_at`        | _timestamp_ | Upload date.                                                                                         |
| `updated_at`        | _timestamp_ | Last change to the datasource record, including status changes.                                      |
| `update_time`       | _timestamp_ | Date the data is current as of. Set when the file is mapped, to the date entered at mapping or else the mapping time. Before mapping, it is the upload date. |
| `metadata`          | _json_      | Metadata of the file, including `mapped_perils`.                                                     |
| `note`              | _string_    | Free note.                                                                                           |
| `error_message`     | _string_    | Error message when the processing failed.                                                            |
| `file_size`         | _int_       | File size in bytes.                                                                                  |
| `download_url`      | _string_    | Signed URL to download the original file, valid for 60 minutes.                                      |
| `actions`           | _json_      | Actions the app offers on the datasource in its current status.                                      |

**Only when retrieving one** (`GET /datasources/:id/` and `GET /portfolios/:id/datasources/`): `file`, `filename`, `config` (the mapping), `line_of_business`, `parent_id`.

**Only in the list** (`GET /datasources/`): `portfolio_name`, `portfolio_labels`, `mapped_perils`, `mapped_by` (user ID), `mapped_by_full_name` and `mapped_date` (when the file was last mapped). In the list, `extension` and `provider` are returned as codes, for example `excel` and `edm`.

### Statuses

| `status`      | Meaning                                                                       |
|---------------|-------------------------------------------------------------------------------|
| `Uploading`   | The file is being uploaded.                                                   |
| `Duplicating` | The file is being copied from another datasource.                             |
| `Scanning`    | The file is being analyzed.                                                   |
| `To Map`      | The file is analyzed and waits for its mapping.                               |
| `To Confirm`  | A structure file is analyzed and waits to be applied or cancelled.            |
| `Calculating` | The mapped data is being imported, enriched and computed.                     |
| `Ready`       | Processing is complete.                                                       |
| `Error`       | Processing failed. See `error_message`.                                       |
| `Superseded`  | The datasource was replaced by another one.                                   |

### Knowing when data is up to date

* A datasource's data is fully processed when its `status` is `Ready`.
* `update_time` tells you what date the data is current as of.
* On a portfolio, `risks_ready` (`GET /portfolios/:id/`) is `true` when every datasource is `Ready` or `Error`, and `data_update_time` (`GET /portfolios/`) is the most recent `update_time` among its datasources.

## Retrieve all datasources

```shell
curl "https://api.allphins.com/api/v1/datasources/?portfolio_id=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&status=Ready" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "meta": null,
  "results": [
    {
      "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
      "user": "Jane Doe",
      "original_filename": "Risk_list.xlsx",
      "portfolio_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "portfolio_name": "Cyber Client 1 2024",
      "portfolio_labels": [],
      "nature": "Risks",
      "extension": "excel",
      "provider": "customer",
      "mapped_perils": [],
      "actions": {},
      "created_at": "2024-05-20T07:52:55.491729Z",
      "updated_at": "2024-05-20T08:10:12.000000Z",
      "update_time": "2024-05-01T00:00:00Z",
      "mapped_by_full_name": "Jane Doe",
      "mapped_by": 42,
      "mapped_date": "2024-05-20T08:01:00Z",
      "metadata": {},
      "status": "Ready",
      "download_url": "https://storage.googleapis.com/...",
      "note": null,
      "error_message": null,
      "file_size": 102400
    }
  ]
}
```

This endpoint is paginated with `page` and `pageSize` (default 30, maximum 1000), see [Pagination](#pagination). Archives are left out unless you filter on `nature`.

### HTTP Request

`GET https://api.allphins.com/api/v1/datasources/`

### Query Parameters

| Parameter                        | Description                                                                            |
|----------------------------------|----------------------------------------------------------------------------------------|
| `portfolio_id`                   | Portfolio ID. `portfolio_id__in` takes a comma-separated list.                         |
| `status`                         | One status label, for example `Ready`.                                                 |
| `nature`                         | Nature code, for example `risks`. `nature__in` takes a comma-separated list.           |
| `provider`                       | Provider code, for example `edm`. `provider__in` takes a comma-separated list.         |
| `created_at__gte`, `created_at__lte`   | Upload date range.                                                               |
| `mapped_date__gte`, `mapped_date__lte` | Mapping date range.                                                              |
| `original_filename__icontains`   | Part of the file name.                                                                 |
| `ordering`                       | `created_at` (default `-created_at`), `file_size` or `original_filename`. Prefix with `-` for descending. |
| `page`, `pageSize`               | See [Pagination](#pagination).                                                         |

## Retrieve a datasource

```shell
curl https://api.allphins.com/api/v1/datasources/yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy/ \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This endpoint retrieves one datasource. To get every datasource of a portfolio in one call, without pagination, use [`GET /portfolios/:id/datasources/`](#retrieve-the-datasources-of-a-portfolio).

### HTTP Request

`GET https://api.allphins.com/api/v1/datasources/:id/`

### URL Arguments

| Argument | Description                          |
| -------- | ------------------------------------ |
| `id`     | The ID of the datasource to retrieve |

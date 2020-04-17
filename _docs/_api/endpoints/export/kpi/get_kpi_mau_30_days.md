---
nav_title: "GET: Monthly Active Users for Last 30 Days"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool: Segments
description: "This article outlines details about the Monthly Active Users Endpoint."
---
{% api %}
# Monthly Active Users Endpoint
{% apimethod get %}
/kpi/mau/data_series
{% endapimethod %}

This endpoint allows you to retrieve a daily series of the total number of unique active users over a 30-day rolling window.

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Export/Kpi%20export%20%20mau%20example {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#68f45461-3bf1-425c-b918-f0bbf3f87149 {% endapiref %}

## Parameters

| Parameter| Required | Data Type | Description |
| -------- | -------- | --------- | ----------- |
| `api_key`   | Yes | String | App Group REST API Key |
| `length`    | Yes | Integer | Max number of days before ending_at to include in the returned series - must be between 1 and 100 inclusive |
| `ending_at` | No | DateTime (ISO 8601 string) | Point in time when the data series should end - defaults to time of the request |
| `app_id`    | No | String | App API Identifier; if excluded, results for all apps in app group will be returned |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Example URL
`https://rest.iad-01.braze.com/kpi/mau/data_series?api_key=75480f9a-4db8-4057-8b7e-4d59bfd73709&length=7&ending_at=2014-12-10T23:59:59-05:00`

## Response

`Content-Type: application/json`

```json
Content-Type: application/json
{
    "message": (required, string) the status of the export, returns 'success' when completed without errors,
    "data" : [
        {
            "time" : (string) date as ISO 8601 date,
            "mau" : (int)
        },
        ...
    ]
}
```

{% endapi %}

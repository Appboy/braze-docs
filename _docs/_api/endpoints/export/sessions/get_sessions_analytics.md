---
nav_title: "GET: App Sessions by Time"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool: Segments
description: "This article outlines details about the sessions analytics endpoint."
---
{% api %}
# Session Analytics Endpoint
{% apimethod get %}
/sessions/data_series
{% endapimethod %}

This endpoint allows you to retrieve a series of the number of sessions for your app over a designated time period.

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Export/Session%20analytics%20export%20%20session%20series%20example {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#79efb6a9-62ec-4b8a-bf4a-e96313aa4be1 {% endapiref %}

## Parameters

| Parameter| Required | Data Type | Description |
| -------- | -------- | --------- | ----------- |
| `api_key`    | Yes | String | App Group REST API Key. |
| `length`     | Yes | Integer | Max number of units (days or hours) before ending_at to include in the returned series - must be between 1 and 100 inclusive. |
| `unit`       | No | String | Unit of time between data points - can be "day" or "hour" (defaults to "day"). |
| `ending_at`  | No | DateTime (ISO 8601 string) | Point in time when the data series should end - defaults to time of the request. |
| `app_id`     | No | String | App API Identifier retrieved from the Developer Console to limit analytics to a specific app. |
| `segment_id` | No | String | Segment API Identifier indicating the analytics enabled segment for which sessions should be returned. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Example URL
`https://rest.iad-01.braze.com/sessions/data_series?api_key=75480f9a-4db8-4057-8b7e-4d59bfd73709&length=24&unit=hour&ending_at=2014-12-10T23:59:59-05:00&app_id=3bbc4555-8fa0-4c9b-a5c0-4505edf3e064`

## Response

`Content-Type: application/json`

```json
Content-Type: application/json
{
    "message": (required, string) the status of the export, returns 'success' when completed without errors,
    "data" : [
        {
            "time" : (string) point in time - as ISO 8601 extended when unit is "hour" and as ISO 8601 date when unit is "day",
            "sessions" : (int)
        },
        ...
    ]
}
```

{% endapi %}

---
nav_title: "GET: Canvas Data Series Analytics"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool: Canvas
description: "This article outlines details about the Canvas Data Series Analytics Endpoint."
---
{% api %}
# Canvas Data Series Analytics Endpoint
{% apimethod get %}
/canvas/data_series
{% endapimethod %}

This endpoint allows you to export time series data for a Canvas.

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Export/Canvas%20export%20%20data%20series%20example {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#0fd61e93-7edf-4d87-a8dc-052420aefb73 {% endapiref %}

## Parameters

| Parameter| Required | Data Type | Description |
| -------- | -------- | --------- | ----------- |
| `api_key` | Yes | String | App Group REST API Key |
| `canvas_id` | Yes | String | Canvas API Identifier |
| `ending_at` | Yes | DateTime (ISO 8601 string) | Date on which the data export should end - defaults to time of the request |
| `starting_at` | No | DateTime (ISO 8601 string) | Date on which the data export should begin (either length or starting_at are required) |
| `length` | No | String | Max number of days before ending_at to include in the returned series - must be between 1 and 14 inclusive (either length or starting_at required) |
| `include_variant_breakdown` | No | Boolean | Whether or not to include variant stats (defaults to false) |
| `include_step_breakdown` | No | Boolean | Whether or not to include step stats (defaults to false) |
| `include_deleted_step_data` | No | Boolean | Whether or not to include step stats for deleted steps (defaults to false) |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Response

`Content-Type: application/json`

```json
{
  "data": {
    "name": (string) Canvas name,
    "stats": [
      {
        "time": (string) date as ISO 8601 date,
        "total_stats": {
          "revenue": (float),
          "conversions": (int),
          "conversions_by_entry_time": (int),
          "entries": (int)
        },
        "variant_stats": (optional) {
          "00000000-0000-0000-0000-0000000000000": (API identifier for variant) {
            "name": (string) name of variant,
            "revenue": (int),
            "conversions": (int),
            "conversions_by_entry_time": (int),
            "entries": (int)
          },
          ... (more variants)
        },
        "step_stats": (optional) {
          "00000000-0000-0000-0000-0000000000000": (API identifier for step) {
            "name": (string) name of step,
            "revenue": (float),
            "conversions": (int),
            "conversions_by_entry_time": (int),
            "messages": {
              "email": [
                {
                  "sent": (int),
                  "opens": (int),
                  "unique_opens": (int),
                  "clicks": (int),
                  ... (more stats)
                }
              ],
              ... (more channels)
            }
          },
          ... (more steps)
        }
      },
      ... (more stats by time)
    ]
  },
  "message": (required, string) the status of the export, returns 'success' when completed without errors
}
```

{% endapi %}

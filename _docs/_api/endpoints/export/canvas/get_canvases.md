---
nav_title: "GET: Canvas List"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool: Canvas
description: "This article outlines details about the Canvas List endpoint ."
---
{% api %}
# Canvas List Endpoint
{% apimethod get %}
/canvas/list
{% endapimethod %}

This endpoint allows you to export a list of Canvases, including the name, Canvas API Identifier and associated Tags. The Canvases are returned in groups of 100 sorted by time of creation (oldest to newest by default).

Archived Canvases will not be included in the API response unless the `include_archived` field is specified. Canvases that are stopped but not archived, however, will be returned by default.

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Export/Canvas%20export%20%20list%20example {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#e6c150d7-fceb-4b10-91e2-a9ca4d5806d1 {% endapiref %}

## Parameters

| Parameter | Required | Data Type | Description |
| --------- | -------- | --------- | ----------- |
| `api_key` | Yes | String    | App Group REST API Key |
| `page` | No | Integer   | The page of Canvases to return, defaults to `0` (returns the first set of up to 100) |
| `include_archived` | No | Boolean | Whether or not to include archived Canvases, defaults to `false`. |
| `sort_direction`   | No | String | Pass in the value `desc` to sort by creation time from newest to oldest. Pass in `asc` to sort from oldest to newest. If sort_direction is not included, the default order is oldest to newest. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Response

`Content-Type: application/json`

```json
{
  "canvases" : [
  	{
  		"id" : (string) Canvas API Identifier,
  		"name" : (string) Canvas name,
  		"tags" : (array) tag names associated with the Canvas,
  	},
    ... (more Canvases)
  ],
  "message": (required, string) the status of the export, returns 'success' when completed without errors
}
```

{% endapi %}

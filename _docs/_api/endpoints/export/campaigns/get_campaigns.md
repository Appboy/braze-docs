---
nav_title: "GET: Campaigns List"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool: Segments
description: "This article outlines details about a specified campaign."
---
{% api %}
# Campaigns List Endpoint
{% apimethod get %}
/campaigns/list
{% endapimethod %}

This endpoint allows you to export a list of campaigns, each of which will include its name, Campaign API Identifier, whether it is an API Campaign, and Tags associated with the campaign. The campaigns are returned in groups of 100 sorted by time of creation (oldest to newest by default).

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Export/Campaign%20export%20%20list%20example {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#f3b0b3ef-04fb-4a31-8570-e6ad88dacb18 {% endapiref %}

## Request Parameter Details

| Parameter | Required | Data Type | Description |
| --------- | -------- | --------- | ----------- |
| `api_key` | Yes | String | App Group REST API Key |
| `page` | No | Integer   | The page of campaigns to return, defaults to 0 (returns the first set of up to 100) |
| `include_archived` | No | Boolean | Whether or not to include archived campaigns, defaults to false |
| `sort_direction` | No | String | Pass in the value `desc` to sort by creation time from newest to oldest. Pass in `asc` to sort from oldest to newest. If sort_direction is not included, the default order is oldest to newest. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Example URL
`https://rest.iad-01.braze.com/campaigns/list?api_key=75480f9a-4db8-4057-8b7e-4d59bfd73709&page=1&include_archived=true`

## Campaign List Endpoint API Response

```json
Content-Type: application/json
{
    "message": (required, string) the status of the export, returns 'success' when completed without errors,
    "campaigns" : [
        {
            "id" : (string) Campaign API Identifier,
            "name" : (string) campaign name,
            "is_api_campaign" : (boolean) whether the campaign is an API Campaign,
            "tags" : (array) tag names associated with the campaign
        },
        ...
    ]
}
```


{% endapi %}

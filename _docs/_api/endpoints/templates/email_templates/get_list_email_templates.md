---
nav_title: "GET: List Available Email Templates"
page_order: 4

layout: api_page

page_type: reference
platform: API
channel:
  - Email
tool:
  - Canvas
  - Campaigns

description: "This article outlines details about the List Available Email Templates Braze endpoint."
---
{% api %}
# List Available Email Templates
{% apimethod get %}
/templates/email/list
{% endapimethod %}

Use this endpoint to get a list of available templates in your Braze account.

Use the Template REST APIs to programmatically manage the email templates that you have stored on the Braze dashboard, on the Templates & Media page. Braze provides two endpoints for creating and updating your email templates.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#eec24bf4-a3f4-47cb-b4d8-bb8f03964cca {% endapiref %}

## Request Parameters

| Parameter | Required | Data Type | Description |
|---|---|---|---|
| `modified_after`  | Optional | String in [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) format | Retrieve only templates updated at or after the given time. |
| `modified_before`  |  Optional | String in [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) format | Retrieve only templates updated at or before the given time. |
| `limit` | Optional | Positive Number | Maximum number of templates to retrieve. Default to 100 if not provided, with a maximum acceptable value of 1000. |
| `offset`  |  Optional | Positive Number | Number of templates to skip before returning rest of the templates that fit the search criteria. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Example Request
```
curl --location --request GET 'https://rest.iad-01.braze.com/templates/email/list?modified_after=2020-01-01T01:01:01.000000&modified_before=2020-02-01T01:01:01.000000&limit=1&offset=0' \
--header 'Authorization: Bearer YOUR-REST-API-KEY'
```

## Response 

```json
Content-Type: application/json
Authorization: Bearer YOUR-REST-API-KEY
{
  "count": number of templates returned
  "templates": [template with the following properties]:
    "email_template_id": (string) your email template's API Identifier,
    "template_name": (string) the name of your email template,
    "created_at": (string, in ISO 8601),
    "updated_at": (string, in ISO 8601),
    "tags": (array of strings) tags appended to the template
}
```
{% endapi %}

{% alert important %}
Templates built using the Drag & Drop Editor are not provided in this response.
{% endalert %}

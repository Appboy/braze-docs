---
nav_title: "POST: Remove Email Addresses from Spam List"
page_order: 4

layout: api_page

page_type: reference
platform: API
channel: Email
tool:
  - Canvas
  - Campaigns

description: "This article outlines details about and using the Remove Email Addresses from the Spam List Braze endpoint."
---
{% api %}
# Remove Email Addresses from Spam List
{% apimethod post %}
/email/spam/remove
{% endapimethod %}

This endpoint allows you to remove email addresses from your Braze spam list. We will also remove them from the spam list maintained by your email provider.

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Email%20Sync/RemovingSpamListEmailExample {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#1614a82f-510a-4c37-95a6-8207a125e487 {% endapiref %}

## Request Body

`Content-Type: application/json`

```json
{
  "api_key": "{{api_key}}",
  "email": "example@123.com"
}
```

## Parameters

| Parameter | Required | Data Type | Description |
| ---------------------| --------------- |
| `api_key` | Yes | String | See App Group REST API Key in Parameter Definitions. |
| `email` | Yes | String or Array | String email address to modify, or an Array of up to 50 email addresses to modify. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Example Unsubscribe CURL

The following example CURL demonstrates how to unsubscribe a user from receiving email via the Braze APIs:

```
curl -X POST -H "Content-Type: application/json" -d '{"api_key":"YOUR_APP_GROUP_REST_API_KEY","email":"EMAIL_TO_UNSUBSCRIBE","subscription_state":"unsubscribed"}' https://rest.iad-01.braze.com/email/status
```
{% endapi %}

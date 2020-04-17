---
nav_title: "POST: Update User's Subscription Group Status"
page_order: 4

layout: api_page

page_type: reference
platform: API
channel:
  - Email
  - SMS
tool:
  - Canvas
  - Campaigns

description: "This article outlines details about the Update User's Subscription Group Status Braze endpoint."
---
{% api %}
# Update Users' Subscription Group Status
{% apimethod post %}
/subscription/status/set
{% endapimethod %}

Use the endpoints below to update the subscription state of a user on the Braze dashboard. You can access a subscription groups `subscription_group_id` by navigating to it on the Subscription Group page.

If you want to see examples or test this endpoint for __Email Subscription Groups__:

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Subscription%20Groups/SetUsersSubscriptionStatus {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#8895e87e-6324-47a3-a833-adf29a258bb9 {% endapiref %}

If you want to see examples or test this endpoint for __SMS Subscription Groups__:

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Subscription%20Groups/SetUsersSubscriptionStatus {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#72558b32-7dbe-4cba-bd22-a7ce513076dd {% endapiref %}

## Request Body

```
Content-Type: application/json
```

```json
{
   "api_key": (required, string) your App Group REST API Key,
   "subscription_group_id": (required, string) the id of your subscription group,
   "subscription_state": (required, string) available values are “unsubscribed” (not in subscription group) or “subscribed” (in subscription group),
   "external_id": (required*, string) the external_id of the user,
   "email": (required*, string) the email address of the user
   //one of external_id or email is required
   //can be passed as an array of string with a max of 50
   //endpoint only accepts email or phone value, not both
   "phone": (optional, string in E.164 format) The phone number of the user (must include at least one phone number and at most 50 phone numbers).
 }
```
\* SMS subscription groups: Only `external_id` or `phone` is accepted.<br>
\* Email subscription groups: Either `email` or `external_id` is required. 

### Example Requests

#### Using Email
```json
{
  "api_key": "12345",
  "subscription_group_id": "111-222-333",
  "subscription_state": "unsubscribed",
  "email": "john@braze.com"
  }
```

#### Using Phone Number
```json
{
  "api_key": "12345",
  "subscription_group_id": "111-222-333",
  "subscription_state": "unsubscribed",
  "phone": "+14152342671"
  }
```

### Example Successful Response

Response: (status 201)

```json
{
    "message": "success"
}
```

{% alert important %}
The endpoint only accepts the `email` or `phone` value, not both. If given both, you will receive this response: `{"message":"Either an email address or a phone number should be provided, but not both."}`
{% endalert %}

{% endapi %}

[support]: {{site.baseurl}}/support_contact/

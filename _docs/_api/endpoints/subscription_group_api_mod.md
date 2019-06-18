---
nav_title: Subscription Groups
page_order: 4
---

# Subscription Group REST APIs

Use the Subscription Group REST APIs to programmatically manage the subscription groups that you have stored on the Braze dashboard, on the Subscription Group page.

## Get Users' Subscription Groups

Use the endpoints below to list and get the subscription groups of a certain user.

Instance  | REST Endpoint
----------|------------------------------------------------
US-01 | `https://rest.iad-01.braze.com/subscription/user/status`
US-02 | `https://rest.iad-02.braze.com/subscription/user/status`
US-03 | `https://rest.iad-03.braze.com/subscription/user/status`
US-04 | `https://rest.iad-04.braze.com/subscription/user/status`
US-06 | `https://rest.iad-06.braze.com/subscription/user/status`
EU-01 | `https://rest.fra-01.braze.eu/subscription/user/status`


```yaml
GET https://YOUR_REST_API_URL/subscription/user/status

endpoint: get user subscription groups
endpoint_url: /subscription/user/status
method: get
description: use to list and get the subscription groups of a certain user.

parameters:
  api_key: (required, string) your App Group REST API Key.
  external_id: (required, string) external_id of the user.
  email: (required, string) email address of the user (must include at least one address and at most 50 addresses).
  limit: (integer) the limit on the maximum number of results returned. Default (and max) limit is 100.
  offset: (integer) number of templates to skip before returning rest of the templates that fit the search criteria.

request_body_example: https://rest.iad-03.braze.com/subscription/user/status?api_key=23abc-def5-3729-owod-23f9f3j30&email=example%2B1%40braze.com&subscription_group_id=14386d4a-60dd-42e2-9c94-5f2423b91d9f
response_body_parameters:
link_to_swagger: https://www.braze.com/docs/api/interactive/#/operations/Subscription%20Groups/GetUsersSubscriptionGroups
errors:
glossary_tags: email, get_method
```

| Parameter | Required | Data Type | Description |
|---|---|---|---|
| `api_key`  | Yes | String | Your App Group REST API Key. |
| `external_id`  | Yes | String | The external_id of the user. |
| `email`  |  Yes | String | The email address of the user (must include at least one address and at most 50 addresses). |
| `limit` | No | Integer | The limit on the maximum number of results returned. Default (and max) limit is 100. |
| `offset`  |  No | Integer | Number of templates to skip before returning rest of the templates that fit the search criteria. |

{% alert tip %}
If there are multiple users (multiple external ids) who share the same email address, all users will be returned as a separate user (even if they have the same email address or subscription group).
{% endalert %}

### Example Request

```
https://rest.iad-03.braze.com/subscription/user/status?api_key=23abc-def5-3729-owod-23f9f3j30&email=example%2B1%40braze.com&subscription_group_id=14386d4a-60dd-42e2-9c94-5f2423b91d9f
```

## Getting the Subscription Status

Use the endpoints below to get the subscription state of a user in a subscription group. These groups will be available on the __Subscription Group__ page. The response from this endpoint will include a 0 (subscribed), 1 (unsubscribed), or null value for the specific subscription group requested in the API call.  This can be used to update the subscription group state in subsequent API calls or to be displayed on a hosted web page.

Instance  | REST Endpoint
----------|------------------------------------------------
US-01 | `https://rest.iad-01.braze.com/subscription/status/get`
US-02 | `https://rest.iad-02.braze.com/subscription/status/get`
US-03 | `https://rest.iad-03.braze.com/subscription/status/get`
US-04 | `https://rest.iad-04.braze.com/subscription/status/get`
US-06 | `https://rest.iad-06.braze.com/subscription/status/get`
EU-01 | `https://rest.fra-01.braze.eu/subscription/status/get`

```yaml
GET https://YOUR_REST_API_URL/subscription/status/get

endpoint: get subscription status
endpoint_url: /subscription/status/get
method: get
description: use to get the subscription state of a user in a subscription group. 

parameters:
  api_key: your App Group REST API Key.
  subscription_group_id: the id of your subscription group.
  external_id: the external_id of the user.
  email: (required, string) the email address of the user. Can be passed as an array of string with a max of 100.

request_body_example: https://rest.iad-03.braze.com/subscription/user/status?api_key=23abc-def5-3729-owod-23f9f3j30&email=example%2B1%40braze.com

response_body_parameters:
link_to_swagger: https://www.braze.com/docs/api/interactive/#/operations/Subscription%20Groups/GetUsersSubscriptionStatus
errors:
glossary_tags: email, user_data, get_method

```
| Parameter | Required | Data Type | Description |
|---|---|---|---|
| `api_key`  | Yes | String | Your App Group REST API Key. |
| `subscription_group_id`  | Yes | String | The `id` of your subscription group. |
| `external_id`  |  Yes* | String | The `external_id` of the user. |
| `email` | Yes* | String | The email address of the user. Can be passed as an array of string with a max of 100. |

_/* Either `external_id` or `email` are required._

### Example Request

```
https://rest.iad-03.braze.com/subscription/user/status?api_key=23abc-def5-3729-owod-23f9f3j30&email=example%2B1%40braze.com
```

## Updating a Subscription State

Use the endpoints below to update the subscription state of a user on the Braze dashboard. You can access a subscription groups `subscription_group_id` by navigating to it on the Subscription Group page.

Instance  | REST Endpoint
----------|------------------------------------------------
US-01 | `https://rest.iad-01.braze.com/subscription/status/set`
US-02 | `https://rest.iad-02.braze.com/subscription/status/set`
US-03 | `https://rest.iad-03.braze.com/subscription/status/set`
US-04 | `https://rest.iad-04.braze.com/subscription/status/set`
US-06 | `https://rest.iad-06.braze.com/subscription/status/set`
EU-01 | `https://rest.fra-01.braze.eu/subscription/status/set`

```yaml
POST https://YOUR_REST_API_URL/subscription/status/set

endpoint: update subscription state
endpoint_url: /subscription/status/set
method: post
description: use to update the subscription state of a user on the Braze dashboard.
response_details:

query_parameters:
  api_key: (required, string) your App Group REST API Key,
  subscription_group_id: (required, string) the id of your subscription group,
  subscription_state: (required, string) available values are “unsubscribed” (not in subscription group) or “subscribed” (in subscription group),
  external_id: (required*, string) the external_id of the user,
  email: (required*, string) the email address of the user
  //one of eternal_id or email is required
  //can be passed as an array of string with a max of 100

query_example: https://rest.iad-01.braze.com/subscription/status/set?api_key=23abc-def5-3729-owod-23f9f3j30&email=example%2B1%40braze.com

response_body_parameters:
link_to_swagger: https://www.braze.com/docs/api/interactive/#/operations/Subscription%20Groups/SetUsersSubscriptionStatus
errors:
glossary_tags: email, user_data, post_method

```

_Example of API call_

```json
POST https://YOUR_REST_API_URL/subscription/status/set
Content-Type: application/json
{
  Request:
  POST https://YOUR_REST_API_URL/subscription/status/set
  Body:
  {
    "api_key": "12345",
    "subscription_group_id": "111-222-333",
    "subscription_state": "unsubscribed",
    "email": "john@braze.com"
  }
```

```json
Response: (status 201)
{
    "message": "success"
}
```


[support]: {{ site.baseurl }}/support_contact/

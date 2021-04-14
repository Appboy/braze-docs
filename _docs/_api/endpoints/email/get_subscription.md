---
nav_title: "GET: Subscription information based on email address or phone number"


layout: api_page

page_type: reference
platform: API
channel: Email
tool:
  - Canvas
  - Campaigns

description: "This article outlines the usage of and parameters for using the retrieve the subscription state associated with an email address or phone number."
---
{% api %}
# Query based on email address or phone number
{% apimethod get %}
/users/subscription
{% endapimethod %}

This endpoint allows you to pull the subscription state value based on an email address or phone number.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#7c2ef84f-ddf5-451a-a72c-beeabc06ad9d {% endapiref %}

{% alert important %}
__Looking for the `api_key` parameter?__<br>As of May 2020, Braze has changed how we read API keys to be more secure. Now API keys must be passed as a request header, please see `YOUR_REST_API_KEY` within the __Example Request__ below.<br><br>Braze will continue to support the `api_key` being passed through the request body and URL parameters, but will eventually be sunset. Please update your API calls accordingly.
{% endalert %}

## Request Parameters



| Parameter | Required | Data Type | Description |
| ----------|-----------| ----------|----- |
| `email` | Yes * | String | The email address of the user (must include at least one address and at most 50 addresses). |
| `phone` | Yes * | String | The phone number of the user (must include at least one phone number and at most 50 phone numbers). The recommendation is to provide this in the E.164 format. |

{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}


## Example Request
```
curl --location --request GET 'https://rest.iad-01.braze.com/users/subscriptions?phone=+12123355555&email=example%40braze.com' \
--header 'Authorization: Bearer YOUR_REST_API_KEY'
```

### Sample Response

Entries are listed in descending order.

```json
Content-Type: application/json
Authorization: Bearer YOUR_REST_API_KEY
{
	"emails": [
    {
      "email": "example@braze.com",
      "email_subscribe": {
        "email_subscription_event_date": "2019-11-20T19:58:04.825Z",
        "email_subscription_state": "Subscribed"
      },
      "subscription_group": [
        {
          "subscription_group_id": "5f5536d2a76e0f4e323a1234",
          "subscription_group_event_date": "2021-03-11T21:29:22.347Z",
          "subscription_group_state": "Unsubscribed"
        }
      ],
      "hard_bounced_at": null,
      "spam_at": null
    }
  ],


	"phone": [{
		"phone": "+12123355555",
		"subscription_group": [{
			"subscription_group_id": "3f5536d2a76e0f4e323a5555",
			"subscription_group_state": "Subsscribed",
			"subscription_group_event_date": "2021-03-11T21:29:22.347Z"
		}]
	}],
	"message": "success"
}

```
{% endapi %}

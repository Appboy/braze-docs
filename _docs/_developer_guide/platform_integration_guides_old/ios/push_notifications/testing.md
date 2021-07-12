---
nav_title: Testing
platform: iOS
page_order: 5.1
description: "This article covers command line push testing for your iOS push notifications."
channel:
  - push

---

# Testing {#push-testing}

If you'd like to test in-app and push notifications via the command-line, you can send a single notification through the terminal via CURL and the [Messaging API][29]. You will need to replace the following fields with the correct values for your test case:

- `YOUR_API_KEY` — available on the [Developer Console][30] page
- `YOUR_EXTERNAL_USER_ID` — available on the [User Profile Search Page][31]. See documentation on [assigning user IDs][32]
- `YOUR_KEY1` (optional)
- `YOUR_VALUE1` (optional)

```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer {{YOUR_API_KEY}}" -d "{\"external_user_ids\":[\"YOUR_EXTERNAL_USER_ID\"],\"messages\":{\"apple_push\":{\"alert\":\"Test push\",\"extra\":{\"YOUR_KEY1\":\"YOUR_VALUE1\"}}}}" https://rest.iad-01.braze.com/messages/send
```
>  The above is an example for customers on the `US-01` instance. If you are not on this instance please refer to our [API documentation][66] to see which endpoint to make requests to.

[29]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[30]: https://dashboard-01.braze.com/app_settings/api_settings/
[31]: https://dashboard-01.braze.com/users/user_search/user-search/
[32]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/setting_user_ids/#assigning-a-user-id
[66]: {{site.baseurl}}/developer_guide/rest_api/messaging/#sending-messages-immediately-via-api-only

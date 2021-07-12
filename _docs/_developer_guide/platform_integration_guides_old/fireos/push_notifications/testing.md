---
nav_title: Testing
platform: FireOS
page_order: 1

page_type: reference
description: "This page provides information on testing in-app messages and push notificatsion via the command line."
channel: 
- push
- in-app messages

---

# Testing From Command Line

If you'd like to test in-app and push notifications via the command-line, you can send a single notification through the terminal via cURL and the [Messaging API][13]. You will need to replace the following fields with the correct values for your test case:

- `YOUR_API_KEY` - available on the [Developer Console][14] page
- `YOUR_EXTERNAL_USER_ID` - available on the [User Profile Search Page][15]
- `YOUR_KEY1` (optional)
- `YOUR_VALUE1` (optional)

```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer {{YOUR_API_KEY}}" -d "{\"external_user_ids\":[\"YOUR_EXTERNAL_USER_ID\"],\"messages\":{\"kindle_push\":{\"title\":\"Test push title\",\"alert\":\"Test push\",\"extra\":{\"YOUR_KEY1\":\"YOUR_VALUE1\"}}}}" https://rest.iad-01.braze.com/messages/send
```
>  The above is an example for customers on the `US-01` instance. If you are not on this instance please refer to our [API documentation][66] to see which endpoint to make requests to.

[13]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[14]: https://dashboard-01.braze.com/app_settings/api_settings/
[15]: https://dashboard-01.braze.com/users/user_search/user-search/
[66]: {{site.baseurl}}/developer_guide/rest_api/messaging/#sending-messages-immediately-via-api-only

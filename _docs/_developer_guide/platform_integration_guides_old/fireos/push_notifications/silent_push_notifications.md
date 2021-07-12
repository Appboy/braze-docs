---
nav_title: Silent Push Notifications
platform: FireOS
page_order: 3

page_type: reference
description: "This article describes how to send silent push notifications, and potential use cases for when silent push notifications may be preferable."
channel: push

---

# Silent Push Notifications

Silent notifications allow you to notify your app in the background when important events occur. You might have new instant messages to deliver, new issues of a magazine to publish, breaking news alerts to send, or the latest episode of your user’s favorite TV show ready for them to download for offline viewing. Silent notifications are great for sporadic but immediately important content, where the delay between background fetches might not be acceptable.

Silent notifications are available through our [Messaging RESTful API][2]. You need only set the `send_to_sync` flag to `true` within the [Android Push Object][3]. You should ensure there are no `title` or `alert` fields set within the [Android Push Object][3] as it will cause errors when `send_to_sync` is set to `true`. You can however still include data `extras` within the [Android Push Object][3].

Silent notifications are also available within the dashboard. In order to send a silent notification, you need only to ensure the title and body fields of the notification are blank as pictured below:

![Android Silent Push Example][6]

This message will cause an intent to be received with an action `.intent.APPBOY_PUSH_RECEIVED`. Handling of this intent to cause any action such as a refresh of app content must be defined within the broadcast receiver you defined in [Step 4 of Enabling Push Notifications - Android][4]. Please see [AppboyBroadcastReceiver.java][5] for an example of this receiver.

[2]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[3]: {{site.baseurl}}/developer_guide/rest_api/messaging/#android-push-object
[4]: {{site.baseurl}}/developer_guide/platform_integration_guides/fireos/push_notifications/integration/#step-4-define-notification-channels
[5]: https://github.com/Appboy/appboy-android-sdk/blob/master/samples/custom-broadcast/src/main/java/com/appboy/custombroadcast/AppboyBroadcastReceiver.java "AppboyBroadcastReceiver.java -- Sample Project"
[6]: {% image_buster /assets/img_archive/SilentPushExample.png %} "Silent Push Notification Example -- Android"

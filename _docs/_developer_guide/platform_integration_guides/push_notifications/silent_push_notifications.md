---
nav_title: Silent Push Notifications
page_order: 1
description: ""
---

# Silent Push Notifications

{% tabs %}
{% tab Android & FireOS %}

Silent notifications allow you to notify your app in the background when important events occur. You might have new instant messages to deliver, new issues of a magazine to publish, breaking news alerts to send, or the latest episode of your user’s favorite TV show ready for them to download for offline viewing. Silent notifications are great for sporadic but immediately important content, where the delay between background fetches might not be acceptable.

Silent notifications are available through our [Messaging RESTful API][2]. You need only set the `send_to_sync` flag to `true` within the [Android Push Object][3]. You should ensure there are no `title` or `alert` fields set within the [Android Push Object][3] as it will cause errors when `send_to_sync` is set to `true`. You can however still include data `extras` within the [Android Push Object][3].

Silent notifications are also available within the dashboard. To send a silent notification, you need only to ensure the title and body fields of the notification are blank as pictured below:

![Android Silent Push Example][6]

This message will cause an intent to be received with an action `.intent.APPBOY_PUSH_RECEIVED`. Handling of this intent to cause any action such as a refresh of app content must be defined within the broadcast receiver you defined in [Step 4 of Enabling Push Notifications - Android][4]. Please see [AppboyBroadcastReceiver.java][5] for an example of this receiver.

[2]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[3]: {{site.baseurl}}/developer_guide/rest_api/messaging/#android-push-object
[4]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/#step-4-define-notification-channels
[5]: https://github.com/Appboy/appboy-android-sdk/blob/master/samples/custom-broadcast/src/main/java/com/appboy/custombroadcast/AppboyBroadcastReceiver.java "AppboyBroadcastReceiver.java -- Sample Project"
[6]: {% image_buster /assets/img_archive/SilentPushExample.png %} "Silent Push Notification Example -- Android"

{% endtab %}
{% tab iOS %}

Remote notifications allow you to notify your app when important events occur. You might have new instant messages to deliver, breaking news alerts to send, or the latest episode of your user's favorite TV show ready for them to download for offline viewing. Remote notifications are great for sporadic but immediately important content, where the delay between background fetches might not be acceptable. Remote Notifications can also be much more efficient than Background Fetch, as your application only launches when necessary. However, remote notifications are limited by the system and cannot automatically launch your application if the user has force-quit it.

A Remote Notification is really just a normal Push Notification with the `content-available` flag set. You might send a push with an alert message informing the user that something has happened, while you update the UI in the background. But Remote Notifications can also be silent, containing no alert message or sound, used only to update your app's interface or trigger background work. You might then post a local notification when you've finished downloading or processing the new content.

Silent push notifications are rate-limited, so don't be afraid of sending as many as your application needs. iOS and the APNs servers will control how often they are delivered, and you won't get into trouble for sending too many. If your push notifications are throttled, they might be delayed until the next time the device sends a keep-alive packet or receives another notification.

## Sending Remote Notifications

To send a remote notification, set the `content-available` flag to 1 in a push notification payload. When you're sending a Remote Notification, you might also want to include some data in the notification payload, so your application can reference the event. This could save you a few networking requests and increase the responsiveness of your app.

The `content-available` flag can be set in the Braze dashboard (pictured below) as well as within our [User API][1].

![content-available][2]

## Use Silent Remote Notifications to Trigger Background Work

Silent remote notifications can wake your app from a "Suspended" or "Not Running" state, to update content or run certain tasks without notifying your users. To send a silent remote push notification, you just need to set up the `content-available` flag with no message nor sound. Please set up your app's background mode to enable `remote notifications` under the "Capabilities" tab in your project settings.

![background-mode-enabled][3]

Enabling background mode for remote notifications is required for Braze's [Uninstall Tracking][6] feature.

Even with the remote notifications background mode enabled, the system will not launch your app into the background if the user has force-quit the application. The user must explicitly launch the application or reboot the device before the app can be automatically launched into the background by the system.

For more information, please refer to Apple's documentation on [Pushing Background Updates][4] and [`application:didReceiveRemoteNotification:fetchCompletionHandler:`][5].

## iOS Silent Notifications Limitations
The iOS operating system may gate notifications for some features. Please note that if you are experiencing difficulties with these features, the iOS's silent notifications gate might be the cause.

Braze has several features which rely on iOS Silent Push Notifications:

|Feature|User Experience|
|---|---|
|Uninstall Tracking | User receives a silent, nightly uninstall tracking push.|
|Geofences | Silent syncing of geofences from server to device.|
{: .reset-td-br-1 .reset-td-br-2}

For more information, check out Apple's developer site on the [Instance Method][7] and [Unreceived Notifications][8].

[1]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[2]: {% image_buster /assets/img_archive/remote_notification.png %} "content available"
[3]: {% image_buster /assets/img_archive/background_mode.png %} "background mode enabled"
[4]: https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/pushing_background_updates_to_your_app?language=objc
[5]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intfm/UIApplicationDelegate/application:didReceiveRemoteNotification:fetchCompletionHandler:
[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/uninstall_tracking/#uninstall-tracking
[7]: https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623013-application
[8]:https://developer.apple.com/library/content/technotes/tn2265/_index.html#//apple_ref/doc/uid/DTS40010376-CH1-TNTAG23

{% endtab %}
{% endtabs %}


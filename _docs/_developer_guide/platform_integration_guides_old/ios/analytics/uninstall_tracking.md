---
nav_title: Uninstall Tracking
platform: iOS
page_order: 7
description: "This article covers how to configure uninstall tracking for your iOS application."

---

# Uninstall Tracking

Uninstall Tracking utilizes background push notifications with a Braze flag in the payload. For more information, see our [Uninstall Tracking][6] page in our User Guide.

## Step 1: Enabling Background Push

Make sure that you have enabled the "Remote notifications" option from the "Background Modes" section of your Xcode project's Capabilities tab. For additional details, refer to our documentation on [Silent Push Notifications][5].

## Step 2: Checking for Braze Background Push

Braze uses background push notifications to collect uninstall tracking analytics. Follow the instructions [here][4] to ensure that your application does not take any unwanted actions upon receiving Braze's uninstall tracking notifications.

## Step 3: Test from the Dashboard

To ensure that your app does not take any unwanted automatic actions upon receiving a Braze uninstall tracking push, send yourself a test push from the [Dashboard][7].

1. On the Campaigns page, create a Push Notification campaign and select iOS Push as your platform.

2. On the Additional Message Settings page,
  - Add the key `appboy_uninstall_tracking` with corresponding value `true`
  - Check "Add Content-Available Flag"

    ![key-value  Pair][9]{: width="40%"}

3. Use the Preview Message page to send yourself a test uninstall tracking push.

    ![Test User][10]{: width="40%"}

4. Check that your app does not take any unwanted automatic actions upon receiving the push.

{% alert important %}
The above steps are a proxy for sending an uninstall tracking push from Braze. If you have badge counts enabled, a badge number will be sent along with the test push, but Braze's uninstall tracking pushes will not set a badge number on your application.
{% endalert %}

## Step 4: Enable Uninstall Tracking

Follow the instructions for enabling uninstall tracking using our [article on Uninstall Tracking][6].

[4]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/customization/#ignoring-brazes-internal-push-notifications
[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/silent_push_notifications/
[6]: {{site.baseurl}}/user_guide/data_and_analytics/tracking/uninstall_tracking/#uninstall-tracking
[7]: https://dashboard-01.braze.com/
[9]: {% image_buster /assets/img_archive/ios-uninstall-tracking-2.png %}
[10]: {% image_buster /assets/img_archive/ios-uninstall-tracking-3.png %}

---
nav_title: Badge Counts
platform: iOS
page_order: 0.2
description: "This article covers how to implement badge counts in your iOS push notifications."
channel:
  - push

---

## Badge Counts

You can specify the desired badge count when you compose a push notification through Braze's dashboard. You may also update your badge count manually through your application's [`applicationIconBadgeNumber`][20] property or through the [remote notification payload][21]. Braze will also clear the badge count when a Braze notification is received while the app is foregrounded. If you do not have a plan for clearing badges as part of normal app operation or by sending pushes which clear the badge, you should clear the badge when the app becomes active by adding the following code to your app's `applicationDidBecomeActive:` delegate method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[UIApplication sharedApplication].applicationIconBadgeNumber = 0;
```

{% endtab %}
{% tab swift %}

```swift
UIApplication.shared.applicationIconBadgeNumber = 0
```

{% endtab %}
{% endtabs %}

Please note that setting the badge number to 0 will also clear up notifications in the notification center. So even if you don't set badge number in push payloads, you can still set the badge number to 0 to remove the push notification(s) in the notification center after users clicked on the push.

[20]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/instp/UIApplication/applicationIconBadgeNumber
[21]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1
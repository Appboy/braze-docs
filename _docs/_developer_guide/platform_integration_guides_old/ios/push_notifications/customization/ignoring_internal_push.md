---
nav_title: Ignoring Internal Push
platform: iOS
page_order: 0.2
description: "This article covers how to ignore Braze's internal push notifications."
channel:
  - push

---

# Ignoring Braze's Internal Push Notifications

Braze uses silent push notifications for internal implementation of certain advanced features. For most integrations, this requires no changes on your app's behalf. However, if you integrate a Braze feature that relies on internal push notifications (i.e., uninstall tracking or geofences), you may want to update your app to ignore Braze's internal pushes.

If your app takes automatic actions on application launches or background pushes, you should consider gating that activity so that it's not triggered by Braze's internal push notifications. For example, if you have logic that calls your servers for new content upon every background push or application launch, you likely would not want Braze’s internal pushes triggering that because you would incur unnecessary network traffic. Furthermore, because Braze sends certain kinds of internal pushes to all users at approximately the same time, not gating network calls on launch from internal pushes could introduce significant server load.

## Checking Your App for Automatic Actions

You should check your application for automatic actions in the following places and update your code to ignore Braze’s internal pushes:

1. **Push Receivers.** Background push notifications will call `application:didReceiveRemoteNotification:fetchCompletionHandler:` on the `UIApplicationDelegate`.
2. **Application Delegate.** Background pushes can launch [suspended][4] apps into the background, triggering the `application:willFinishLaunchingWithOptions:` and `application:didFinishLaunchingWithOptions:` methods on your `UIApplicationDelegate`. You can check the `launchOptions` of these methods to determine if the application has been launched from a background push.

## Using Braze's Internal Push Utility Methods

You can use the utility methods in `ABKPushUtils` to check if your app has received or was launched by a Braze internal push. `isAppboyInternalRemoteNotification:` will return `YES` on all Braze internal push notifications, while `isUninstallTrackingRemoteNotification:` and `isGeofencesSyncRemoteNotification:` will return `YES` for uninstall tracking and geofences sync notifications, respectively.
See [`ABKPushUtils.h`][1] for method declarations.

## Implementation Example {#internal-push-implementation-example}

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  NSDictionary *pushDictionary = launchOptions[UIApplicationLaunchOptionsRemoteNotificationKey];
  BOOL launchedFromAppboyInternalPush = pushDictionary && [ABKPushUtils isAppboyInternalRemoteNotification:pushDictionary];
  if (!launchedFromAppboyInternalPush) {
    // ... Gated logic here (e.g., pinging your server to download content) ...
  }
}
```

```objc
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
  if (![ABKPushUtils isAppboyInternalRemoteNotification:userInfo]) {
    // ... Gated logic here (e.g., pinging server for content) ...
  }
}
```

{% endtab %}
{% tab swift %}

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil) -> Bool {
  let pushDictionary = launchOptions?[UIApplicationLaunchOptionsKey.remoteNotification] as? NSDictionary as? [AnyHashable : Any] ?? [:]
  let launchedFromAppboyInternalPush = ABKPushUtils.isAppboyInternalRemoteNotification(pushDictionary)
  if (!launchedFromAppboyInternalPush) {
    // ... Gated logic here (e.g., pinging your server to download content) ...
  }
}
```

```swift
func application(_ application: UIApplication,
                 didReceiveRemoteNotification userInfo: [AnyHashable : Any],
                 fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
  if (!ABKPushUtils.isAppboyInternalRemoteNotification(userInfo)) {
    // ... Gated logic here (e.g., pinging server for content) ...
  }
}
```

{% endtab %}
{% endtabs %}

[1]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKPushUtils.h
[4]: https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html#//apple_ref/doc/uid/TP40007072-CH2-SW3
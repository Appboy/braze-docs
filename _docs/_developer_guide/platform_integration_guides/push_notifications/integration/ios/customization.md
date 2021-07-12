---
nav_title: Customization
page_title: Customization for iOS Push Notifications
page_order: 1

platform: iOS
description: ""
channel: push
---

# Customization for iOS Push Notifications

## Action Buttons {#push-action-buttons-integration}

The Braze iOS SDK supports default push categories, including URL handling support for each push action button. Currently, the default categories have four sets of push action buttons: `Accept`/`Decline`, `Yes`/`No`, `Confirm`/`Cancel` and `More`. 

![Illustration of Notification Action][1]

To register Braze's default push categories, follow the integration instructions below:

### Step 1: Adding Braze Default Push Categories

Use the following code to register for Braze's default push categories when you [register for push][3]:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// For UserNotification.framework (iOS 10+ only)
NSSet *appboyCategories = [ABKPushUtils getAppboyUNNotificationCategorySet];
[[UNUserNotificationCenter currentNotificationCenter] setNotificationCategories:appboyCategories];

// For UIUserNotificationSettings (before iOS 10)
NSSet *appboyCategories = [ABKPushUtils getAppboyUIUserNotificationCategorySet];
UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge
                                                                         categories:appboyCategories];
[[UIApplication sharedApplication] registerUserNotificationSettings:settings];
```

{% endtab %}
{% tab swift %}

```swift
// For UserNotification.framework (iOS 10+ only)
let appboyCategories = ABKPushUtils.getAppboyUNNotificationCategorySet()
UNUserNotificationCenter.current().setNotificationCategories(appboyCategories)

// For UIUserNotificationSettings (before iOS 10)
let appboyCategories = ABKPushUtils.getAppboyUIUserNotificationCategorySet()
let settings = UIUserNotificationSettings.init(types: .badge, categories: appboyCategories)
UIApplication.shared.registerUserNotificationSettings(settings)
```

{% endtab %}
{% endtabs %}

Clicking on push action buttons with background activation mode will only dismiss the notification and will not open the app. Button click analytics for these actions will be flushed to the server the next time the user opens the app.

>  If you wish to create your own custom notification categories, see our [action button customization][4] documentation.

### Step 2: Enable Interactive Push Handling

If you are using the UNNotification Framework and have implemented [Braze delegates][5], you should already have this method integrated. 

To enable Braze's push action button handling, including click analytics and URL routing, add the following code to your app's `(void)userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:` delegate method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] userNotificationCenter:center
                           didReceiveNotificationResponse:response
                               withCompletionHandler:completionHandler];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.userNotificationCenter(center,
                                                didReceive: response,
                                                withCompletionHandler: completionHandler)
```

{% endtab %}
{% endtabs %}

If you are not using UNNotification Framework you will need to add the following code to your app's `application:handleActionWithIdentifier:forRemoteNotification:completionHandler:` to enable Braze's push action button handling:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] getActionWithIdentifier:identifier
                           forRemoteNotification:userInfo
                               completionHandler:completionHandler];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.getActionWithIdentifier(identifier,
                                                 forRemoteNotification: userInfo,,
                                                 completionHandler: completionHandler)
```

{% endtab %}
{% endtabs %}

{% alert important %}
We strongly recommend that people using `handleActionWithIdentifier` begin using UNNotification Framework. We recommend this due to the [deprecation of handleActionWithIdentifier](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623068-application?language=objc).
{% endalert %}

### Push Category Customization

In addition to providing a set of [default push categories][2], Braze supports custom notification categories and actions. Once you register categories in your application, you can use the Braze dashboard to send notification categories to your users.

>  If you are not using the `UserNotifications` framework, please see this [alternative categories documentation][6].

These categories can then be assigned to push notifications via our dashboard to trigger the action button configurations of your design. Here's an example that leverages the "LIKE_CATEGORY" displayed on the device!

![Push Category Customization Example][7]

## Badge Counts {#badge-counts}

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

## Custom Sounds {#custom-sounds}

### Step 1: Hosting the Sound in the App

Custom push notification sounds must be hosted locally within the main bundle of the client application. The following audio data formats are accepted:

- Linear PCM
- MA4
- µLaw
- aLaw

You can package the audio data in an aiff, wav, or caf file. Then, in Xcode, add the sound file to your project as a nonlocalized resource of the application bundle.

You may use the afconvert tool to convert sounds. For example, to convert the 16-bit linear PCM system sound Submarine.aiff to IMA4 audio in a CAF file, use the following command in the Terminal application:

```bash
afconvert /System/Library/Sounds/Submarine.aiff ~/Desktop/sub.caf -d ima4 -f caff -v
```

You can inspect a sound to determine its data format by opening it in QuickTime Player and choosing Show Movie Inspector from the Movie menu.

Custom sounds must be under 30 seconds when played. If a custom sound is over that limit, the default system sound is played instead.

### Step 2: Providing the Dashboard with a Protocol URL for the Sound

Your sound must be hosted locally within the app. You must specify a Protocol URL that directs to the location of the sound file in the app within the "Sound" field in the push composer. Specifying “default” in this field will play the default notification sound on the device. This can be specified via our [Messaging API][25] or our dashboard under “Advanced Settings” in the push composer wizard as pictured below:

![Push Notification Sound][8]

If the specified sound file doesn’t exist or the keyword ‘default’ is entered, Braze will use the default device alert sound. Aside from our dashboard, sound can also be configured via our our [Messaging API][12]. For additional information see the Apple Developer Documentation regarding ["Preparing Custom Alert Sounds"][9].

## Ignoring Braze's Internal Push Notifications {#ignoring-internal-push}

Braze uses silent push notifications for internal implementation of certain advanced features. For most integrations, this requires no changes on your app's behalf. However, if you integrate a Braze feature that relies on internal push notifications (i.e., uninstall tracking or geofences), you may want to update your app to ignore Braze's internal pushes.

If your app takes automatic actions on application launches or background pushes, you should consider gating that activity so that it's not triggered by Braze's internal push notifications. For example, if you have logic that calls your servers for new content upon every background push or application launch, you likely would not want Braze’s internal pushes triggering that because you would incur unnecessary network traffic. Furthermore, because Braze sends certain kinds of internal pushes to all users at approximately the same time, not gating network calls on launch from internal pushes could introduce significant server load.

### Checking Your App for Automatic Actions

You should check your application for automatic actions in the following places and update your code to ignore Braze’s internal pushes:

1. **Push Receivers.** Background push notifications will call `application:didReceiveRemoteNotification:fetchCompletionHandler:` on the `UIApplicationDelegate`.
2. **Application Delegate.** Background pushes can launch [suspended][31] apps into the background, triggering the `application:willFinishLaunchingWithOptions:` and `application:didFinishLaunchingWithOptions:` methods on your `UIApplicationDelegate`. You can check the `launchOptions` of these methods to determine if the application has been launched from a background push.

### Using Braze's Internal Push Utility Methods

You can use the utility methods in `ABKPushUtils` to check if your app has received or was launched by a Braze internal push. `isAppboyInternalRemoteNotification:` will return `YES` on all Braze internal push notifications, while `isUninstallTrackingRemoteNotification:` and `isGeofencesSyncRemoteNotification:` will return `YES` for uninstall tracking and geofences sync notifications, respectively.
See [`ABKPushUtils.h`][30] for method declarations.

### Implementation Example {#internal-push-implementation-example}

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

## Extracting Data from Push Notification Key-Value Pairs {#key-value-pairs}

Braze allows you to send custom-defined string key-value pairs, known as extras, along with a push notification to your application. Extras can be defined via the dashboard or API and will be available as key-value pairs within the notification dictionary passed to your push delegate implementations.


[1]: {% image_buster /assets/img_archive/iOS8Action.gif %}
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/customization/action_buttons/
[3]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/#step-4-register-push-tokens-with-braze
[4]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/customization/action_buttons/#push-category-customization
[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/#step-5-enable-push-handling
[6]: https://developer.apple.com/reference/uikit/uiusernotificationcategory
[7]: {% image_buster /assets/img_archive/push_example_category.png %}
[8]: {% image_buster /assets/img_archive/sound_push_ios.png %}
[9]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SupportingNotificationsinYourApp.html
[12]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[25]: {{site.baseurl}}/developer_guide/rest_api/messaging/#messaging
[14]: https://developer.apple.com/reference/usernotifications/unnotificationcategory "Categories Docs"
[20]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/instp/UIApplication/applicationIconBadgeNumber
[21]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1

[30]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKPushUtils.h
[31]: https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html#//apple_ref/doc/uid/TP40007072-CH2-SW3



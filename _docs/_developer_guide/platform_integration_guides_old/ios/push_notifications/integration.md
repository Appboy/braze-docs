---
nav_title: Integration
platform: iOS
page_order: 0
description: "This article covers how to integrate push notifications in your iOS application."
channel:
  - push


local_redirect:
  ios-10-rich-notifications: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/rich/'
local_redirect:
  creating-a-service-extension: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/rich/#creating-a-service-extension'
local_redirect:
  setting-up-the-service-extension: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/rich/#setting-up-the-service-extension'
local_redirect:
  creating-a-rich-notification-in-your-dashboard: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/rich/#creating-a-rich-notification-in-your-dashboard'
local_redirect:
  push-action-buttons-integration: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/action_buttons/'
local_redirect:
  step-1-adding-braze-default-push-categories: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/action_buttons/#step-1-adding-braze-default-push-categories'
local_redirect:
  step-2-enable-interactive-push-handling: '/docs/developer_guide/platform_integration_guides/ios/push_notifications/action_buttons/#step-2-enable-interactive-push-handling'
  
---

# Push Integration

## Step 1: Configure Push Notifications

Before you can send an iOS push notification using Braze, you must provide your Push notification file or certificate from Apple. You may present either a `.p8` file (recommended) or a `.p12` certificate.

{% tabs local %}
  {% tab .p8 File (Recommended) %}
__Using a .p8 File (Authentication Tokens)__

As described on [this page](https://help.apple.com/developer-account/#/devcdfbb56a3):

1. In your developer account, go to [Certificates, Identifiers & Profiles](https://developer.apple.com/account/ios/certificate).
2. Under Keys, select All and click the Add button (+) in the upper-right corner.
3. Under Key Description, enter a unique name for the signing key.
4. Under Key Services, select the APNs checkbox, then click Continue. Click Confirm.
5. Note the Key ID. Click Download to generate and download the key now.

>  When you download the key, it is saved as a text file with a .p8 file extension. Save the file in a secure place because the key is **not saved in your developer account and you won’t be able to download it again**.

6. Navigate to the [Settings page](https://dashboard-01.braze.com/app_settings/app_settings) in the dashboard and upload the .p8 file.
7. When prompted, also enter your [app's Bundle Id](https://developer.apple.com/account/ios/identifier/bundle/), the [Key Id](https://developer.apple.com/account/ios/authkey), and your [Team Id](https://developer.apple.com/account/#/membership). Click `Save`.

  {% endtab %}
  {% tab .p12 Certificate (Legacy) %}
__Using a .p12 Certificate (Legacy)__

Alternatively, you may utilize Apple's older authentication scheme (.p12 SSL certificates). Unlike the .p8 solution described above, these certificates automatically expire every year and will require you to regenerate and re-upload them. Braze will send you email reminders as the certificate approaches expiration to help your notifications continue uninterrupted, but because this is a manual process we recommend utilizing the above-described .p8 authentication scheme instead.  However, if you still wish to, you may configure and upload .p12 certificates as described here:

_Step 1: Generate Certificate Signing Request_

1. Navigate to the [iOS Provisioning Portal](https://developer.apple.com/ios/manage/overview/index.action)
2. Select Identifiers > App IDs in the left sidebar

 ![iOSPush3]({% image_buster /assets/img_archive/ios_provisioning.png %})

3. Select your application.
4. If push notifications are not enabled, click Edit to update the app settings
  ![AppleProvisioningOptions]({% image_buster /assets/img_archive/AppleProvisioningOptions.png %})
5. Tick the Enable check box and click Create Certificate under the Production SSL Certificate
  ![iOSPush32]({% image_buster /assets/img_archive/push_cert_gen.png %})
6. Follow the instructions from the SSL certificate assistant. You should now see a green status to indicate that push is enabled.

>  You must update your provisioning profile for the app after you create your SSL certificates. A simple "Refresh" in the organizer will accomplish this.

_Step 2: Export Certificate_

1. Download the production push certificate that you just created and open it with the Keychain Access application.
2. In Keychain Access, click on My Certificates and locate your push certificate.
3. Export it as a `.p12` file and use a temporary, unsecure password (you will need this password when uploading your certificate to Braze).
4. Navigate to the [app settings page](https://dashboard-01.braze.com/app_settings/app_settings) in the dashboard and upload your production certificate.

 ![push upload example]({% image_buster /assets/img_archive/push_cert_upload.png %})

>  You can upload either your development or production push certificates to the dashboard for your distribution provisioning profile apps, but you can only have one active at a time. As such, if you wish to do repeated testing of push notifications once your app goes live in the App Store, we recommend setting up a separate App Group or App for the development version of your app.

  {% endtab %}
{% endtabs %}


## Step 2: Enable Push Capabilities

In your project settings, ensure that under the `Capabilities` tab your `Push Notifications` capability is toggled on, as described on [this page](https://help.apple.com/developer-account/#/devcdfbb56a3).

![enable push notification][24]

>  If you have separate development and production push certificates, please make sure to uncheck the `Automatically manage signing` box in the `General` tab. This will allow you to choose different provisioning profiles for each of your build configurations, as Xcode's automatic code signing feature only does development signing.
![xcode 8 auto signing][34]

## Step 3: Register for Push Notifications

The appropriate code sample below must be included within your app's `application:didFinishLaunchingWithOptions:` delegate method for your users' device to register with APNs.

Braze also provides default push categories for push action button support, which must be manually added to your push registration code. See our [push action buttons documentation][35] for additional integration steps.

> If you've implemented a custom push prompt as described in our [push best practices][0], make sure that you're calling the following code **EVERY time the app runs** after they grant push permissions to your app. **Apps need to re-register with APNs as [device tokens can change arbitrarily][19].**

{% alert warning %}
Be sure to call all push integration code in your application's main thread.
{% endalert %}

### Using UserNotification Framework (iOS 10+)

If you are using the `UserNotifications` framework (recommended) that was introduced in iOS 10, add the following code to the `application:didFinishLaunchingWithOptions:` method of your app delegate:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
if (floor(NSFoundationVersionNumber) > NSFoundationVersionNumber_iOS_9_x_Max) {
  UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
  center.delegate = self;
  UNAuthorizationOptions options = UNAuthorizationOptionAlert | UNAuthorizationOptionSound | UNAuthorizationOptionBadge;
  if (@available(iOS 12.0, *)) {
  options = options | UNAuthorizationOptionProvisional;
  }
  [center requestAuthorizationWithOptions:options
                        completionHandler:^(BOOL granted, NSError * _Nullable error) {
                          [[Appboy sharedInstance] pushAuthorizationFromUserNotificationCenter:granted];
  }];
  [[UIApplication sharedApplication] registerForRemoteNotifications];
} else {
  UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeAlert | UIUserNotificationTypeSound) categories:nil];
  [[UIApplication sharedApplication] registerForRemoteNotifications];
  [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
}
```

{% endtab %}
{% tab swift %}

```swift
if #available(iOS 10, *) {
  let center = UNUserNotificationCenter.current()
  center.delegate = self as? UNUserNotificationCenterDelegate
  var options: UNAuthorizationOptions = [.alert, .sound, .badge]
  if #available(iOS 12.0, *) {
    options = UNAuthorizationOptions(rawValue: options.rawValue | UNAuthorizationOptions.provisional.rawValue)
  }
  center.requestAuthorization(options: options) { (granted, error) in
    Appboy.sharedInstance()?.pushAuthorization(fromUserNotificationCenter: granted)
  }
  UIApplication.shared.registerForRemoteNotifications()
} else {
  let types : UIUserNotificationType = [.alert, .badge, .sound]
  let setting : UIUserNotificationSettings = UIUserNotificationSettings(types:types, categories:nil)
  UIApplication.shared.registerUserNotificationSettings(setting)
  UIApplication.shared.registerForRemoteNotifications()
}
```

{% endtab %}
{% endtabs %}

{% alert important %}
Please note that the code sample above includes integration for Provisional Push Authentication (lines 5 and 6 in `Objective-C` tab; lines 5 and 6 in `Swift` tab). If you are not planning on using provisional authorization in your app, you can remove the lines of code that add `UNAuthorizationOptionProvisional` to the `requestAuthorization` options in the above code snippet. Learn more about [Push Provisional Authentication, iOS notification options, and iOS 12 here]({{site.baseurl}}/user_guide/message_building_by_channel/push/notification_options_ios/).
{% endalert %}

{% alert warning %}
You must assign your delegate object using `center.delegate = self` synchronously before your app finishes launching, preferably in `application:didFinishLaunchingWithOptions:`. Not doing so may cause your app to miss incoming push notifications. For more information, visit Apple's [`UNUserNotificationCenterDelegate` documentation](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate).
{% endalert %}

### Without UserNotifications Framework

If you are not using the `UserNotifications` framework, add the following code to the `application:didFinishLaunchingWithOptions:` method of your app delegate:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeAlert | UIUserNotificationTypeSound) categories:nil];
[[UIApplication sharedApplication] registerForRemoteNotifications];
[[UIApplication sharedApplication] registerUserNotificationSettings:settings];
```

{% endtab %}
{% tab swift %}

```swift
let types : UIUserNotificationType = UIUserNotificationType.Badge | UIUserNotificationType.Sound | UIUserNotificationType.Alert
var setting : UIUserNotificationSettings = UIUserNotificationSettings(forTypes: types, categories: nil)
UIApplication.shared.registerUserNotificationSettings(setting)
UIApplication.shared.registerForRemoteNotifications()
```

{% endtab %}
{% endtabs %}


## Step 4: Register Push Tokens With Braze

Once APNs registration is complete, the following method must be altered to pass the resulting `deviceToken` to Braze so the user becomes enabled for push notifications:

{% tabs %}
{% tab OBJECTIVE-C %}

Add the following code to your `application:didRegisterForRemoteNotificationsWithDeviceToken:` method:

```objc
[[Appboy sharedInstance] registerDeviceToken:deviceToken];
```

{% endtab %}
{% tab swift %}

Add the following code to your app's `application(_:didRegisterForRemoteNotificationsWithDeviceToken:)` method:

```swift
Appboy.sharedInstance()?.registerDeviceToken(deviceToken)
```

{% endtab %}
{% endtabs %}

{% alert important %}
The `application:didRegisterForRemoteNotificationsWithDeviceToken:` delegate method is called every time after `[[UIApplication sharedApplication] registerForRemoteNotifications]` is called. If you are migrating to Braze from another push service and your user's device has already registered with APNs, this method will collect tokens from existing registrations the next time the method is called, and users will not have to re-opt-in to push.
{% endalert %}

## Step 5: Enable Push Handling

The following code passes received push notifications along to Braze and is necessary for logging push analytics and link handling.

{% alert warning %}
Be sure to call all push integration code in your application's main thread.
{% endalert %}

### iOS 10+

When building against iOS 10+ we recommend you integrate the `UserNotifications` framework and do the following:

{% tabs %}
{% tab OBJECTIVE-C %}

Add the following code to your application's `application:didReceiveRemoteNotification:fetchCompletionHandler:` method:

```objc
[[Appboy sharedInstance] registerApplication:application
                didReceiveRemoteNotification:userInfo
                      fetchCompletionHandler:completionHandler];
```

Next, add the following code to your app's `(void)userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:` method:

```objc
[[Appboy sharedInstance] userNotificationCenter:center
                 didReceiveNotificationResponse:response
                          withCompletionHandler:completionHandler];
```

**Foreground Push Handling**

To display a push notification while the app is in the foreground, implement `userNotificationCenter:willPresentNotification:withCompletionHandler:`:

```objc
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
       willPresentNotification:(UNNotification *)notification
         withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {
  if (@available(iOS 14.0, *)) {
    completionHandler(UNNotificationPresentationOptionList | UNNotificationPresentationOptionBanner);
  } else {
    completionHandler(UNNotificationPresentationOptionAlert);
  }
}
```

If the foreground notification is clicked, the iOS 10 push delegate `userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:` will be called and Braze will log a push click event.

{% endtab %}
{% tab swift %}

Add the following code to your app's `application(_:didReceiveRemoteNotification:fetchCompletionHandler:)` method:

```swift
Appboy.sharedInstance()?.register(application,
                                            didReceiveRemoteNotification: userInfo,
                                            fetchCompletionHandler: completionHandler)
```

Next, add the following code to your app's `userNotificationCenter(_:didReceive:withCompletionHandler:)` method:

```swift
Appboy.sharedInstance()?.userNotificationCenter(center,
                                               didReceive: response,
                                               withCompletionHandler: completionHandler)
```

**Foreground Push Handling**

To display a push notification while the app is in the foreground, implement `userNotificationCenter(_:willPresent:withCompletionHandler:)`:

```swift
func userNotificationCenter(_ center: UNUserNotificationCenter,
                              willPresent notification: UNNotification,
                              withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
  if #available(iOS 14.0, *) {
    completionHandler([.list, .banner]);
  } else {
    completionHandler([.alert]);
  }
}
```

If the foreground notification is clicked, the iOS 10 push delegate `userNotificationCenter(_:didReceive:withCompletionHandler:)` will be called and Braze will log a push click event.

{% endtab %}
{% endtabs %}

### Pre-iOS 10

iOS 10 updated behavior such that it no longer calls `application:didReceiveRemoteNotification:fetchCompletionHandler:` when a push is clicked.  For this reason, if you don't update to building against iOS 10+ and use the `UserNotifications` framework, you have to call Braze from both old-style delegates, which is a break from our previous integration.

For apps building against SDKs < iOS 10, use the following instructions:

{% tabs %}
{% tab OBJECTIVE-C %}

To enable open tracking on push notifications, add the following code to your app's `application:didReceiveRemoteNotification:fetchCompletionHandler:` method:

```objc
[[Appboy sharedInstance] registerApplication:application
                didReceiveRemoteNotification:userInfo
                      fetchCompletionHandler:completionHandler];
```

To support push analytics on iOS 10, you must also add the following code to your app's `application:didReceiveRemoteNotification:` delegate method:

```objc
[[Appboy sharedInstance] registerApplication:application
                didReceiveRemoteNotification:userInfo];
```

{% endtab %}
{% tab swift %}

To enable open tracking on push notifications, add the following code to your app's `application(_:didReceiveRemoteNotification:fetchCompletionHandler:)` method:

```swift
Appboy.sharedInstance()?.register(application,
  didReceiveRemoteNotification: userInfo,
  fetchCompletionHandler: completionHandler)
```

To support push analytics on iOS 10, you must also add the following code to your app's `application(_:didReceiveRemoteNotification:)` delegate method:

```swift
Appboy.sharedInstance()?.register(application,
  didReceiveRemoteNotification: userInfo)
```

{% endtab %}
{% endtabs %}

## Step 6: Deep Linking

Deep linking from a push into the app is automatically handled via our standard push integration documentation. If you'd like to learn more about how to add deep links to specific locations in your app, see our [Advanced Use Cases section on Deep Linking for iOS][10].


[0]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/troubleshooting/
[10]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/advanced_use_cases/linking/#linking-implementation
[19]: https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html
[24]: {% image_buster /assets/img_archive/Enable_push_capabilities.png %}
[34]: {% image_buster /assets/img_archive/xcode8_auto_signing.png %}
[35]: #push-action-buttons-integration

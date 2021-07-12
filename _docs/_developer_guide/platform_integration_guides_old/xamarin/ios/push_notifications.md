---
nav_title: Push Notifications
platform: Xamarin
subplatform: iOS
page_order: 1
description: "This article covers iOS push notification integration for the Xamarin platform."


---

# Push Notifications

See [the iOS integration instructions][1] for information about setting up your application with push and storing your credentials on our server.

## Integration

### Requesting Push Permissions

Set up push permissions by adding the following code to the ```FinishedLaunching``` section of your ```AppDelegate.cs```:

```csharp
// C#
UIUserNotificationSettings settings = UIUserNotificationSettings.GetSettingsForTypes(UIUserNotificationType.Badge | UIUserNotificationType.Alert | UIUserNotificationType.Sound, null);
UIApplication.SharedApplication.RegisterForRemoteNotifications();
UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
```

>  If you’ve implemented a custom push opt-in prompt, make sure that you’re calling the above code EVERY time the app runs after they grant push permissions to your app. Apps need to reregister with APNs as device tokens can change arbitrarily.

### Registering Push Tokens

Register for your push tokens by adding the following code in the ```RegisteredForRemoteNotifications``` method of your ```AppDelegate.cs```:

```csharp
// C#
Appboy.SharedInstance().RegisterDeviceToken (deviceToken);
```

### Enabling Push Analytics

Enable open tracking on push notifications by adding the following code to the `DidReceiveRemoteNotification` method of your `AppDelegate.cs`:

```csharp
// C#
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
  {
    Appboy.SharedInstance().RegisterApplicationWithFetchCompletionHandler(application, userInfo, completionHandler);
  }
```

## Badge Count

If [badge counts are enabled][2], Braze will display a badge when a customer has unread notifications. By default, this number is 1. Braze will only clear the badge count when the app is opened directly from a Braze push notification. To clear the badge count, you can refer to the [Xamarin documentation][3] and use the following code:

```csharp
// C#
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
```



[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/
[2]: {{site.baseurl}}/help/best_practices/utilizing_badge_count/#badge-count-with-braze
[3]: https://developer.xamarin.com/guides/cross-platform/application_fundamentals/notifications/ios/local_notifications_in_ios/#Handling_Notifications

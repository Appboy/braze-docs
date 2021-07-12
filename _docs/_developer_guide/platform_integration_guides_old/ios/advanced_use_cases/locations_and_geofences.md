---
nav_title: Locations & Geofences
platform: iOS
page_order: 6
description: "This reference article covers how to implement locations and geofences in your iOS application."
Tool:
  - Location

---

# Locations & Geofences

Geofences are only available in select Braze packages. For access please create a support ticket or speak with your Braze Customer Success Manager. Learn more in [Docs]({{site.baseurl}}/developer_guide/platform_integration_guides/fireos/advanced_use_cases/locations_and_geofences/#locations--geofences).

To support geofences for iOS:

1. Your integration must support background push notifications.

2. Braze Geofences [must be enabled][1] through the SDK either implicitly by enabling location collection or explicitly by enabling geofence collection. They are not enabled by default.

{% alert important %}
As of iOS 14, Geofences do not work reliably for users who choose to give their Approximate Location permission.
{% endalert %}

## Step 1: Enable Background Push

To fully utilize our geofence syncing strategy you must have [Background Push][6] enabled in addition to completing the standard push integration.

## Step 2: Enable Geofences

By default, geofences are enabled based on whether automatic location collection is enabled. You can enable geofences using the `Info.plist` file. Add the `Braze` dictionary to your `Info.plist` file. Inside the `Braze` dictionary, add the `EnableGeofences` boolean subentry and set the value to `YES`. Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

 You can also enable geofences at app startup time via the [`startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions`][4] method. In the `appboyOptions` dictionary, set `ABKEnableGeofencesKey` to `YES`. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[Appboy startWithApiKey:@"YOUR-API_KEY"
          inApplication:application
      withLaunchOptions:options
      withAppboyOptions:@{ ABKEnableGeofencesKey : @(YES) }];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.start(withApiKey: "YOUR-API-KEY",
                 in:application,
                 withLaunchOptions:launchOptions,
                 withAppboyOptions:[ ABKEnableGeofencesKey : true ])
```

{% endtab %}
{% endtabs %}

## Step 3: Check for Braze Background Push

Braze syncs geofences to devices using background push notifications. Follow the instructions [here][7] to ensure that your application does not take any unwanted actions upon receiving Braze's geofence sync notifications.

## Step 4: Add NSLocationAlwaysUsageDescription to your Info.plist

Add the key `NSLocationAlwaysUsageDescription` and `NSLocationAlwaysAndWhenInUseUsageDescription` to your `info.plist` with a `String` value that has a description of why your application needs to track location. Both keys are required by iOS 11 and above.
This description will be shown when the system location prompt requests authorization and should clearly explain the benefits of location tracking to your users.

## Step 5: Request authorization from the user

The Geofences feature is only functional while `Always` location authorization is granted.

To request for `Always` location authorization, use the following code:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
CLLocationManager *locationManager = [[CLLocationManager alloc] init];
[locationManager requestAlwaysAuthorization];
```

{% endtab %}
{% tab swift %}

```swift
var locationManager = CLLocationManager()
locationManager.requestAlwaysAuthorization()
```

{% endtab %}
{% endtabs %}

## Step 6: Enable Geofences on the Dashboard

iOS only allows up to 20 geofences to be stored for a given app. Braze's Locations product will use up some of these 20 available geofence slots. To prevent accidental or unwanted disruption to other geofence-related functionality in your app, location geofences must be enabled for individual Apps on the Dashboard.

For Braze's Locations product to work correctly, you should also ensure that your App is not using all available geofence spots.

### Enable geofences from the Locations page:

![Appboy Developer Console]({% image_buster /assets/img_archive/enable-geofences-locations-page.png %})

### Enable geofences from the Settings page:

![Appboy Developer Console]({% image_buster /assets/img_archive/enable-geofences-app-settings-page.png %})

## Disabling Automatic Geofence Requests

Starting in iOS SDK version 3.21.3, you can disable geofences from being automatically requested. You can do this by using the `Info.plist` file. Add the `Braze` dictionary to your `Info.plist` file. Inside the `Braze` dictionary, add the `DisableAutomaticGeofenceRequests` boolean subentry and set the value to `YES`.

 You can also disable automatic geofence requests at app startup time via the [`startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions`][4] method. In the `appboyOptions` dictionary, set `ABKDisableAutomaticGeofenceRequestsKey` to `YES`. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[Appboy startWithApiKey:@"YOUR-API_KEY"
          inApplication:application
      withLaunchOptions:options
      withAppboyOptions:@{ ABKDisableAutomaticGeofenceRequestsKey : @(YES) }];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.start(withApiKey: "YOUR-API-KEY",
                 in:application,
                 withLaunchOptions:launchOptions,
                 withAppboyOptions:[ ABKDisableAutomaticGeofenceRequestsKey : true ])
```

{% endtab %}
{% endtabs %}

If you choose to use this option, you will need to manually request geofences for the feature to work.

## Manually Requesting Geofences

When the Braze SDK requests geofences to monitor from the backend, it reports the user's current location and in turn, receives geofences that are determined to be optimally relevant based on the location reported. To control the location that the SDK reports for the purposes of receiving the most relevant geofences, starting in iOS SDK version 3.21.3 you can manually request geofences by providing latitude and longitude of a location. To do so, use the following code:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] requestGeofencesWithLongitude:longitude
                                              latitude:latitude];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.requestGeofences(withLongitude: longitude, latitude: latitude)
```

{% endtab %}
{% endtabs %}

__Note:__ There is a rate-limit of one geofence refresh per session.
__Note:__ It is recommended to disable automatic geofence requests when using this method.

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/location_tracking/#enabling-automatic-location-tracking
[4]: #customizing-appboy-on-startup
[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/silent_push_notifications/#use-silent-remote-notifications-to-trigger-background-work
[7]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/customization/#ignoring-brazes-internal-push-notifications

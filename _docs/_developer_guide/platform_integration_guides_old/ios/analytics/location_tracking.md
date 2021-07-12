---
nav_title: Location Tracking
platform: iOS
page_order: 6
description: "This article shows how to configure location tracking for your iOS application."
Tool:
  - Location

---

# Location Tracking

By default, Braze disables location tracking. We enable location tracking after the host application has opted in to location tracking and gained permission from the user. Provided that users have opted into location tracking, Braze will log a single location for each user on session start.

{% alert important %}
In order for location tracking to work reliably in iOS 14 for users who give Approximate Location permission, you must update your SDK version to at least `3.26.1`.
{% endalert %}

## Enabling Automatic Location Tracking
Starting with Braze iOS SDK `v3.17.0`, location tracking is disabled by default. You can enable automatic location tracking using the `Info.plist` file. Add the `Braze` dictionary to your `Info.plist` file. Inside the `Braze` dictionary, add the `EnableAutomaticLocationCollection` boolean subentry and set the value to `YES`. Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

 You can also enable automatic location tracking at app startup time via the [`startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions`][4] method. In the `appboyOptions` dictionary, set `ABKEnableAutomaticLocationCollectionKey` to `YES`. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[Appboy startWithApiKey:@"YOUR-API_KEY"
          inApplication:application
      withLaunchOptions:options
      withAppboyOptions:@{ ABKEnableAutomaticLocationCollectionKey : @(YES) }];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.start(withApiKey: "YOUR-API-KEY",
                 in:application,
                 withLaunchOptions:launchOptions,
                 withAppboyOptions:[ ABKEnableAutomaticLocationCollectionKey : true ])
```

{% endtab %}
{% endtabs %}

### Passing Location Data to Braze

The following two methods can be used to manually set the last known location for the user.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setLastKnownLocationWithLatitude:latitude
                                                     longitude:longitude
                                            horizontalAccuracy:horizontalAccuracy];

```

```objc
[[Appboy sharedInstance].user setLastKnownLocationWithLatitude:latitude
                                                     longitude:longitude
                                            horizontalAccuracy:horizontalAccuracy
                                                      altitude:altitude
                                              verticalAccuracy:verticalAccuracy];

```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setLastKnownLocationWithLatitude(latitude: latitude, longitude: longitude, horizontalAccuracy: horizontalAccuracy)
```

```swift
Appboy.sharedInstance()?.user.setLastKnownLocationWithLatitude(latitude: latitude, longitude: longitude, horizontalAccuracy: horizontalAccuracy, altitude: altitude, verticalAccuracy: verticalAccuracy)
```

{% endtab %}
{% endtabs %}

For more information, see [`ABKUser.h`][5].

[4]: #customizing-appboy-on-startup
[5]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKUser.h

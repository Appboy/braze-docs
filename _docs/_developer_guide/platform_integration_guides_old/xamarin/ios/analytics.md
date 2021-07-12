---
nav_title: Analytics
platform: Xamarin
subplatform: iOS
page_order: 4
description: "This article covers iOS analytics for the Xamarin platform."

---

# Analytics

## Setting User IDs

```csharp
// C#
Appboy.SharedInstance().ChangeUser("YOUR_USER_ID");
```

See [the iOS integration instructions][7] for an in-depth discussion of when and how to set and change a user ID.

## Tracking Custom Events

```csharp
// C#
Appboy.SharedInstance ().LogCustomEvent ("YOUR_EVENT_NAME");
```

**Implementation Example** - `logCustomEvent` is utilized within the `AppboySampleViewController.cs` within the [TestApp.XamariniOS][4] sample application.

See [the iOS integration instructions][3] for an in-depth discussion of event tracking best practices and interfaces.

## Logging Purchases

```csharp
// C#
Appboy.SharedInstance ().LogPurchase ("myProduct", "USD", new NSDecimalNumber("10"));
```

**Implementation Example** - You can see user properties being set in the sample application's `EventsAndPurchasesButtonHandler` method inside `AppboySampleViewController.cs`.

See [the iOS integration instructions][5] for an in-depth discussion of revenue tracking best practices and interfaces.

## Setting Custom Attributes

```csharp
// C#
Appboy.SharedInstance ().User.FirstName = "YOUR_NAME";
```

**Implementation Example** - You can see user properties being set in the sample application's `UserPropertyButtonHandler` method inside `AppboySampleViewController.cs`.

See [the iOS integration instructions][6] for an in-depth discussion of attribute tracking best practices and interfaces.

## Location Tracking

See [Xamarin Walkthrough - Using Background Location][1] and [the iOS integration instructions][2] for information on how to support location tracking.

[1]: http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_4_ios_backgrounding_walkthroughs/location_walkthrough/
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/advanced_use_cases/locations_and_geofences/
[3]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/tracking_custom_events/
[4]: https://github.com/Appboy/appboy-xamarin-bindings/tree/master/appboy-component/samples/ios-unified/TestApp.XamariniOS
[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/logging_purchases/
[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/setting_custom_attributes/
[7]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/setting_user_ids/

---
nav_title: Analytics
platform: Xamarin
subplatform: Android and FireOS
page_order: 4
description: "This article covers Android and FireOS analytics for the Xamarin platform."

---

# Analytics 

## Setting User IDs

```csharp
Appboy.GetInstance(context).ChangeUser("YOUR_USER_ID");
```

See [the Android integration instructions][5] for an in-depth discussion of when and how to set and change a user ID.

## Tracking Custom Events

```csharp
Appboy.GetInstance(context).LogCustomEvent("YOUR_EVENT_NAME");
```

See [the Android integration instructions][3] for an in-depth discussion of event tracking best practices and interfaces.

## Logging Purchases

```csharp
Appboy.GetInstance(context).LogPurchase("YOUR_PURCHASE_NAME", 100);
```

See [the Android integration instructions][4] for an in-depth discussion of revenue tracking best practices and interfaces.

## Setting Custom Attributes

```csharp
Appboy.GetInstance(context).CurrentUser.SetFirstName("FirstName");
```

See [the Android integration instructions][1] for an in-depth discussion of attribute tracking best practices and interfaces.

## Location Tracking

See [the Android integration instructions][2] for information on how to support location tracking.

## Social Data Tracking

You can see the Xamarin binding accessing these interfaces in the `HomeFragment.cs` of our sample app.  The sample code logs a social share and populates the Braze user with data from the social networks.

```csharp
// Record Facebook Data
FacebookUser facebookUser = new FacebookUser("708379", "Test", "User", "test@braze.com", "Test", "Testtown", Gender.Male, new Java.Lang.Integer(100), new String[]{"Cats", "Dogs"}, "06/17/1987");
Appboy.GetInstance(context).CurrentUser.SetFacebookData(facebookUser);

// Record Twitter Data
TwitterUser twitterUser = new TwitterUser(6253282, "Test", "User", "Tester",  new Java.Lang.Integer(100), new Java.Lang.Integer(100), new Java.Lang.Integer(100), "https://si0.twimg.com/profile_images/2685532587/fa47382ad67a0135acc62d4c6b49dbdc_bigger.jpeg");
Appboy.GetInstance(context).CurrentUser.SetTwitterData(twitterUser);
```

See [the Android integration instructions][6] for an in-depth discussion of social data best practices and interfaces.

[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/social_data_tracking/
[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/setting_user_ids/
[4]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/logging_purchases/#logging-purchases
[3]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/location_tracking/#location-tracking
[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/setting_custom_attributes/

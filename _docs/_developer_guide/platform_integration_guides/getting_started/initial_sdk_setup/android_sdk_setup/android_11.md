---
nav_title: Android 11 Upgrade Guide
page_order: 1
platform: Android
description: "This reference article covers the Android 11 SDK update, highlighting changes such as deep linking, SDK compatibility, and more."

---

# Android 11 SDK Upgrade Guide

This guide describes relevant changes introduced in Android 11 (released September 8, 2020) and the required upgrade steps for your Braze Android SDK integration.

For a full migration guide of Android 11, see the [Android Developer Documentation](https://developer.android.com/preview/migration).

## Braze SDK Compatibility

All apps that _target_ Android 11 (API 30) must upgrade to [Braze Android SDK v8.1.0+][1] in order to continue using Braze messaging features.

{% alert important %}
Due to changes in Android 11’s APIs, apps targeting Android 11 that do not upgrade to [Braze Android SDK v8.1.0+][1] will experience issues with deep linking from Braze UI components, and will not properly display custom HTML In-App Messages.
{% endalert %}

### Deep Links

Apps targeting Android 11 or above (API Version 30+) must upgrade to [Braze Android SDK v8.1.0][1] to continue using deep links within Braze messages. Due to a change in Android 11 APIs, apps that do not upgrade to at least Android SDK v8.1.0 will experience issues with deep links within Braze messages (In-App Messages or Content Cards).

### HTML In-App Messages

Apps targeting Android 11 or above (API Version 30+) must upgrade to Braze Android SDK v8.1.0 to continue using custom HTML In-App Messages. Due to a change in Android 11 WebView settings, HTML In-App Messages will not properly display on Android 11 targeted apps until upgrading to [Braze Android SDK v8.1.0][1]. 

### Location Permissions

Apps using location permissions should follow Android's [Best Practices](https://developer.android.com/preview/privacy/location#change-details) when requesting location access. No changes to your Braze integration are necessary for these location updates.

## Android 11 Behavior Changes

### Allow Once Permissions

Users can now grant permissions, such as Location Collection, on a one-time basis (see the [Android Docs](https://developer.android.com/preview/privacy/location#one-time-access) for more information). Once an app is closed, or in the background for long enough, that permission will be revoked automatically. The app would need to re-request this permission when needed in the future. Apps that already follow the recommended flow for requesting permissions for location will already support one-time permissions.

![Android Allow Once Permission][3]{: height="230px" }

### Background Location Permission

Android 11 will require apps to first request the foreground location permission, and then after the app is backgrounded it may prompt the user again for Background Location permission. 
Customers using Geofences should ensure their app follows Android’s recommendations on collecting Background Location permission. For more information, see the [Android Docs](https://developer.android.com/preview/privacy/location#background-location).

[1]: https://github.com/Appboy/appboy-android-sdk/blob/master/CHANGELOG.md#810
[3]: {% image_buster /assets/img/android/android-11-one-time-permission.svg %}

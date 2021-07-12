---
nav_title: Disabling Android SDK Tracking
platform: Android
page_order: 8
description: "This article shows how to disable data collection for your Android application."

---

# Disabling Data Collection

In order to comply with data privacy regulations, data tracking activity on the Android SDK can be stopped entirely using the method [`disableSDK()`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#disableSdk-android.content.Context-). This method will cause all network connections to be canceled, and the Braze SDK will not pass any data to Braze's servers. If you wish to resume data collection at a later point in time, you can use the [`enableSDK()`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#enableSdk-android.content.Context-) method in the future to resume data collection.

Additionally, you can use the method [`wipeData()`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#wipeData-android.content.Context-) to fully clear all client-side data stored on the device.

---
nav_title: AndroidX Integration
platform: Android
page_order: 3
description: "This article covers how to integrate AndroidX."

---

# AndroidX
[AndroidX][1] is the project that Android uses to release libraries within [Jetpack][2].

## Braze AndroidX Module
An AndroidX compatible module is published alongside the main Braze SDK, starting in version 3.0.0.

This module is used via:

```gradle
dependencies {
  ...
  implementation "com.appboy:android-sdk-ui-x:${LATEST_SDK_VERSION}"
}
```

{% alert important %}
If including `android-sdk-ui-x` in your `build.gradle`, you can remove `android-sdk-ui` from your `build.gradle`.
{% endalert %}

## Gradle Changes
The following gradle properties are required when using AndroidX libraries with the Braze SDK:

```gradle
android.enableJetifier=true
android.useAndroidX=true
```

## SDK Migration
Braze SDK imports should be migrated from the `android-sdk-ui` package to their `android-sdk-ui-x` package counterparts, where applicable. Only a subset of packages required AndroidX compatibility. For example, `com.appboy.ui.push` classes should be moved to `com.appboy.uix.push`. See the [Braze SDK Changelog][3] for a complete list of packages.

[1]: https://developer.android.com/jetpack/androidx/
[2]: https://developer.android.com/jetpack/
[3]: https://github.com/Appboy/appboy-android-sdk/blob/master/CHANGELOG.md

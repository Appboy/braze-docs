---
nav_title: Android SDK Integration
page_order: 0
platform: Android
description: "This reference article covers how to integrate the Android SDK into your Android application."

---

# Android SDK Integration

Installing the Braze SDK will provide you with basic analytics functionality as well as working in-app messages with which you can engage your users.

{% alert note %}
For apps targeting Android 11 (API 30) be sure to view our [Android 11 Support]({{site.baseurl}}/developer_guide/platform_integration_guides/android/android_11/) article
{% endalert %}

## Step 1: Integrate the Braze Library

The Braze Android SDK can optionally be integrated without UI components. However, Content Cards, News Feed, and In-App Messaging will be rendered inoperable unless you pass the custom data to a UI solely of your design. Additionally, push notifications will not work because our push handling code is in the UI library. Please note that these UI elements are open source and fully customizable. We strongly recommend the integration of these features. Please refer to the [Braze user guide][2] for the benefits of using the Braze Content Cards, News Feed, and In-App Message UI.

### Basic Integration

To access Braze's messaging features, you must integrate the UI library. Please see the following Android Studio directions to integrate the UI library depending on your IDE:

#### Add Our Repository

In your top-level project `build.gradle`, add the following as repositories under `allprojects` -> `repositories`.

For example:

```gradle
allprojects {
  repositories {
    google()
    maven { url "https://appboy.github.io/appboy-android-sdk/sdk" }
  }
}
```

{% alert note %}
The Braze Android SDK uses AndroidX Jetpack dependencies as of SDK version 10.0.0.
{% endalert %}

Alternatively, you can directly find the artifact AAR files on our [maven repository][71].

#### Add Braze Dependency

Add the `android-sdk-ui` dependency to your app's `build.gradle`:

```gradle
dependencies {
  implementation "com.appboy:android-sdk-ui:+"
}
```

The example below shows where to place the dependency line in your `build.gradle`. Note that the version used in the example uses an old version. Please visit [Braze Android SDK Releases][60] for the most up-to-date version of the Braze Android SDK.

![MavenScreen2][32]

#### Perform Gradle Sync

Be sure to perform a Gradle Sync to build your project and incorporate the dependency additions noted above.

![GradleSync][38]

## Step 2: Configure the Braze SDK in braze.xml

{% alert note %}
Note that as of December 2019, custom endpoints are no longer given out, if you have a pre-existing custom endpoint, you may continue to use it. For a list of our available endpoints, <a href="{{site.baseurl}}/api/basics/#endpoints">click here</a>.
{% endalert %}

Now that the libraries have been integrated, you have to create an `braze.xml` file in your project's `res/values` folder. If you are on a specific data cluster or have a pre-existing custom endpoint, you need to specify the endpoint in your `braze.xml` file as well. The contents of that file should resemble the following code snippet: 

>  Be sure to substitute your [App Identifier API Key]({{site.baseurl}}/api/api_key/#the-app-identifier-api-key) found within the  **Settings** page of the Braze dashboard for `YOUR_APP_IDENTIFIER_API_KEY`. To find out your specific cluster or endpoint, please ask your Customer Success Manager or [open a support ticket][68].

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
<string name="com_appboy_api_key">YOU_APP_IDENTIFIER_API_KEY</string>
<string translatable="false" name="com_appboy_custom_endpoint">YOUR_CUSTOM_ENDPOINT_OR_CLUSTER</string>
</resources>
```

## Step 3: Add Required Permissions to Android Manifest
Now that you've added your API key, you need to add the following permissions to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

>  With the release of Android M, Android switched from an install-time to a runtime permissions model. However, both of these permissions are normal permissions and are granted automatically if listed in the app manifest. For more information, visit Android's [permission documentation][46].

## Step 4: Tracking User Sessions in Android

### Activity Lifecycle Callback Integration

Calls to `openSession()`, `closeSession()`,[`ensureSubscribedToInAppMessageEvents()`][64], and `InAppMessageManager` registration are optionally handled automatically.

#### Instructions
Add the following code to the `onCreate()` method of your Application class:

{% tabs %}
{% tab JAVA %}

```java
public class MyApplication extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    registerActivityLifecycleCallbacks(new AppboyLifecycleCallbackListener(sessionHandlingEnabled, inAppMessagingRegistrationEnabled));
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class MyApplication : Application() {
  override fun onCreate() {
    super.onCreate()
    registerActivityLifecycleCallbacks(AppboyLifecycleCallbackListener(sessionHandlingEnabled, inAppMessagingRegistrationEnabled))
  }
}
```

{% endtab %}
{% endtabs %}

The first argument instructs the listener to handle `openSession()` and `closeSession()` calls.
The second argument instructs the listener to handle `registerInAppMessageManager()` and `unregisterInAppMessageManager()` calls.

See the [javadoc][63] for more information. Please note that any non-standard manual session integration is not fully supported.

## Step 5: Custom Endpoint Setup {#step-5-optional-custom-endpoint-setup}

{% alert note %}
Note that as of December 2019, custom endpoints are no longer given out, if you have a pre-existing custom endpoint, you may continue to use it. For a list of our available endpoints, <a href="{{site.baseurl}}/api/basics/#endpoints">click here</a>.
{% endalert %}

Your Braze representative should have already advised you of the [correct endpoint]({{site.baseurl}}/user_guide/administrative/access_braze/sdk_endpoints/).

To update the default endpoint in your integration of the Braze SDKs please add the following code to your `braze.xml`:

```xml
<string translatable="false" name="com_appboy_custom_endpoint">YOUR_CUSTOM_ENDPOINT_OR_CLUSTER</string>
```

## Step 6: Enable Location Tracking

If you would like to enable Braze location collection, update your `braze.xml` file to include `com_appboy_enable_location_collection` and ensure its value is set to `true`.

```xml
<bool name="com_appboy_enable_location_collection">true</bool>
```

{% alert important %}
Starting with Braze Android SDK version 3.6.0 Braze location collection is disabled by default.
{% endalert %}

## SDK Integration Complete

Braze will now be able to collect [specified data from your application]({{site.baseurl}}/user_guide/data_and_analytics/user_data_collection/overview/) and your basic integration should be complete.

Please see the following sections in order to enable [custom event tracking]({{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/tracking_custom_events/#tracking-custom-events), [push messaging]({{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/), [Content Cards]({{site.baseurl}}/developer_guide/platform_integration_guides/android/content_cards/overview/) and the [complete suite]({{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/) of Braze features.

[2]: {{site.baseurl}}/user_guide/introduction/
[32]: {% image_buster /assets/img_archive/androidstudio2.png %}
[38]: {% image_buster /assets/img_archive/androidstudio3.png %}
[46]: https://developer.android.com/training/permissions/index.html
[60]: https://github.com/Appboy/appboy-android-sdk/releases
[63]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/AppboyLifecycleCallbackListener.html#AppboyLifecycleCallbackListener-boolean-boolean-
[64]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/AppboyInAppMessageManager.html#ensureSubscribedToInAppMessageEvents-android.content.Context-
[68]: {{site.baseurl}}/support_contact/
[71]: https://appboy.github.io/appboy-android-sdk/sdk/com/appboy/android-sdk-ui/

---
nav_title: SDK Integration
platform: Unity
subplatform: Android
page_order: 0
description: "This reference article covers the Android SDK integration for the Unity platform."

---

# SDK Integration

## Step 1: Choosing A Braze Unity Package

The Braze [`.unitypackage`][26] bundles native bindings for the Android and iOS platforms, along with a C# interface.

There are several Braze Unity packages available for download at [Braze Unity Releases Page][16]:
 
{% include archive/unity/unitypackage_descriptions.md%}

## Step 2: Importing a Braze Unity Package

1. In the Unity Editor, import the package into your Unity project by navigating to `Assets > Import Package > Custom Package`.
2. Click "Import".

Alternatively, follow the Unity instructions for [Importing Asset packages][26] for a more detailed guide on importing custom Unity packages. 

{% alert note %}
If you only wish to import the Android plugin, deselect the `Plugins/iOS` subdirectory when importing the Braze `.unitypackage`.
{% endalert %}

## Step 3: Updating your AndroidManifest.xml

Android Unity projects require an [`AndroidManifest.xml`][29] to be present to run the application. Additionally, Braze requires several additions to your [`AndroidManifest.xml`][29] in order to function.

### Part 1: Configuring the AndroidManifest.xml

If your app does not have an `AndroidManifest.xml`, you can use the following as a template. Otherwise, if you already have an `AndroidManifest.xml`, ensure that any missing sections below are added to your existing `AndroidManifest.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="REPLACE_WITH_YOUR_PACKAGE_NAME">

  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.INTERNET" />

  <application android:icon="@drawable/app_icon" 
               android:label="@string/app_name">

    <!-- Calls the necessary Braze methods to ensure that analytics are collected and that push notifications are properly forwarded to the Unity application. -->
    <activity android:name="com.appboy.unity.AppboyUnityPlayerActivity" 
      android:label="@string/app_name" 
      android:configChanges="fontScale|keyboard|keyboardHidden|locale|mnc|mcc|navigation|orientation|screenLayout|screenSize|smallestScreenSize|uiMode|touchscreen" 
      android:screenOrientation="sensor">
      <meta-data android:name="android.app.lib_name" android:value="unity" />
      <meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>

    <!-- A Braze specific FirebaseMessagingService used to handle push notifications. -->
    <service android:name="com.appboy.AppboyFirebaseMessagingService"
      android:exported="false">
      <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
      </intent-filter>
    </service>

    <!-- BroadcastReceiver used to forward certain Braze push notification events to Unity -->
    <receiver android:name="com.appboy.unity.AppboyUnityPushBroadcastReceiver" android:exported="false" >
      <intent-filter>
        <action android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.intent.APPBOY_PUSH_RECEIVED" />
        <action android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.intent.APPBOY_NOTIFICATION_OPENED" />
        <action android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.intent.APPBOY_PUSH_DELETED" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

> Your `AndroidManifest.xml` should exist under `Assets/Plugins/Android/AndroidManifest.xml`. Please see [the Unity AndroidManifest documentation][29] for more information.

> All Activity classes registered in your `AndroidManifest.xml` file should be fully integrated with the Braze Android SDK. If you add your own Activity class, you must follow [Braze's Unity Activity integration instructions][34] to ensure that analytics are being collected.

{% alert note %}
Your final `AndroidManifest.xml` should only contain a single Activity with `"android.intent.category.LAUNCHER"` present.
{% endalert %}

### Part 2: Finding your Package Name 

- Click File -> Build Settings -> Player Settings -> Android Tab
![Unity Package Name][2]

### Part 3: Make Replacements in the AndroidManifest

In your `AndroidManifest.xml`, all instances of `REPLACE_WITH_YOUR_PACKAGE_NAME` should be replaced with your `Package Name` from the previous step.

## Step 4: Add Gradle Dependencies {#unity-android-gradle-configuration}

The following dependencies are required:

```groovy
implementation "androidx.appcompat:appcompat:+"

// Both are required if using the default Content Cards Activity on Android
implementation "androidx.swiperefreshlayout:swiperefreshlayout:+"
implementation "androidx.recyclerview:recyclerview:+"
```

Examples on how to add these dependencies using Unity tools are provided below.

### Custom Gradle Template

[Custom Gradle Template][55]

```groovy
dependencies {
  implementation "androidx.appcompat:appcompat:+"
}
```

### External Dependency Manager for Unity

[External Dependency Manager for Unity][56]

```xml
<dependencies>
  <androidPackages>
    <androidPackage spec="androidx.appcompat:appcompat:+" />
  </androidPackages>
</dependencies>
```

## Step 5: Configure the SDK {#unity-static-configuration}

Braze provides a native Unity solution for automating the Unity Android integration. 

1. In the Unity Editor, open the Braze Configuration Settings by navigating to Braze > Braze Configuration.
2. Check the "Automate Unity Android Integration" box.
3. In the "Braze API Key" field, input your application's API key from the Braze Dashboard.

>  Your Braze API key can be found within the **Settings** page of the Braze dashboard. To find out your specific cluster or endpoint, please ask your Customer Success Manager or [open a support ticket][30].

{% alert note %}
This automatic integration should not be used in conjunction with a manually created `braze.xml` file since the configuration values may conflict during project building. If you require the use of a manual `braze.xml`, please disable the automatic integration.
{% endalert %}

## Step 6: Basic SDK Integration Complete

Braze should now be collecting data from your application and your basic integration should be complete.

## Push

See the [Push documentation][53] for information on integrating push.

## In-App Messages

See the [In-App Message documentation][50] for information on integrating in-app messages.

## Content Cards Integration

See the [Content Cards documentation][52] for information on integrating Content Cards.

## News Feed Integration

See the [News Feed documentation][51] for information on integrating the News Feed.

## Extending the SDK

To extend the SDK's behaviors, fork our [Braze Unity SDK Github project][1] and make your required changes.

To publish your modified code as a Unity package, see [Advanced Use Cases][54].

## Advanced Android Integration Options

### Extending Braze's Unity Player {#extending-braze-unity-player}

The example `AndroidManifest.xml` file provided has one Activity class registered, [`AppboyUnityPlayerActivity`][32]. This class is integrated with the Braze SDK and extends `UnityPlayerActivity` with session handling, in-app message registration, push notification analytics logging, and more. See [this documentation][33] for more information on extending the `UnityPlayerActivity` class.

If you are creating your own custom `UnityPlayerActivity` in a library or plugin project, you will need to extend Braze's `AppboyUnityPlayerActivity` to integrate your custom functionality with Braze.

>  Before beginning work on extending `AppboyUnityPlayerActivity`, follow our instructions for integrating Braze into your Unity project.

1. Add the Braze Android SDK as a dependency to your library or plugin project as described in the [Braze Android SDK integration instructions][14].
2. Integrate our Unity `.aar`, which contains Braze's Unity-specific functionality, to your Android library project you are building for Unity. The `appboy-unity.aar` is available from our [public repo][15]. Once our Unity library is successfully integrated, modify your `UnityPlayerActivity` to extend `AppboyUnityPlayerActivity`.
3. Export your library or plugin project and drop it into `/<your-project>/Assets/Plugins/Android` as normal. Do not include any Braze source code in your library or plugin as they will already be present in `/<your-project>/Assets/Plugins/Android`.
4. Edit your `/<your-project>/Assets/Plugins/Android/AndroidManifest.xml` to specify your `AppboyUnityPlayerActivity` subclass as the main activity.

You should now be able to package an `.apk` from the Unity IDE that is fully integrated with Braze and contains your custom `UnityPlayerActivity` functionality.

[1]: https://github.com/appboy/appboy-unity-sdk
[2]: {% image_buster /assets/img_archive/UnityPackageName.png %}
[14]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/
[15]: https://github.com/Appboy/appboy-unity-sdk/tree/master/Assets/Plugins/Android
[16]: https://github.com/Appboy/appboy-unity-sdk/releases
[26]: https://docs.unity3d.com/Manual/AssetPackages.html
[29]: https://docs.unity3d.com/Manual/android-manifest.html
[30]: {{site.baseurl}}/support_contact/
[32]: https://github.com/Appboy/appboy-android-sdk/blob/e67e09f785adeff075a5d7710e79f41ed3676a6a/android-sdk-unity/src/main/java/com/appboy/unity/AppboyUnityPlayerActivity.java
[33]: https://docs.unity3d.com/Manual/AndroidUnityPlayerActivity.html
[34]: #extending-braze-unity-player
[35]: https://docs.unity3d.com/ScriptReference/Android.IPostGenerateGradleAndroidProject.html
[50]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/in-app_messaging/
[51]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/news_feed/
[52]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/content_cards/
[53]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/android/push_notifications/
[54]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/Advanced_Use_Cases/advanced_use_cases
[55]: https://docs.unity3d.com/Manual/android-gradle-overview.html
[56]: https://github.com/googlesamples/unity-jar-resolver

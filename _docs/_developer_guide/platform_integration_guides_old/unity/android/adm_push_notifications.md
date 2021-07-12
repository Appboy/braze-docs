---
nav_title: Amazon Device Messaging Push Notifications
platform: Unity
subplatform: Android
page_order: 2
description: "This reference article covers the Amazon Android push notification integration for the Unity platform."
channel: push
---

# Integration

A push notification is an out-of-app alert that appears on the user's screen when an important update occurs. Push notifications are a valuable way to provide your users with time-sensitive and relevant content or to re-engage them with your app.

>  ADM (Amazon Device Messaging) is not supported on non-Amazon devices. In order to test Kindle Push you must have a FireOS device ([see Amazon Listing of supported devices][32]).

Check out [the Help section][8] for additional best practices.

Braze sends push notifications to Amazon devices using [Amazon Device Messaging (ADM)][14].

>  Amazon Device Messaging (ADM) is __only__ supported on Fire phones and tablets (except for Kindle Fire 1st Generation). You cannot test ADM messaging on a regular Android device.

## Step 1: Enable ADM

- Create an account with the [Amazon Apps & Games Developer Portal][10] if you have not already done so.
- Obtain OAuth credentials (Client ID and Client Secret) and an ADM API key by following the instructions in [Obtaining Amazon Device Messaging Credentials][11].
- Enable "Automatic ADM Registration Enabled" in the Unity Braze Configuration window.
- Alternatively, you may add the following line to your `res/values/braze.xml` file to enable ADM registration:

  ```xml
  <bool name="com_appboy_push_adm_messaging_registration_enabled">true</bool>
  ```

## Step 2: Update Unity AndroidManifest.xml

If your app does not have an `AndroidManifest.xml`, you can use the following as a template. Otherwise, if you already have an `AndroidManifest.xml`, ensure that any missing sections below are added to your existing `AndroidManifest.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="REPLACE_WITH_YOUR_PACKAGE_NAME">

  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.INTERNET" />
  <permission
    android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.permission.RECEIVE_ADM_MESSAGE"
    android:protectionLevel="signature" />
  <uses-permission android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.permission.RECEIVE_ADM_MESSAGE" />
  <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

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

    <!-- BroadcastReceiver used to forward certain Braze push notification events to Unity -->
    <receiver android:name="com.appboy.unity.AppboyUnityPushBroadcastReceiver" android:exported="false" >
      <intent-filter>
        <action android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.intent.APPBOY_PUSH_RECEIVED" />
        <action android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.intent.APPBOY_NOTIFICATION_OPENED" />
        <action android:name="REPLACE_WITH_YOUR_PACKAGE_NAME.intent.APPBOY_PUSH_DELETED" />
      </intent-filter>
    </receiver>
    <receiver android:name="com.appboy.AppboyAdmReceiver" android:permission="com.amazon.device.messaging.permission.SEND">
      <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <category android:name="REPLACE_WITH_YOUR_PACKAGE_NAME" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

## Step 3: Store Your ADM API Key

- Save your ADM API key to a file named `api_key.txt` and save it in your project's [`Assets/Plugins/Android/assets`][54] folder.
- For how to obtain an ADM API Key for your app, consult Amazon's documentation on [obtaining an ADM API Key][11].
- Amazon will not recognize your key if `api_key.txt` contains any white space characters, such as a trailing line break.

## Step 4: Add ADM Jar

The required ADM Jar file may be placed anywhere in your project according to the [Unity JAR documentation][53].

## Step 5: Add Client Secret and Client ID to Braze Dashboard

Lastly, you must add the Client Secret and Client ID you obtained in [Step 1][2] to the Braze dashboard's "Manage Settings" page as pictured below:

![FireOS Dashboard][34]

[2]: #step-1-enable-adm
[8]: {{site.baseurl}}/developer_guide/platform_integration_guides/fireos/push_notifications/troubleshooting/
[10]: https://developer.amazon.com/public
[11]: https://developer.amazon.com/public/apis/engage/device-messaging/tech-docs/02-obtaining-adm-credentials
[12]: https://developer.amazon.com/public/apis/engage/device-messaging/tech-docs/03-setting-up-adm
[14]: https://developer.amazon.com/public/apis/engage/device-messaging
[29]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/deep_linking/
[32]: https://developer.amazon.com/appsandservices/apis/engage/device-messaging/tech-docs/04-integrating-your-app-with-adm
[34]: {% image_buster /assets/img_archive/fire_os_dashboard.png %}
[37]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#registerAppboyPushMessages(java.lang.String) "registerAppboyPushMessages()"
[52]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/#custom-handling-push-receipts-opens-and-key-value-pairs
[53]: https://docs.unity3d.com/Manual/AndroidJARPlugins.html
[54]: https://docs.unity3d.com/Manual/AndroidAARPlugins.html

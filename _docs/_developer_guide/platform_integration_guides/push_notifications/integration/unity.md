---
nav_title: Unity
page_title: Unity Push Notification Integration
page_order: 4
description: ""
---

# Unity Push Notification Integration

A push notification is an out-of-app alert that appears on the user's screen when an important update occurs. Push notifications are a valuable way to provide your users with time-sensitive and relevant content or to re-engage them with your app.

{% tabs %}
{% tab Android %}
{% subtabs %}
{% subtab ADM %}

> These instructions are for integrating push notifications into your Unity app using Amazon Device Messaging (ADM).

Braze sends push notifications to Amazon devices using [Amazon Device Messaging (ADM)][14].

{% alert note %}
ADM (Amazon Device Messaging) is not supported on non-Amazon devices. In order to test Kindle Push you must have a FireOS device ([see Amazon Listing of supported devices](https://developer.amazon.com/docs/adm/integrate-your-app.html)).
{% endalert %}

Check out [the Help section][8] for additional best practices.

### Step 1: Enable ADM

- Create an account with the [Amazon Apps & Games Developer Portal][10] if you have not already done so.
- Obtain OAuth credentials (Client ID and Client Secret) and an ADM API key by following the instructions in [Obtaining Amazon Device Messaging Credentials][11].
- Enable "Automatic ADM Registration Enabled" in the Unity Braze Configuration window.
- Alternatively, you may add the following line to your `res/values/braze.xml` file to enable ADM registration:

  ```xml
  <bool name="com_appboy_push_adm_messaging_registration_enabled">true</bool>
  ```

### Step 2: Update Unity AndroidManifest.xml

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

### Step 3: Store Your ADM API Key

- Save your ADM API key to a file named `api_key.txt` and save it in your project's [`Assets/Plugins/Android/assets`][54] folder.
- For how to obtain an ADM API Key for your app, consult Amazon's documentation on [obtaining an ADM API Key][11].
- Amazon will not recognize your key if `api_key.txt` contains any white space characters, such as a trailing line break.

### Step 4: Add ADM Jar

The required ADM Jar file may be placed anywhere in your project according to the [Unity JAR documentation][53].

### Step 5: Add Client Secret and Client ID to Braze Dashboard

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

{% endsubtab %}

{% subtab Firebase %}

> These instructions are for integrating push notifcations into your Unity app with [Firebase Cloud Messaging (FCM)][9].

### Step 1: Enable Firebase

To get started, follow the instructions at [Firebase Unity Setup Docs][11].

Note that integrating the Firebase Unity SDK may cause your `AndroidManifest.xml` to be overriden. If that occurs, make sure to revert it to its original self.

### Step 2: Set Your Firebase Credentials

You need to input your Firebase Server Key and Sender ID into the Braze dashboard:

* On the app settings page (where your API keys are located), select your Android app.
* Enter your Firebase Server Key in the field labeled "Firebase Cloud Messaging Server Key" under the Push Notification Settings section.
* Enter your Firebase Sender ID in the field labeled "Firebase Cloud Messaging Sender ID" under the Push Notification Settings section.

![FCMKey][15]

If you're not familiar with the location of your Firebase Server Key and Sender ID, follow these steps:

1. Login to the [Firebase Developers Console][58]
2. Select your Firebase project
3. Select Cloud Messaging under Settings and copy the Server Key and Sender ID:
![FirebaseServerKey][59]

### Step 3: Implement Automatic Push Integration

The Braze SDK can automatically handle push registration with the Firebase Cloud Messaging Servers in order to have devices receive push notifications.

![AndroidPushSettings][62]

- **"Automatic Firebase Cloud Messaging Registration Enabled":** Instructs the Braze SDK to automatically retrieve and send a FCM push token for a device. 
- **"Firebase Cloud Messaging Sender ID":** The Sender ID from your Firebase console.
- **"Handle Push Deeplinks Automatically":** Whether the SDK should handle opening deeplinks or the opening the app when push notifications are clicked.
- **"Small Notification Icon Drawable":** The drawable that should be displayed as the small icon whenever a push notification is received. If no icon is given, the notification will use the application icon as the small icon.

### Step 4: Set Push Listeners

If you would like to pass push notification payloads to Unity or take additional steps when a user receives a push notification, Braze provides the option of setting push notification listeners.

* On the **Settings** page (where your API keys are located), select your Android app.
* Enter your Firebase Server Key in the **Firebase Cloud Messaging Server Key** field, under the Push Notification Settings section.
* Enter your Firebase Sender ID in the **Firebase Cloud Messaging Sender ID** field, under the Push Notification Settings section.

#### Push Received Listener

The Push Received listener is fired when a user receives a push notification. To send the push payload to Unity, set the name of your Game Object and Push Received listener callback method under the "Set Push Received Listener".

#### Push Opened Listener

The Push Opened listener is fired when a user launches the app by clicking on a push notification. To send the push payload to Unity, set the name of your Game Object and Push Opened listener callback method under the "Set Push Opened Listener".

#### Push Deleted Listener (Android Only)

The Push Deleted listener is fired when a user swipes away or dismisses a push notification. To send the push payload to Unity, set the name of your Game Object and Push Deleted listener callback method under the "Set Push Deleted Listener".

#### Push Listener Implementation Example

The following example implements the `BrazeCallback` game object using a callback method name of `PushNotificationReceivedCallback`, `PushNotificationOpenedCallback`, and `PushNotificationDeletedCallback` respectively.

![Game Object Linking][63]

```csharp
public class MainMenu : MonoBehaviour {
  void PushNotificationReceivedCallback(string message) {
#if UNITY_ANDROID
    Debug.Log("PushNotificationReceivedCallback message: " + message);
    PushNotification pushNotification = new PushNotification(message);
    Debug.Log("Push Notification received: " + pushNotification);   
#elif UNITY_IOS
    ApplePushNotification pushNotification = new ApplePushNotification(message);
    Debug.Log("Push received Notification event: " + pushNotification);   
#endif  
  }

  void PushNotificationOpenedCallback(string message) {
#if UNITY_ANDROID
    Debug.Log("PushNotificationOpenedCallback message: " + message);
    PushNotification pushNotification = new PushNotification(message);
    Debug.Log("Push Notification opened: " + pushNotification);  
#elif UNITY_IOS
    ApplePushNotification pushNotification = new ApplePushNotification(message);
    Debug.Log("Push opened Notification event: " + pushNotification);   
#endif  
  }

  void PushNotificationDeletedCallback(string message) {
#if UNITY_ANDROID
    Debug.Log("PushNotificationDeletedCallback message: " + message);
    PushNotification pushNotification = new PushNotification(message);
    Debug.Log("Push Notification dismissed: " + pushNotification);  
#endif
  }
}
```

### Implementation Example

The sample project in the [Braze Unity SDK repository][13] contains a full working sample app that includes FCM.

### Deep Linking to In-App Resources

Although Braze can handle standard deep links (such as website urls, Android uris, etc.) out of the box, creating custom deep links requires additional Manifest setup.

See Android's documentation on ["Deep Linking" to In-App Resources][26]

[8]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/troubleshooting/
[9]: https://firebase.google.com/docs/cloud-messaging/
[11]: https://firebase.google.com/docs/unity/setup
[12]: https://firebase.google.com/docs/android/setup
[13]: https://github.com/Appboy/appboy-unity-sdk/tree/master/unity-samples
[15]: {% image_buster /assets/img_archive/fcm_api_insert.png %} "FCMKey"
[26]: https://developer.android.com/training/app-links/deep-linking
[58]: https://console.firebase.google.com/
[59]: {% image_buster /assets/img_archive/finding_firebase_server_key.png %} "FirebaseServerKey"
[61]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/advanced_use_cases
[62]: {% image_buster /assets/img/unity/android/unity_android_push_settings_config.png %} "Android Push Settings"
[63]: {% image_buster /assets/img/unity/android/unity_android_full_push_listener.png %} "Android Full Listener Example"
[64]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/android/adm_push_notifications/

{% endsubtab %}
{% endsubtabs %}
{% endtab %}

{% tab iOS %}

> These instructions are for integrating push notifications into your Unity iOS app.

### Step 1: Choose Automatic or Manual Push Integration

Braze provides a native Unity solution for automating iOS push integrations.

- If you would prefer instead to complete the integration manually by modifying your built Xcode project, please follow our [native iOS Push instructions][8].
- If you are transitioning from a manual integration to an automated one, follow the instructions on [Transitioning from Manual to Automated Integration][2].
- Our automatic push notification solution takes advantage of iOS 12's Provisional Authorization feature and is not available to use with the native push prompt pop-up.

### Step 2 (Optional): Implement Automatic Push Integration

#### Configure Push Notifications

Follow the instructions in our [iOS Push Notification Configuration documentation][8] to configure Braze using a `.p8` or `.p12` file.

#### Enable Automatic Push Integration

In the Unity Editor, open the Braze Configuration Settings by navigating to "Braze" > "Braze Configuration".

![enable push notification][24]

Check "Integrate Push With Braze" to automatically register users for push notifications, pass push tokens to Braze, track analytics for push opens, and take advantage of Braze's default push notification handling.

![Integrate Push With Braze][27]

#### (Optional): Enable Background Push

Check "Enable Background Push" if you would like to enable `background mode` for push notifications. This allows the system to wake your application from the `suspended` state when a push notification arrives, enabling your application to download content in response to push notifications. Checking this option is required for Braze's uninstall tracking functionality.

![Enabling Background Push][29]

#### (Optional): Disable Automatic Registration

Users who have not yet opted-in to push notifications will automatically be authorized for push upon opening your application. To disable this feature and manually register users for push, check "Disable Automatic Push Registration".

- If "Disable Provisional Authorization" is not checked, on iOS 12 and above, the user will be provisionally (silently) authorized to receive quiet push. If checked, the user will be shown the native push prompt.
- If you need to configure exactly when the prompt is shown at runtime, disable automatic registration from the Braze configuration editor and use `AppboyBinding.PromptUserForPushPermissions()` instead.

![Disable Automatic Push Registration][28]

### Step 3: Set Push Listeners

If you would like to pass push notification payloads to Unity or take additional steps when a user receives a push notification, Braze provides the option of setting push notification listeners.

#### Push Received Listener

The Push Received listener is fired when a user receives a push notification while they are actively using the application (i.e., the app is foregrounded). Set the push received listener in the Braze configuration editor.

![Push Received Listener][30]

- If you need to configure your game object listener at runtime, use `AppboyBinding.ConfigureListener()` and specify `BrazeUnityMessageType.PUSH_RECEIVED`.

#### Push Opened Listener

The Push Opened listener is fired when a user launches the app by clicking on a push notification. To send the push payload to Unity, set the name of your Game Object and Push Opened listener callback method under the "Set Push Opened Listener" foldout, like so:

![Push Opened Listener][31]


- If you need to configure your game object listener at runtime, use `AppboyBinding.ConfigureListener()` and specify `BrazeUnityMessageType.PUSH_OPENED`.

#### Push Listener Implementation Example

The following example implements the `AppboyCallback` game object using a callback method name of `PushNotificationReceivedCallback` and `PushNotificationOpenedCallback` respectively.

![Game Object Linking][32]

```csharp
public class MainMenu : MonoBehaviour {
  void PushNotificationReceivedCallback(string message) {
#if UNITY_ANDROID
    Debug.Log("PushNotificationReceivedCallback message: " + message);
    PushNotification pushNotification = new PushNotification(message);
    Debug.Log("Push Notification received: " + pushNotification);   
#elif UNITY_IOS
    ApplePushNotification pushNotification = new ApplePushNotification(message);
    Debug.Log("Push received Notification event: " + pushNotification);   
#endif  
  }

  void PushNotificationOpenedCallback(string message) {
#if UNITY_ANDROID
    Debug.Log("PushNotificationOpenedCallback message: " + message);
    PushNotification pushNotification = new PushNotification(message);
    Debug.Log("Push Notification opened: " + pushNotification);  
#elif UNITY_IOS
    ApplePushNotification pushNotification = new ApplePushNotification(message);
    Debug.Log("Push opened Notification event: " + pushNotification);   
#endif  
  }
}
```

### Advanced Features

#### Push Token Callback

To receive a copy of device tokens that Braze receives from the OS, set a delegate using `AppboyBinding.SetPushTokenReceivedFromSystemDelegate()`.

#### Other Features

To implement advanced features such as deep links, badge counts, and custom sounds, visit our [native iOS Push instructions][8].

[1]: #manual-push-integration
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/ios/sdk_integration/#transitioning-from-manual-to-automated-integration
[8]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/
[9]: https://developer.apple.com/ios/manage/overview/index.action "iOS Provisioning Portal"
[10]: {% image_buster /assets/img_archive/ios_provisioning.png %} "pushNotification2.png"
[11]: {% image_buster /assets/img_archive/AppleProvisioningOptions.png %} "AppleProvisioningOptions.png"
[12]: {% image_buster /assets/img_archive/push_cert_gen.png %} "pushNotification3.png"
[15]: https://github.com/Appboy/appboy-unity-sdk/blob/master/unity-samples/iOS/Roll-A-Ball-Ios/Classes/UnityAppController.mm "sample AppController.mm"
[24]: {% image_buster /assets/img_archive/Enable_push_capabilities.png %}
[27]: {% image_buster /assets/img/unity/ios/unity_ios_api_key.png %}
[28]: {% image_buster /assets/img/unity/ios/unity_ios_disable_auto_push.png %}
[29]: {% image_buster /assets/img/unity/ios/unity_ios_enable_background.png %}
[30]: {% image_buster /assets/img/unity/ios/unity_ios_push_received.png %}
[31]: {% image_buster /assets/img/unity/ios/unity_ios_push_opened.png %}
[32]: {% image_buster /assets/img/unity/ios/unity_ios_appboy_callback.png %}

{% endtab %}

{% endtabs %}
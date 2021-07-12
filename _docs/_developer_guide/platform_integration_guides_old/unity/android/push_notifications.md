---
nav_title: Firebase Push Notifications
platform: Unity
subplatform: Android
page_order: 1
description: "This reference article covers the Android push notification integration for the Unity platform."

---

# Push Notifications

These instructions are for integrating push with [Firebase Cloud Messaging (FCM)][9].

For ADM integration instructions, see our [Unity ADM][64] documentation.

## Step 1: Enable Firebase

To get started, follow the instructions at [Firebase Unity Setup Docs][11].

Note that integrating the Firebase Unity SDK may cause your `AndroidManifest.xml` to be overriden. If that occurs, make sure to revert it to its original self.

## Step 2: Set Your Firebase Credentials

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

## Step 3: Implement Automatic Push Integration

The Braze SDK can automatically handle push registration with the Firebase Cloud Messaging Servers in order to have devices receive push notifications.

![AndroidPushSettings][62]

- **"Automatic Firebase Cloud Messaging Registration Enabled"**

Instructs the Braze SDK to automatically retrieve and send a FCM push token for a device. 

- **"Firebase Cloud Messaging Sender ID"**

The Sender ID from your Firebase console.

- **"Handle Push Deeplinks Automatically"**

Whether the SDK should handle opening deeplinks or the opening the app when push notifications are clicked.

- **"Small Notification Icon Drawable"**

The drawable that should be displayed as the small icon whenever a push notification is received. If no icon is given, the notification will use the application icon as the small icon.

## Step 4: Set Push Listeners

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

## Deep Linking to In-App Resources

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

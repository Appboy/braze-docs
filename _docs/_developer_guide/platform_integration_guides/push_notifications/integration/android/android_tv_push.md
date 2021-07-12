---
nav_title: Android TV Push
page_title: Android TV Push Notification Integration
page_order: 2

platform: Android
description: "This article shows how to implement and test Android TV Push."
channel: push

---

# Android TV Push Notification Integration

![TV icon][12]{: style="float:right;max-width:25%;margin-left:15px; border: 0"}

While not a native feature, Android TV Push integration is made possible by leveraging the Braze Android SDK and Firebase Cloud Messaging to register a push token for Android TV. It is, however, necessary to build a UI to display the notification payload once it is received. The steps to do so are detailed below. 

## Implementation
- __Step 1: Integrate Braze Android SDK__<br>
First, you must integrate the [Braze Android SDK][6] (If not already completed).<br><br>
- __Step 2: Integrate Push Notifications__<br>
Next, you must integrate the [Android Push Notifications][10] (If not already completed).<br><br>
- __Step 3: Create Custom Toast View__<br>
Next, you will need to create a custom toast view for the app to display your notifications. Please refer to [Google Documentation][9] on how to do this.<br><br>
- __Step 4: Create a Custom Notification Factory__<br>
Lastly, you must create a custom notification factory. This will be used to override the default SDK behavior and allow you to manually display the notifications. By returning `null`, this will prevent the SDK from processing and will require custom code to display the notification. Please reference our [Braze documentation][8] that describes how to do this. 

Once these steps have been completed, you can start sending push to Android TV!

- __Step 5 (Optional): Set Up Click Analytics Tracking__<br>
To track click analytics effectively, it is necessary to handle this manually, as Braze does not handle the display of the messages automatically. This can be achieved by creating a custom `BroadcastReceiver` to listen for Braze push opened and received intents. Details on how to set this up can be found in the [documentation][7].

Note that these notifications __will not persist__ and will only be visible to the user at the point the device displays them. This is due to Android TV's notification center not supporting historical notifications. 

## How to Test Push on Android TV

To test if your push implementation is successful, send a notification from the Braze dashboard as you would normally for an Android device.

- __If Application is Closed__: The Push message will display a toast notification in the corner of the screen.
![Android TV Push Example][11]
- __If Application is Open__: You have the opportunity to display the message in your own hosted UI, we recommend following the UI styling of our Android Mobile SDK in-app messages.

## Additional Information
For a marketing end-user in Braze, launching a campaign to Android TV will be identical to launching a Push to Android Mobile apps, to target these devices exclusively, we recommend selecting the __Android TV App in segmentation__. 

The delivered/clicked response returned by FCM will follow the same convention as a mobile Android device, therefore, any errors will be visible in the Message Activity Log.

[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/?redirected=true
[7]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/#custom-handling-for-push-receipts-opens-dismissals-and-key-value-pairs
[8]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/#custom-displaying-notifications
[9]: https://developer.android.com/guide/topics/ui/notifiers/toasts#CustomToastView
[10]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/
[11]: {% image_buster /assets/img/android_tv.png %}
[12]: {% image_buster /assets/img/Television.png %}

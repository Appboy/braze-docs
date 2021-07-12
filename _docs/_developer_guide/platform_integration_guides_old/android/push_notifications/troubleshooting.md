---
nav_title: Troubleshooting
platform: Android
page_order: 16
description: "This article covers potential troubleshooting topics for your Android push implementation."
channel:
  - push

---

# Troubleshooting

## Understanding the Braze Workflow
The Firebase Cloud Messaging (FCM) service is Google's infrastructure for push notifications sent to Android applications. Here is the simplified structure of how push notifications are enabled for your users' devices and how Braze can send push notifications to them:

### Step 1: Configuring Your Google Cloud API Key
In the development of your app, you'll need to provide the Braze Android SDK with your Firebase Sender ID. Additionally, you'll need to provide an API Key for server applications to the Braze dashboard. Braze will use this API key when we attempt to send messages to your devices. You will need to ensure that FCM service is enabled in Google Developer's console as well. __Note__: A common mistake in this step is using an API key for Android applications. This is a different, incompatible API key for the type of access Braze needs.

### Step 2: Devices Register for FCM and Provide Braze with Push Tokens
In typical integrations, the Braze Android SDK will handle the process of registering devices for FCM capability. This will usually happen immediately upon opening the app for the first time. After registration, Braze will be provided with a FCM Registration ID, which is used to send messages to that device specifically. We will store the Registration ID for that user and that user will become "Push Registered" if they previously did not have a push token for any of your apps.

### Step 3: Launching a Braze Push Campaign
When a push campaign is launched, Braze will make requests to FCM to deliver your message. Braze will use the API key copied in the dashboard to authenticate and verify that we are allowed to send push notifications to the push tokens provided.

### Step 4: Removing Invalid Tokens
If FCM informs us that any of the push tokens we were attempting to send a message to are invalid, we remove those tokens from the user profiles they were associated with. If that user has no other push tokens, they will no longer show up as "Push Registered" under the Segments page.

Google has more details about FCM on their [Developers page][6].

## Utilizing the Push Error Logs
Braze provides a log of Push Notification Errors within the "Message Activity Log". This error log provides a variety of warnings which can be very helpful for identifying why your campaigns aren't working as expected.  Clicking on an error message will redirect you to relevant documentation to help you troubleshoot a particular incident.

![Push Error Log][11]

## Troubleshooting Scenarios

### No "Push Registered" Users Showing in the Braze Dashboard (Prior to Sending Messages)

Ensure that your app is correctly configured to allow push notifications. Common failure points to check include:

#### Incorrect Sender Id

Ensure that the correct FCM Sender ID is included in the `braze.xml` file. An incorrect Sender ID will lead to `MismatchSenderID` errors reported in the dashboard's Message Activity Log.

#### Braze Registration Not Occurring

Since FCM registration is handled outside of Braze, failure to register can only occur in two places:

1. During registration with FCM
2. When passing the FCM-generated push token to Braze

We recommend setting a breakpoint or logging to ensure that the FCM-generated push token is being sent to Braze. If a token is not being generated correctly or at all, we recommend consulting the [FCM documentation][1].

#### Google Play Services not present

For FCM push to work, Google Play Services must be present on the device. If Google Play Services isn't on a device, push registration will not occur.

__Note:__ Google Play Services is not installed on Android emulators without Google APIs installed.

#### Device not connected to the internet

Ensure your device has good internet connectivity and that it isn't sending network traffic through a proxy.

### Push Notifications Bounced

If a push notification isn't delivered, make sure it didn't bounce by looking in the [developer console][2]. The following are descriptions of common errors that may be logged in the developer console:

#### Error: MismatchSenderID

`MismatchSenderID` indicates an authentication failure. Ensure sure your Firebase Sender ID and FCM API key are correct. See our section on [debugging push registration][3] for more information.

#### Error: InvalidRegistration

`InvalidRegistration` can be caused by a malformed push token.

1. Make sure to pass a valid push token to Braze from Firebase Cloud Messaging [according to their documentation][21].

#### Error: NotRegistered

1. `NotRegistered` typically occurs when an app has been deleted from a device. Braze uses `NotRegistered` internally as a signal that an app has been uninstalled from a device.

2. `NotRegistered` may also occur when multiple registrations are occurring and a second registration is invalidating the first token.

### Push Notifications Sent But Not Displayed on Users' Devices

There are a few reasons why this could be occurring:

#### Application was Force Quit

If you force-quit your application through your system settings, your push notifications will not be sent. Launching the app again will re-enable your device to receive push notifications.

#### AppboyFirebaseMessagingService Not Registered

The AppboyFirebaseMessagingService must be properly registered in `AndroidManifest.xml` for push notifications to appear:

```xml
<service android:name="com.appboy.AppboyFirebaseMessagingService"
  android:exported="false">
  <intent-filter>
    <action android:name="com.google.firebase.MESSAGING_EVENT" />
  </intent-filter>
</service>
```

#### Firewall is Blocking Push

If you are testing push over Wi-Fi, your firewall may be blocking ports necessary for FCM to receive messages. Please ensure that ports 5228, 5229, and 5230 are open. Additionally, since FCM doesn't specify its IPs, you must also allow your firewall to accept outgoing connections to all IP addresses contained in the IP blocks listed in Google's ASN of 15169.

#### Custom Notification Factory Returning Null

If you have implemented a [custom notification factory][16], ensure that it is not returning `null`. This will cause notifications not to be displayed.

### "Push Registered" Users No Longer Enabled After Sending Messages

There are a few reasons why this could be happening:

#### Application was Uninstalled

Users have uninstalled the application. This will invalidate their FCM push token.

#### Invalid Firebase Cloud Messaging Server Key

The Firebase Cloud Messaging Server Key provided in the Braze dashboard is invalid. The Sender ID provided should match the one referenced in your app's `braze.xml` file. The Server key and Sender ID are found here in your Firebase Console:

![FirebaseServerKey][20]

### Push Clicks Not Logged

Braze logs push clicks automatically, so this scenario should be comparatively rare.

If push clicks are not being logged, it is possible that push click data has not been flushed to Braze's servers yet. Braze throttles the frequency of its flushes based on the strength of the network connection. With a good network connection, push click data should arrive at the server within a minute in most circumstances.

### Deep Links Not Working

#### Verify Deep Link configuration

Deep links can be [tested with ADB][17]. We recommend testing your deep link with the following command:

`adb shell am start -W -a android.intent.action.VIEW -d "THE_DEEP_LINK" THE_PACKAGE_NAME`

If the deep link fails to work, the deep link may be misconfigured. A misconfigured deep link will not work when sent through Braze push.

#### Verify Custom Handling Logic

If the deep link [works correctly with ADB][17] but fails to work from Braze push, check whether any [custom push open handling][18] has been implemented. If so, verify that the custom handling code is properly handling the incoming deep link.

[1]: https://firebase.google.com/docs/cloud-messaging/android/client
[2]: #utilizing-the-push-error-log
[3]: #scenario-1-no-push-registered-users-showing-in-the-appboy-dashboard-prior-to-sending-messages
[4]: https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId
[6]: https://firebase.google.com/docs/cloud-messaging/
[11]: {% image_buster /assets/img_archive/message_activity_log.png %}
[16]: #custom-displaying-notifications
[17]: https://developer.android.com/training/app-indexing/deep-linking.html#testing-filters
[18]: #custom-handling-push-receipts-and-opens
[20]: {% image_buster /assets/img_archive/finding_firebase_server_key.png %} "FirebaseServerKey"
[21]: https://firebase.google.com/docs/cloud-messaging/android/client#retrieve-the-current-registration-token

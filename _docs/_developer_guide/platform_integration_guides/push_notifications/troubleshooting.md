---
nav_title: Troubleshooting
page_order: 6
description: "This article covers potential troubleshooting topics for your push implementation."
---

# Troubleshooting Push Notifications

This article covers potential troubleshooting topics for your push implementation.

{% tabs %}
{% tab Android %}

### Understanding the Braze Workflow
The Firebase Cloud Messaging (FCM) service is Google's infrastructure for push notifications sent to Android applications. Here is the simplified structure of how push notifications are enabled for your users' devices and how Braze can send push notifications to them:

#### Step 1: Configuring Your Google Cloud API Key
In the development of your app, you'll need to provide the Braze Android SDK with your Firebase Sender ID. Additionally, you'll need to provide an API Key for server applications to the Braze dashboard. Braze will use this API key when we attempt to send messages to your devices. You will need to ensure that FCM service is enabled in Google Developer's console as well. 

{% alert tip %}
A common mistake in this step is using an API key for Android applications. This is a different, incompatible API key for the type of access Braze needs.
{% endalert %}

#### Step 2: Devices Register for FCM and Provide Braze with Push Tokens
In typical integrations, the Braze Android SDK will handle the process of registering devices for FCM capability. This will usually happen immediately upon opening the app for the first time. After registration, Braze will be provided with a FCM Registration ID, which is used to send messages to that device specifically. We will store the Registration ID for that user and that user will become "Push Registered" if they previously did not have a push token for any of your apps.

#### Step 3: Launching a Braze Push Campaign
When a push campaign is launched, Braze will make requests to FCM to deliver your message. Braze will use the API key copied in the dashboard to authenticate and verify that we are allowed to send push notifications to the push tokens provided.

#### Step 4: Removing Invalid Tokens
If FCM informs us that any of the push tokens we were attempting to send a message to are invalid, we remove those tokens from the user profiles they were associated with. If that user has no other push tokens, they will no longer show up as "Push Registered" under the Segments page.

Google has more details about FCM on their [Developers page][6].

### Utilizing the Push Error Logs
Braze provides a log of Push Notification Errors within the "Message Activity Log". This error log provides a variety of warnings which can be very helpful for identifying why your campaigns aren't working as expected.  Clicking on an error message will redirect you to relevant documentation to help you troubleshoot a particular incident.

![Push Error Log][11]

### Troubleshooting Scenarios

#### No "Push Registered" Users Showing in the Braze Dashboard (Prior to Sending Messages)

Ensure that your app is correctly configured to allow push notifications. Common failure points to check include:

| Issue | Troubleshooting Steps |
| --- | --- |
| Incorrect Sender ID | Ensure that the correct FCM Sender ID is included in the `braze.xml` file. An incorrect Sender ID will lead to `MismatchSenderID` errors reported in the dashboard's Message Activity Log. |
| Braze registration not occuring | Since FCM registration is handled outside of Braze, failure to register can only occur in two places: <br><br>1. During registration with FCM <br>2. When passing the FCM-generated push token to Braze<br><br>We recommend setting a breakpoint or logging to ensure that the FCM-generated push token is being sent to Braze. If a token is not being generated correctly or at all, we recommend consulting the [FCM documentation][1].|
| Google Play Services not present | For FCM push to work, Google Play Services must be present on the device. If Google Play Services isn't on a device, push registration will not occur.<br><br> **Note:** Google Play Services is not installed on Android emulators without Google APIs installed.|
| Device not connected to the internet | Ensure your device has good internet connectivity and that it isn't sending network traffic through a proxy. |
{: .reset-td-br-1 .reset-td-br-2}

#### Push Notifications Bounced

If a push notification isn't delivered, make sure it didn't bounce by looking in the [developer console][2]. The following are descriptions of common errors that may be logged in the developer console:

| Error | Troubleshooting Steps |
| --- | --- |
| `MismatchSenderID` | `MismatchSenderID` indicates an authentication failure. Ensure sure your Firebase Sender ID and FCM API key are correct. See our section on [debugging push registration][3] for more information. |
| `InvalidRegistration` | `InvalidRegistration` can be caused by a malformed push token.<br><br>Make sure to pass a valid push token to Braze from Firebase Cloud Messaging [according to their documentation][21]. |
| `NotRegistered` | `NotRegistered` typically occurs when an app has been deleted from a device. Braze uses `NotRegistered` internally as a signal that an app has been uninstalled from a device.<br><br>`NotRegistered` may also occur when multiple registrations are occurring and a second registration is invalidating the first token. |
{: .reset-td-br-1 .reset-td-br-2}


#### Push Notifications Sent But Not Displayed on Users' Devices

There are a few reasons why this could be occurring:

##### Application was Force Quit

If you force-quit your application through your system settings, your push notifications will not be sent. Launching the app again will re-enable your device to receive push notifications.

##### AppboyFirebaseMessagingService Not Registered

The AppboyFirebaseMessagingService must be properly registered in `AndroidManifest.xml` for push notifications to appear:

```xml
<service android:name="com.appboy.AppboyFirebaseMessagingService"
  android:exported="false">
  <intent-filter>
    <action android:name="com.google.firebase.MESSAGING_EVENT" />
  </intent-filter>
</service>
```

##### Firewall is Blocking Push

If you are testing push over Wi-Fi, your firewall may be blocking ports necessary for FCM to receive messages. Please ensure that ports 5228, 5229, and 5230 are open. Additionally, since FCM doesn't specify its IPs, you must also allow your firewall to accept outgoing connections to all IP addresses contained in the IP blocks listed in Google's ASN of 15169.

##### Custom Notification Factory Returning Null

If you have implemented a [custom notification factory][16], ensure that it is not returning `null`. This will cause notifications not to be displayed.

#### "Push Registered" Users No Longer Enabled After Sending Messages

There are a few reasons why this could be happening:

##### Application was Uninstalled

Users have uninstalled the application. This will invalidate their FCM push token.

##### Invalid Firebase Cloud Messaging Server Key

The Firebase Cloud Messaging Server Key provided in the Braze dashboard is invalid. The Sender ID provided should match the one referenced in your app's `braze.xml` file. The Server key and Sender ID are found here in your Firebase Console:

![FirebaseServerKey][20]

#### Push Clicks Not Logged

Braze logs push clicks automatically, so this scenario should be comparatively rare.

If push clicks are not being logged, it is possible that push click data has not been flushed to Braze's servers yet. Braze throttles the frequency of its flushes based on the strength of the network connection. With a good network connection, push click data should arrive at the server within a minute in most circumstances.

#### Deep Links Not Working

##### Verify Deep Link configuration

Deep links can be [tested with ADB][17]. We recommend testing your deep link with the following command:

`adb shell am start -W -a android.intent.action.VIEW -d "THE_DEEP_LINK" THE_PACKAGE_NAME`

If the deep link fails to work, the deep link may be misconfigured. A misconfigured deep link will not work when sent through Braze push.

##### Verify Custom Handling Logic

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

{% endtab %}

{% tab FireOS %}

### Utilizing the Push Error Logs

Braze provides a log of Push Notification Errors within the "Message Activity Log". This error log provides a variety of warnings which can be very helpful for identifying why your campaigns aren't working as expected.  Clicking on an error message will redirect you to relevant documentation to help you troubleshoot a particular incident.

![Push Error Log][11]

### Troubleshooting Scenarios

#### No "Push Registered" Users Showing in the Braze Dashboard (Prior to Sending Messages)
  - Ensure that your app is correctly configured to allow push notifications.
  - Ensure that the Client Id and Client Secret configured in your Braze dashboard are correct.

#### Push Notifications Not Displayed on Users' Devices
There are a few scenarios why this could be occurring:

  - If you force-quit your application, your push notifications will not be displayed while your app is not running.
  - Make sure the ["Notification Priority"][15] setting is set to "HIGH" in your campaign
  - The ADM API Key in your `api_key.txt` is incorrect or contins invalid characters.
  - The AppboyAdmReceiver is not properly registered in `AndroidManifest.xml` with intent filters for `<action android:name="com.amazon.device.messaging.intent.RECEIVE" />` and `<action android:name="com.amazon.device.messaging.intent.REGISTRATION" />`.

#### "Push Registered" Users No Longer Enabled After Sending Messages

  - This typically occurs when users have uninstalled the application, causing their ADM Registration ID to become invalid.

[11]: {% image_buster /assets/img_archive/message_activity_log.png %}
[15]: {{ site.baseurl }}/developer_guide/platform_integration_guides/fireos/push_notifications/advanced_settings/#notification-priority

{% endtab %}
{% tab iOS %}

### Understanding the Braze/APNs workflow

The Apple Push Notification service (APNs) is Apple's infrastructure for push notifications sending to iOS and OS X applications. Here is the simplified structure of how push notifications are enabled for your users' devices and how Braze is able to send push notifications to them:

#### Step 1: Configuring the push certificate and provisioning profile

In the development of your app, you'll need to create an SSL certificate to enable push notifications. This certificate will be included in the provisioning profile your app is built with and will also need to be uploaded to the Braze dashboard. The certificate allows Braze to tell APNs that we are allowed to send push notifications on your behalf.

There are two types of provisioning profiles and certificates - development and distribution. We recommend just using distribution profiles/certificates to avoid any confusion. If you choose to use different profiles and certificates for development and distribution, make sure that the certificate uploaded to the dashboard matches the provisioning profile you are currently using. You can read more about provisioning profiles [here][2].

#### Step 2: Devices register for APNs and provide Braze with push tokens

When users open your app, they will be prompted to accept push notifications. If they accept this prompt, then APNs will generate a push token for that particular device. The iOS SDK will immediately and asynchronously send up the push token for apps using the default [Automatic Flush Policy][40].  After we have a push token associated with a user, they will show as "Push Registered" in the dashboard on their user profile under the "Engagement" tab and will be eligible to receive push notifications from Braze campaigns.

> This does not work with the iOS Simulator. You cannot test push notifications with the iOS Simulator as a result.

#### Step 3: Launching a Braze push campaign

When a push campaign is launched, Braze will make requests to APNs to deliver your message. Braze will use the SSL push certificate uploaded in the dashboard to authenticate and verify that we are allowed to send push notifications to the push tokens provided. If a device is online, the notification should be received shortly after the campaign has been sent. 

{% alert note %}
Braze sets the default APNs [expiration date](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns#2947607) for notifications to 30 days.
{% endalert %}

#### Step 4: Removing invalid tokens
If APNs informs us that any of the push tokens we were attempting to send a message to are invalid, we remove those tokens from the user profiles they were associated with.

Apple has more details about APNs in their [Developer Library][20].

### Utilizing the Push Error Logs
Braze provides a log of Push Notification Errors within the [Message Activity Log][27]. This error log provides a variety of warnings which can be very helpful for identifying why your campaigns aren't working as expected.  Clicking on an error message will redirect you to relevant documentation to help you troubleshoot a particular incident.

![Push Error Log][26]

Common errors you might see here include user-specific notifications, such as ["Received Unregistered Sending to Push Token"][35].

In addition, Braze also provides a Push Changelog on the user profile under the Engagement tab. This changelog provides insight into push registration behavior such as token invalidation, push registration errors, tokens being moved to new users, etc.

![Push Changelog][1]

### Push Registration Issues

#### No Push Registration Prompt

If the application does not prompt you to register for push notifications, there is likely an issue with your push registration integration. Make sure you have followed our [documentation][21] and correctly integrated our push registration. You can also set breakpoints in your code to ensure push registration code is running.

#### No "Push Registered" Users Showing in the Dashboard

  - Ensure that your app is prompting you to allow push notifications. Typically this prompt will appear upon your first open of the app, but it can be programmed to appear elsewhere. If it is not appearing where it should be, then the problem is likely with the basic configuration of your app's push capabilities.
    - Verify the steps under ["Enabling Message Channels" > "Push Notifications"][21] were successfully completed.
    - Make sure that the provisioning profile your app was built with includes permissions for push. Make sure that you're pulling down all of the available provisioning profiles from your Apple Developer account, as well. You can confirm this by navigating to "Preferences" > "Accounts" in Xcode (Command+,).

    ![Provisioning Profile Refresh Step 1][23]

    Click on the Apple ID you use for your developer account and then "View Details...". In the bottom left corner of the pane that opens up, click on the refresh icon.

    ![Provisioning Profile Refresh Step 2][24]
  - Make sure you have [properly enabled push capability][29] in your app.
  - Make sure your push provisioning profile matches the environment you're testing in. Universal certificates may be configured in the Braze dashboard to send to either the development or production APNs environment. Using a development certificate for a production app or a production certificate for a development app will not work.
  - Ensure that you are calling our `registerPushToken` method by setting a breakpoint in your code.
  - Ensure that you are on a device (push will not work on a simulator) and have good network connectivity.

### Devices Not Receiving Push Notifications

#### Users No Longer "Push Registered" After Sending a Push Notification

This would likely indicate that user had an invalid push token. This can happen for several reasons:

##### Dashboard/App Certificate Mismatch

If the push certificate that you uploaded in the dashboard is not the same one in the provisioning profile that your app was built with, APNs will reject the token. Verify that you have uploaded the correct certificate and complete another session in the app before attempting another test notification.

##### Uninstalls

If a user has uninstalled your application, then their push token will be invalid and removed upon the next send.

##### Regenerating Your Provisioning Profile

As a last resort, starting over fresh and creating a whole new provisioning profile can clear up configuration errors that come from working with multiple environments, profiles and apps at the same time. There are many "moving parts" in setting up push notifications for iOS apps, so sometimes it is best to retry from the beginning. This will also help isolate the problem if you need to continue troubleshooting.

#### Users Still "Push Registered" After Sending a Push Notification

##### App Is Foregrounded

On iOS versions that do not integrate push via the UserNotifications framework, if the app is in the foreground when the push message is received it will not be displayed. You should background the app on your test devices before sending test messages.

##### Test Notification Scheduled Incorrectly

Check the schedule you set for your test message. If it is set to local time zone delivery or [Intelligent Timing]({{site.baseurl}}/user_guide/intelligence/intelligent_timing/), you may have just not received the message yet (or had the app in the foreground when it was received).

#### User Not "Push Registered" For the App Being Tested

Check the user profile of the user you are trying to send a test message to. Under the "Engagement" tab, there should be a list of "Pushable Apps". Verify the app you are trying to send test messages to is in this list. Users will show up as "Push Registered" if they have a push token for any app in your app group, so this could be something of a false positive.

The following would indicate a problem with push registration, or that the user's token had been returned to Braze as invalid by APNs after being pushed:

![Push Problem][25]

### Message Activity Log Errors

#### Received Unregistered Sending to Push Token {#received-unregistered-sending}

- Make sure that the push token being sent to Braze from the method `[[Appboy sharedInstance] registerPushToken:]` is valid. You can look in the [Message Activity Log][27] to see the push token. It should look something like `6e407a9be8d07f0cdeb9e724733a89445f57a89ec890d63867c482a483506fa6`, a string about that length with a mix of letters and numbers. If your push token looks different, check your [code][37] for sending Braze the push tokens.
- Ensure that your push provisioning profile matches the environment you're testing in. Universal certificates may be configured in the Braze dashboard to send to either the development or production APNs environment. Using a development certificate for a production app or a production certificate for a development app will not work.
 - Check that the push token you have uploaded to Braze matches the provisioning profile you used to build the app you sent the push token from.

#### Device Token Not For Topic

 This error indicates that the push certificate and bundle ID for your app are mismatched. Check that the push certificate you have uploaded to Braze matches the provisioning profile used to build the app that the push token was sent from.

### Issues After Push Delivery

#### Push Clicks Not Logged {#push-clicks-not-logged}

 - If this is only occurring on iOS 10, make sure you have followed the push integration steps for [iOS 10][30].
 - Braze does not handle push notifications received silently in the foreground (e.g., default foreground push behavior prior to the UserNotifications framework). This means that links will not be opened and push clicks will not be logged. If your application has not yet integrated the UserNotifications framework, Braze will not handle push notifications when the application state is UIApplicationStateActive. You should ensure that your app is not delaying calls to [Braze's push handling methods][30], otherwise, the iOS SDK may be treating push notifications as silent foreground push events and not handing them.

#### Web Links From Push Clicks Not Opening

iOS 9+ requires links be ATS compliant in order to be opened in web views. Ensure that your web links use HTTPS. For more information, refer to our [documentation on ATS compliance][38].

#### Deep Links From Push Clicks Not Opening

Most of the code that handles deep links also handles push opens.  First, ensure that push opens are being logged; if not, first [fix that issue][34] (as the fix often fixes link handling).

If opens are being logged, check to see if it is an issue with the deep link in general or with the deep linking push click handling.  To do this, test to see if a deep link from an In-App Message click works.

[1]: {% image_buster /assets/img_archive/push_changelog.png %}
[20]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1
[21]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/
[23]: {% image_buster /assets/img_archive/prov_profile_refresh_step1.png %}
[24]: {% image_buster /assets/img_archive/prov_profile_refresh_step2.png %}
[25]: {% image_buster /assets/img_archive/RegistrationProblem.png %}
[26]: {% image_buster /assets/img_archive/message_activity_log.png %}
[27]: https://dashboard-01.braze.com/app_settings/developer_console/activitylog/
[14]: https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns#2947607
[2]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingProfiles/MaintainingProfiles.html
[29]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/#step-2-enable-push-capabilities
[30]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/#step-5-enable-push-handling
[34]: #push-clicks-not-logged
[35]: #received-unregistered-sending
[37]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/#step-4-register-push-tokens-with-braze
[38]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/advanced_use_cases/linking/#app-transport-security-ats
[40]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/advanced_use_cases/fine_network_traffic_control/#automatic-request-processing

{% endtab %}
{% tab Xamarin %}

### Push Doesn't Appear After App is Closed from Task Switcher

If you observe that push notifications no longer appear after the app is closed from the task switcher, your app is likely in Debug mode. Xamarin adds scaffolding in Debug mode that prevents apps from receiving push after their process is killed. If you run your app in Release Mode, you should see push even after the app is closed from the task switcher.

{% endtab %}
{% endtabs %}
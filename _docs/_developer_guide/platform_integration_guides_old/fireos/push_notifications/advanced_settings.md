---
nav_title: Advanced Settings
platform: FireOS
page_order: 4

page_type: reference
description: "This article covers advanced settings available for Android and FireOS push notifications sent through the Braze dashboard."
channel: push

---

# Advanced Settings

There are many advanced settings available for Android and Fire OS push notifications sent through the Braze dashboard. This article will describe these features and how to use them successfully.

![Advanced Settings][1]

## Delivery Options

Both Google and Amazon's push messaging services (FCM and ADM, respectively) allow you to set additional parameters for when and how your messages are delivered. FCM and ADM both follow roughly the same pattern in how these options are structured and utilized. The default behavior of these services is to send messages to devices as soon as they are received for delivery. However, depending on the use case, this may not be the best way to communicate to your users. To provide you with precise control over when your push messages are delivered, we have implemented ways to use FCM and ADM delivery options with Braze campaigns.

For example, say that a user of a sports news app had their device offline the duration of a football game. The app sends out updates after one of the competing teams score, for a total of 10 push notifications over that duration. When the user reconnects to the messaging service, we would not want them to receive 10 redundant notifications. We have a few tools available to solve this problem.

- The __"Delay While Idle"__ value (Android only) specifies whether or not you'd like to delay sending the message until a user's device is online. This can be used to ensure delivery of all of your push notifications, even if the end user is offline. When used in conjunction with a unique "Collapse Key", "Delay While Idle" can prevent sending outdated information and clogging up the user's notification inbox.
- A __"Collapse Key"__ is a unique identifier for a message category of your choosing that informs the messaging service to only respect the most recent message from that ID. Setting a collapse key allows you to send just the most recent and relevant message, rather than a stack of outdated, irrelevant ones. However, if the phone correctly receives the original push notifications, then the collapse key will not replace the already delivered notification.
- The __"Time to Live"__ field allows you to set a custom length of time to store messages with the push messaging service. Braze's default values for time to live are 4 weeks for FCM and 31 days for ADM. If the hypothetical user from the example above were to reconnect their device 4 weeks after the game with the time to live set to the default, then those messages would have already expired in the messaging service and would not be delivered.

> FCM has a limit of 4 collapse keys per app. If you use more than 4 collapse keys, FCM does not make any guarantees as to which ones will be respected. Braze uses 2 of these by default for campaigns, so make sure to specify only up to 2 additional collapse keys for Android messages.

For more information on these delivery options, please see the [Firebase Cloud Messaging][7] and [Amazon Device Messaging][8] documentation.

## Summary Text

Summary text allows you to set additional text in the "Expanded Notification" view. It also serves as a caption for notifications with images.

![Summary Text Example][9]

The summary text will display below the body of the message in the expanded view.

For push notifications that include images, the message text will be shown in the collapsed view, while the summary text will be displayed as the image caption when the notification is expanded. See the animation below for an example of this behavior.

![Summary Text Behavior][15]

## Custom URIs

The __"Custom URI"__ feature allows you to specify a Web URL or an Android resource to navigate to when the notification is clicked. If no custom URI is specified, clicking on the notification brings users into your app. You can use the custom URI to deep link inside your app as well as direct users to resources that exist outside of your app as well. This can be specified via our [Messaging API][13] or via our dashboard under "Advanced Settings" in the push composer wizard as pictured below:

> To enable Custom URI, your app's `BroadcastReceiver` must be configured to properly handle opening the URI.  This involves parsing incoming message contents for the custom URI and navigating to it.  Our [example receiver][14] provides a sample implementation.

![Custom URI][12]


## Notification Priority

The priority level of a push notification affects how your notification is displayed in the notification tray relative to other notifications. It can also affect the speed and manner of delivery, as normal and lower priority messages may be sent with slightly higher latency or batched to preserve battery life whereas high priority messages are always sent immediately.

This feature is useful for differentiating your messages based on how critical or time-sensitive they are. For example, a notification about dangerous road conditions would be a good candidate to receive a high priority, while a notification about an ongoing sale should receive a lower priority. You should consider whether or not using a disruptive priority is actually necessary for the notification that you are sending as constantly taking the top spot in your users' inbox or interrupting their other activities may have a negative impact.

In Android O, notification priority became a property of notification channels. You will need to work with your developer to define the priority for a channel during its configuration, and then use the dashboard to select the proper channel when sending your notification sounds. For devices running versions of Android before O, specifying a priority level for Android and Fire OS notifications is possible via the Braze dashboard and Messaging API. 

To message your full userbase with a specific priority, we recommend that you indirectly specify the priority through [notification channel configuration][17] (to target O+ devices) _and_ send the individual priority from the dashboard (to target &#60;O devices).

The priority levels that you can set on Android or Fire OS push notifications are:

| Priority | Description/Intended Use | `priority` value (for API messages) |
|----------|-------------------|-------------------------------------|
| Max      | Urgent or time-critical messages | `2` |
| High     | Important communication, such as a new message from a friend | `1`|
| Default  | Most notifications - use if your message doesn't explicitly fall under any of the other priority types | `0`  |
| Low      | Information that you want users to know about, but does not require immediate action | `-1` |
| Min      | Contextual or background information. | `-2` |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

For more information, please consult [Google's documentation on Android notifications][2].

## Sounds

In Android O, notification sounds became a property of notification channels. You will need to work with your developer to define the sound for a channel during its configuration, and then use the dashboard to select the proper channel when sending your notifications.

For devices running versions of Android before O, Braze allows you to set the sound of an individual push message through the dashboard composer. You can do so by specifying a local sound resource on the device (e.g. `android.resource://com.mycompany.myapp/raw/mysound`). Specifying "default" in this field will play the default notification sound on the device. This can be specified via our [Messaging API][13] or via our dashboard under "Advanced Settings" in the push composer wizard as pictured below:

![Sounds][11]

Enter the full sound resource URI (e.g. `android.resource://com.mycompany.myapp/raw/mysound`) into the dashboard prompt.

To message your full userbase with a specific sound, we recommend that you indirectly specify the sound through [notification channel configuration][16] (to target O+ devices) *and* send the individual sound from the dashboard (to target <O devices).

[1]: {% image_buster /assets/img_archive/android_advanced_settings.png %}
[2]: http://developer.android.com/design/patterns/notifications.html
[7]: https://firebase.google.com/docs/cloud-messaging/concept-options
[8]: https://developer.amazon.com/public/apis/engage/device-messaging/tech-docs/06-sending-a-message
[9]: {% image_buster /assets/img_archive/summary_text.png %}
[11]: {% image_buster /assets/img_archive/sound_android.png %}
[12]: {% image_buster /assets/img_archive/deep_link_image_android.png %}
[13]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[14]: https://github.com/Appboy/appboy-android-sdk/blob/master/samples/custom-broadcast/src/main/java/com/appboy/custombroadcast/AppboyBroadcastReceiver.java
[15]: {% image_buster /assets/img_archive/messagesummary.gif %}
[17]: https://developer.android.com/training/notify-user/channels#importance
[16]: https://developer.android.com/training/notify-user/channels

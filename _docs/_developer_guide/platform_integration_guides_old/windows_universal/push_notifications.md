---
nav_title: Push Notifications
platform: Windows_Universal
page_order: 1
description: "This article covers push notification integration instructions for the windows universal platform."

---

# Push Notification Integration

![Sample Push][10]{: style="float:right;max-width:40%;margin-left:15px;"}

A push notification is an out-of-app alert that appears on the user's screen when an important update occurs. Push notifications are a valuable way to provide your users with time-sensitive and relevant content or to re-engage them with your app.

Visit our [documentation][9] for additional best practices.

## Step 1: Configure Your Application For Push

Ensure that in your `Package.appxmanifest` file, the following settings are configured as noted below:

Within the __Application__ tab, ensure that `Toast Capable` is set to `YES`.

## Step 2: Configure the Braze Dashboard

1. Find your SID and Client Secret - [Step By Step Instructions][4]
2. Within the __Settings__ page of the Braze dashboard, add the SID and Client Secret in your settings.

## Step 3: Update for Background Open Logging

In your `OnLaunched` method, after you have called `OpenSession` add the following code snippet.

```
string campaignId = e.Arguments.Split(new[] { "_ab_pn_cid" }, StringSplitOptions.None)[0];
if (!string.IsNullOrEmpty(campaignId))
{
Appboy.SharedInstance.PushManager.LogPushNotificationOpened(campaignId);          
}
```

![Windows SID Dashboard][6]

## Step 4: Creating Event Handlers

To listen to events that are fired when the push is received and activated (clicked by user), create event handlers and add them to the `PushManager` events:

- `Appboy.SharedInstance.PushManager.PushReceivedEvent += YourPushReceivedEventHandler;`
- `Appboy.SharedInstance.PushManager.ToastActivatedEvent += YourToastActivatedEventHandler;`

Your event handlers should have the signatures:

- `void YourPushReceivedEventHandler(PushNotificationChannel sender, AppboyPushNotificationReceivedEventArgs args);`
- `void YourToastActivatedEventHandler(ToastNotification sender, AppboyToastActivatedEventArgs args);`

## Step 5: Deep Linking From Push Into Your App

### Part 1: Creating deep links for your app

Deep links are used to navigate users from outside your application directly to a certain screen or page in your application. Typically this is done by registering a URL scheme (e.g. myapp://mypage) with an operating system and registering your application to handle that scheme; when the OS is asked to open a URL of that format it transfers control to your application.

WNS deep link support differs from this as it launches your application with data about where to send the user. When WNS push is created it can include a launch string that is passed through to your application's `OnLaunched` when the push is clicked and your application is opened. We already use this launch string to do campaign tracking, and we give users the ability to append their own data that can be parsed and used to navigate the user when the app is launched.

If you specify an extra launch string in the Dashboard or the REST API, it will be added to the end of the launch string that we create, after the key "abextras=". So, an example launch string might look like `ab_cn_id=_trackingid_abextras=page=settings`, in which you specified `page=settings` in the extra launch string parameter so you can parse it and navigate the user to the settings page.

### Part 2: Deep Linking Through the Dashboard

Specify the string to be appended to the launch string in the “Additional Launch String Configuration” field in push notification settings.

![Deep_Link_Dash_Example][15]

### Part 3: Deep Linking through the REST API

Braze also allows sending deep links through the REST API. Windows Universal Push objects accept an optional `extra_launch_string` parameter. See the [Windows Universal Push Object Example.][13]

[4]: http://msdn.microsoft.com/en-us/library/windows/apps/hh465407.aspx
[6]: {% image_buster /assets/img_archive/windows_sid.png %} "Windows SID Dashboard"
[9]: {{site.baseurl}}/help/best_practices/push/
[10]: {% image_buster /assets/img_archive/windows_uni_push_sample.png %}
[13]: {{site.baseurl}}/developer_guide/rest_api/messaging/#windows-universal-push-object
[15]: {% image_buster /assets/img_archive/windows_deep_link_click_action.png %} "Deep Link Click Action"
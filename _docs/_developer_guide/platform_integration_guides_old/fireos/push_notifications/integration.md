---
nav_title: Integration
platform: FireOS
page_order: 0

page_type: solution
description: "This article walks you through how to integrate Braze push notifications with your FireOS app."
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
- Add the following line to your `res/values/braze.xml` file to enable ADM:

  ```xml
  <bool name="com_appboy_push_adm_messaging_registration_enabled">true</bool>
  ```

## Step 2: Update AndroidManifest.xml

- In your app's AndroidManifest.xml, add Amazon's namespace to the `<>manifest</>` tag.

  ```xml
  xmlns:amazon="http://schemas.amazon.com/apk/res/android"
  ```
- Declare permissions required to support ADM by adding `<>permission</>` and `<>uses-permission</>` elements after the `<>manifest</> element`.

  ```xml
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:amazon="http://schemas.amazon.com/apk/res/android"
    package="[YOUR PACKAGE NAME]"
    android:versionCode="1"
    android:versionName="1.0">

  <!-- This permission ensures that no other application can intercept your ADM messages. -->
  <permission
    android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
    android:protectionLevel="signature" />
  <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

   <!-- This permission allows your app access to receive push notifications from ADM. -->
  <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

  <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
  <uses-permission android:name="android.permission.WAKE_LOCK" />
    ...
  </manifest>
  ```

- Declare that your app uses the device's ADM feature and declare that your app is designed to remain functional without ADM present on the device (android:required="false") by adding an amazon:enable-feature element to the manifest's application element.  It is safe to set android:required to "false" because Braze ADM code degrades gracefully when ADM is not present on the device.

  ```xml
  ...
  <application
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme">

    <amazon:enable-feature android:name="com.amazon.device.messaging" android:required="false"/>
  ...
  ```
- Add intent filters to handle `REGISTRATION` and `RECEIVE` intents from ADM within your Braze broadcast receiver's `AndroidManifest.xml` file. Immediately after `amazon:enable-feature`, add the following elements:

  ```xml
  <receiver android:name="com.appboy.AppboyAdmReceiver" android:permission="com.amazon.device.messaging.permission.SEND">
    <intent-filter>
        <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
        <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
        <category android:name="com.yourapp.packagename" />
    </intent-filter>
  </receiver>
  ```

## Step 3: Store Your ADM API Key

- Save your ADM API key to a file named `api_key.txt` and save it in your project's `assets` folder.
- For how to obtain an ADM API Key for your app, consult Amazon's documentation on [obtaining an ADM API Key][11].
- Amazon will not recognize your key if `api_key.txt` contains any white space characters, such as a trailing line break.

## Step 4: Add Deep Links

#### Enabling Automatic Deep Link Opening

To enable Braze to automatically open your app and any deep links when a push notification is clicked, set `com_appboy_handle_push_deep_links_automatically` to `true` in your `braze.xml`:

```
<bool name="com_appboy_handle_push_deep_links_automatically">true</bool>
```

If you would like to custom handle deep links, you will need to create a `BroadcastReceiver` that listens for push received and opened intents from Braze. See our section on [Custom Handling Push Receipts and Opens][52] in the Android push documentation for more information.

## Step 5: Add Client Secret and Client ID to Braze Dashboard

Lastly, you must add the Client Secret and Client ID you obtained in [Step 1][2] to the Braze dashboard's "Manage Settings" page as pictured below:

![FireOS Dashboard][34]

## Manual Push Registration
If you need to handle ADM registration yourself, you should do the following:

- Within [braze.xml][12] add the following:

  ```xml
  <!-- This will disable automatic registration for ADM via the Braze SDK-->
  <bool name="com_appboy_push_adm_messaging_registration_enabled">false</bool>
  ```
- Use the [registerAppboyPushMessages()][37] method to pass your user's ADM `registration_id` to Braze:

  ```java
  Appboy.getInstance(context).registerAppboyPushMessages(registration_id);
  ```

>  Braze does not recommend using manual registration if possible.

## ADM Extras

Users may send custom key-value pairs with a Kindle push message as "extras" for ["Deep Linking"][29], tracking urls, etc.  Please note that unlike in Android push, Kindle push users may not use Braze reserved keys as keys when defining "extra" key-value pairs.

Reserved Keys Include:

- `_ab`
- `a`
- `cid`
- `p`
- `s`
- `t`
- `ttl`
- `uri`

If a Kindle reserved key is detected, Braze returns Status Code 400: Kindle Push Reserved Key Used.


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

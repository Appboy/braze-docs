---
nav_title: Standard Integration
platform: Android
page_order: 0
description: "This article covers how to integrate push notifications in your Android application."
channel:
  - push

---

# Overview

![Android Inline Image Push example]({% image_buster /assets/img/android/push/inline_image_push_android_1.png %}){: style="float:right;max-width:35%;margin-left:15px;border: 0;"}

A push notification is an out-of-app alert that appears on the user's screen when an important update occurs. Push notifications are a valuable way to provide your users with time-sensitive and relevant content or to re-engage them with your app.

Check out [our help documentation][8] for push best practices.

Braze sends push notifications to Android devices using [Firebase Cloud Messaging (FCM)][45].

Push notifications for Amazon FireOS use the Amazon Device Messaging (ADM) service rather than FCM. See the [FireOS Push Integration instructions][28] for details on enabling push notifications for FireOS apps.

For devices without Google services installed, Braze offers the option to send push through Baidu Cloud Push. Visit [Baidu Cloud Push instructions][50] for more details.

## Registering for Push

Use [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) to register for push. For a full sample of using Firebase with the Braze Android SDK, see our [Firebase Push sample app](https://github.com/Appboy/appboy-android-sdk/tree/master/samples/firebase-push).

{% alert update %}
Automatic GCM registration is unavailable through the Braze SDK as a result of Google's removal of support for GCM on May 29, 2019. If your app is currently supporting GCM, we advise that you speak to your development teams about transitioning to [Firebase from GCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm) as soon as possible.
{% endalert %}

### Step 1: Enable Firebase

To get started, follow the instructions at [Add Firebase to Your Android Project][49].

Next, add the Firebase Messaging dependency to your module's `build.gradle`:

```gradle
implementation "com.google.firebase:firebase-core:${FIREBASE_CORE_VERSION}"
implementation "com.google.firebase:firebase-messaging:${FIREBASE_PUSH_MESSAGING_VERSION}"
```

### Step 2: Configure Token Registration

Braze push notifications won't work until a Firebase Cloud Messaging token (FCM registration token) is registered. FCM registration tokens can either be registered by the Braze SDK automatically (recommended) or manually registered. Tokens can be manually registered using the [`Appboy.registerAppboyPushMessages()`][35] method.

> Make sure to use your Firebase Sender ID. This is a unique numerical value created when you create your Firebase project, available in the Cloud Messaging tab of the Firebase console Settings pane. The sender ID is used to identify each sender that can send messages to the client app.

##### Option 1: Automatic Registration

To automatically register FCM registration tokens, enable automatic Firebase registration and set a Firebase Cloud Messaging Sender ID.

In your `braze.xml`:

```xml
<bool translatable="false" name="com_appboy_firebase_cloud_messaging_registration_enabled">true</bool>
<string translatable="false" name="com_appboy_firebase_cloud_messaging_sender_id">your_fcm_sender_id_here</string>
```

Or in your [BrazeConfig][68]:

{% tabs %}
{% tab JAVA %}

```java
BrazeConfig brazeConfig = new BrazeConfig.Builder()
  .setIsFirebaseCloudMessagingRegistrationEnabled(true)
  .setFirebaseCloudMessagingSenderIdKey("YOUR FIREBASE SENDER ID HERE")
  .build();
Braze.configure(this, brazeConfig);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val brazeConfig = BrazeConfig.Builder()
    .setIsFirebaseCloudMessagingRegistrationEnabled(true)
    .setFirebaseCloudMessagingSenderIdKey("YOUR FIREBASE SENDER ID HERE")
    .build()
Braze.configure(this, brazeConfig)
```

{% endtab %}
{% endtabs %}

If using a Firebase automatic registration, you can skip the manual options below.

#### Option 2: Manual Registration

We recommended you call [`Appboy.registerAppboyPushMessages()`][35] from within your application [`onCreate()`][67] method to ensure that push tokens are reliably delivered to Braze.

See the snippet below for an example:

{% tabs %}
{% tab JAVA %}

```java
public class MyApplication extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    final Context applicationContext = this;
    FirebaseMessaging.getInstance().getToken().addOnCompleteListener(task -> {
      if (!task.isSuccessful()) {
        Log.w(TAG, "Exception while registering FCM token with Braze.", task.getException());
        return;
      }

      final String token = task.getResult();
      Appboy.getInstance(applicationContext).registerAppboyPushMessages(token);
    });
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class MyApplication: Application() {
  override fun onCreate() {
    super.onCreate()
    FirebaseMessaging.getInstance().token.addOnCompleteListener { task: Task<String?> ->
      if (!task.isSuccessful) {
        Log.w(TAG, "Exception while registering FCM token with Braze.", task.exception)
        return@addOnCompleteListener
      }
      val token = task.result
      Appboy.getInstance(applicationContext).registerAppboyPushMessages(token)
    }
  }
}
```

{% endtab %}
{% endtabs %}

> While we highly recommend you register your FCM registration token in your application [`onCreate()`][67], the token can be registered anywhere in your code.

### Step 3: Migrate from GCM (Optional)

If migrating from using GCM to using Firebase with Braze, visit the [GCM Migration Guide][48] for instructions on how to properly switch to using Firebase in your app.

### Step 4: Set Your Firebase Credentials

You need to input your Firebase Server Key and Sender ID into the Braze dashboard:

* On the **Settings** page (where your API keys are located), select your Android app.
* Enter your Firebase Server Key in the **Firebase Cloud Messaging Server Key** field, under the Push Notification Settings section.
* Enter your Firebase Sender ID in the **Firebase Cloud Messaging Sender ID** field, under the Push Notification Settings section.

![fcmimg][16]

If you're not familiar with the location of your Firebase Server Key and Sender ID, follow these steps:

1. Log in to the [Firebase Developers Console][58]

2. Select your Firebase project

3. Go to **Settings** > **Cloud Messaging** and copy the Server Key and Sender ID:
  ![FirebaseServerKey][59]

### Step 5: Remove Old Permissions
- Braze no longer requires the following permissions if using Firebase:

  ```xml
  <uses-permission android:name="android.permission.GET_ACCOUNTS" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <permission android:name="YOUR-APPLICATION-PACKAGE-NAME.permission.C2D_MESSAGE" android:protectionLevel="signature"/>
  <uses-permission android:name="YOUR-APPLICATION-PACKAGE-NAME.permission.C2D_MESSAGE" />
  ```

### Step 6: Remove Automatic Actions from your Application Class

If you have a custom [Application][76] subclass, ensure you do not have automatic logic that pings your servers in your class's `Application.onCreate()` lifecycle method. This will ensure that silent push notifications from Braze don't cause unnecessary requests to your servers.

## Displaying Push

After completing this section, you should be able to receive and display push notifications sent by Braze.

### Step 1: Register Braze Firebase Messaging Service
Braze includes a service to handle push receipt and open intents. Our `AppboyFirebaseMessagingService` class will need to be registered in your `AndroidManifest.xml`.

```xml
<service android:name="com.appboy.AppboyFirebaseMessagingService"
  android:exported="false">
  <intent-filter>
    <action android:name="com.google.firebase.MESSAGING_EVENT" />
  </intent-filter>
</service>
```

Braze's notification code also uses `AppboyFirebaseMessagingService` to handle open and click action tracking. This service must be registered in the `AndroidManifest.xml` for that to function correctly. Also, keep in mind that Braze prefixes notifications from our system with a unique key to ensure we only render notifications sent from Braze's systems. You may register additional services separately to render notifications sent from other FCM services.

{% alert important %}
If you already have a Firebase Messaging Service registered, do not complete this step. Instead, proceed to [Using Your Own Firebase Messaging Service](#using-your-own-firebase-messaging-service) and complete the steps listed there.
{% endalert %}

{% alert update %}
Before Braze SDK 3.1.1, `AppboyFcmReceiver` was used to handle FCM push. The `AppboyFcmReceiver` class should be removed from your manifest and replaced with the above integration.
{% endalert %}

**Implementation Example**

- See [`AndroidManifest.xml`][70] in the Firebase Push sample app.

##### Using Your Own Firebase Messaging Service

If you already have a Firebase Messaging Service registered, you can pass [`RemoteMessage`][75] objects to Braze via [AppboyFirebaseMessagingService.handleBrazeRemoteMessage()][74]. This method will only display a notification if the [`RemoteMessage`][75] object originated from Braze and will safely ignore if not.

{% tabs %}
{% tab JAVA %}

```java
public class MyFirebaseMessagingService extends FirebaseMessagingService {
  @Override
  public void onMessageReceived(RemoteMessage remoteMessage) {
    super.onMessageReceived(remoteMessage);
    if (AppboyFirebaseMessagingService.handleBrazeRemoteMessage(this, remoteMessage)) {
      // This Remote Message originated from Braze and a push notification was displayed.
      // No further action is needed.
    } else {
      // This Remote Message did not originate from Braze.
      // No action was taken and you can safely pass this Remote Message to other handlers.
    }
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class MyFirebaseMessagingService : FirebaseMessagingService() {
  override fun onMessageReceived(remoteMessage: RemoteMessage?) {
    super.onMessageReceived(remoteMessage)
    if (AppboyFirebaseMessagingService.handleBrazeRemoteMessage(this, remoteMessage)) {
      // This Remote Message originated from Braze and a push notification was displayed.
      // No further action is needed.
    } else {
      // This Remote Message did not originate from Braze.
      // No action was taken and you can safely pass this Remote Message to other handlers.
    }
  }
}
```

{% endtab %}
{% endtabs %}

### Step 2: Ensure Small Icons Conform to Design Guidelines

For general information about Android notification icons, please see the [Notifications Overview documentation][37].

Starting in Android N, you should update or remove small notification icon assets that involve color. The Android system (not the Braze SDK) ignores all non-alpha/transparency channels in action icons and the notification small icon. In other words, Android will convert all parts of your notification small icon to monochrome except for transparent regions.

To properly create a notification small icon asset:
- Remove all colors from the image except for white.
- All other non-white regions of the asset should be transparent. 

{% alert note %}
A common symptom of an improper asset is the notification small icon rendering as a solid monochrome square. This is due to the Android system not being able to find any transparent regions in the notification small icon asset.
{% endalert %}

The icons pictured below are examples of properly designed icons:

![Android Icon Example][38]

### Step 3: Configure Notification Icons

#### Specifying Icons in braze.xml

- Braze allows you to configure your notification icons by specifying drawable resources in your `braze.xml`:

```xml
<drawable name="com_appboy_push_small_notification_icon">REPLACE_WITH_YOUR_ICON</drawable>
<drawable name="com_appboy_push_large_notification_icon">REPLACE_WITH_YOUR_ICON</drawable>
```

Setting a small notification icon is required. __If you do not set one, Braze will default to using the application icon as the small notification icon which may look suboptimal.__

Setting a large notification icon is optional but recommended.

#### Specifying Icon Background Color

- The notification icon background color can be overriden in your `braze.xml`. If the color is not specified, the default background color is the same gray Lollipop uses for system notifications. Please see the example color override below:

```xml
<integer name="com_appboy_default_notification_accent_color">0xFFf33e3e</integer>
```

You may also optionally use a color reference, see:

```xml
<color name="com_appboy_default_notification_accent_color">@color/my_color_here</color>
```

### Step 4: Add Deep Links

#### Enabling Automatic Deep Link Opening

To enable Braze to automatically open your app and any deep links when a push notification is clicked, set `com_appboy_handle_push_deep_links_automatically` to its default setting,`true`, in your `braze.xml`:

```xml
<bool name="com_appboy_handle_push_deep_links_automatically">true</bool>
```

This flag can also be set via [runtime configuration][65]:

{% tabs %}
{% tab JAVA %}

```java
BrazeConfig brazeConfig = new BrazeConfig.Builder()
        .setHandlePushDeepLinksAutomatically(true)
        .build();
Braze.configure(this, brazeConfig);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val brazeConfig = BrazeConfig.Builder()
        .setHandlePushDeepLinksAutomatically(true)
        .build()
Braze.configure(this, brazeConfig)
```

{% endtab %}
{% endtabs %}

If you would like to custom handle deep links, you will need to create a `BroadcastReceiver` that listens for push received and opened intents from Braze. See our section on [Custom Handling Push Receipts and Opens][52] for more information.

#### Creating Custom Deep Links

Please follow the instructions found within the [Android Developer Documentation on Deep Linking][40] if you have not already added deep links to your app. For information regarding what a deep link is, please see our [FAQ Section][42].

#### Adding Deep Links

The Braze dashboard supports setting deep links or web URLs on push notifications that will be opened when the notification is clicked.

![Deep_Link_Dash_Example][41]

#### Customizing Back Stack Behavior

The Android SDK by default will place your host app's main launcher activity in the back stack when following push deep links. Braze allows you to set a custom activity to open in the back stack in place of your main launcher activity or to disable the back stack altogether.

For example, to set an activity called `YourMainActivity` as the back stack activity, see the following example using [runtime configuration][65]:

{% tabs %}
{% tab JAVA %}

```java
BrazeConfig brazeConfig = new BrazeConfig.Builder()
        .setPushDeepLinkBackStackActivityEnabled(true)
        .setPushDeepLinkBackStackActivityClass(YourMainActivity.class)
        .build();
Braze.configure(this, brazeConfig);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val brazeConfig = BrazeConfig.Builder()
        .setPushDeepLinkBackStackActivityEnabled(true)
        .setPushDeepLinkBackStackActivityClass(YourMainActivity.class)
        .build()
Braze.configure(this, brazeConfig)
```

{% endtab %}
{% endtabs %}

See the equivalent configuration for your `braze.xml`. Note that the class name must be the same as returned by `Class.forName()`:

```xml
<bool name="com_appboy_push_deep_link_back_stack_activity_enabled">true</bool>
<string name="com_appboy_push_deep_link_back_stack_activity_class_name">your.package.name.YourMainActivity</string>
```

### Step 5: Define Notification Channels

The Braze Android SDK supports [Android Notification Channels][62]. In the case that a Braze notification does not contain the ID for a notification channel or that a Braze notification contains an invalid channel ID, Braze will display the notification with the default notification channel defined in the SDK. Braze users make use of [Android Notification Channels][61] within the platform to group notifications.

To set the user facing name of the default Braze notification channel, please use [`BrazeConfig.setDefaultNotificationChannelName()`][72].

To set the user facing description of the default Braze notification channel, please use [`BrazeConfig.setDefaultNotificationChannelDescription()`][73].

You should ensure that any API campaigns with the [Android Push Object][63] parameter are updated to include the `notification_channel` field. If this field is not specified, Braze will send the notification payload with the [dashboard fallback][64] channel ID.

Other than the default notification channel, Braze will not create any channels. All other channels must be programmatically defined by the host app and then entered into the Braze dashboard.

### Step 6: Test Notification Display and Analytics

#### Testing Display

At this point, you should be able to see notifications sent from Braze.  To test this, go to the `Campaigns` section of your Braze dashboard and create a `Push Notification` campaign.  Choose `Android Push` and design your message.  Then click the eyeball in the composer to get the test sender.  Enter the user id or email address of your current user and click `Send Test`.  You should see the push show up on your device.

![Android push test][55]

For issues related to push display, see our [Troubleshooting Guide][57].

#### Testing Analytics

At this point, you should also have analytics logging for push notification opens.  To test this, see our [Docs on creating a push campaign][56].  Clicking on the notification when it arrives should result in the `Direct Opens` on your campaign results page to increase by 1.

For issues related to push analytics, see our [Troubleshooting Guide][57].

#### Testing From Command Line

If you'd like to test in-app and push notifications via the command-line, you can send a single notification through the terminal via cURL and the [Messaging API][22]. You will need to replace the following fields with the correct values for your test case:

- `YOUR_API_KEY` - available on the Developer Console page
- `YOUR_EXTERNAL_USER_ID` - available on the User Profile Search Page
- `YOUR_KEY1` (optional)
- `YOUR_VALUE1` (optional)

```
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer {{YOUR_API_KEY}}" -d "{\"external_user_ids\":[\"YOUR_EXTERNAL_USER_ID\"],\"messages\":{\"android_push\":{\"title\":\"Test push title\",\"alert\":\"Test push\",\"extra\":{\"YOUR_KEY1\":\"YOUR_VALUE1\"}}}}" https://rest.iad-01.braze.com/messages/send
```

The above is an example for customers on the `US-01` instance. If you are not on this instance please refer to our [API documentation][66] to see which endpoint to make requests to.

## Customizing Your Integration

### Custom Displaying Notifications

#### Step 1: Create your Custom Notification Factory

In some scenarios, you may wish to customize push notifications in ways that would be cumbersome or unavailable server side. To give you complete control of notification display, we've added the ability to define your own [`IAppboyNotificationFactory`][6] to create notification objects for display by Braze.

If a custom `IAppboyNotificationFactory` is set, Braze will call your factory's `createNotification()` method upon push receipt before the notification is displayed to the user. Braze will pass in a `Bundle` containing Braze push data and another `Bundle` containing custom key-value pairs sent either via the dashboard or the messaging APIs:

Braze will pass in a [`BrazeNotificationPayload`][77] containing data from the Braze push notification.

{% tabs %}
{% tab JAVA %}

```java
// Factory method implemented in your custom IAppboyNotificationFactory
@Override
public Notification createNotification(BrazeNotificationPayload brazeNotificationPayload) {
  // Example of getting notification title
  String title = brazeNotificationPayload.getTitleText();

  // Example of retrieving a custom KVP ("my_key" -> "my_value")
  String customKvp = brazeNotificationPayload.getAppboyExtras().getString("my_key");
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
// Factory method implemented in your custom IAppboyNotificationFactory
override fun createNotification(brazeNotificationPayload: BrazeNotificationPayload): Notification {
  // Example of getting notification title
  val title = brazeNotificationPayload.getTitleText()

  // Example of retrieving a custom KVP ("my_key" -> "my_value")
  val customKvp = brazeNotificationPayload.getAppboyExtras().getString("my_key")
}
```

{% endtab %}
{% endtabs %}

You can return `null` from your custom `createNotification()` method to not show the notification at all, use `AppboyNotificationFactory.getInstance().createNotification()` to obtain Braze's default `notification` object for that data and modify it before display, or generate a completely separate `notification` object for display.

{% alert note %}
Braze push data keys are documented [here](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Constants.html).
{% endalert %}

#### Step 2: Set your Custom Notification Factory

To instruct Braze to use your custom notification factory, use the [method on the Braze interface][5] to set your [`IAppboyNotificationFactory`][6]:

{% tabs %}
{% tab JAVA %}


```java
setCustomAppboyNotificationFactory(IAppboyNotificationFactory appboyNotificationFactory);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
setCustomAppboyNotificationFactory(appboyNotificationFactory: IAppboyNotificationFactory)
```

{% endtab %}
{% endtabs %}

The recommended place to set your custom `IAppboyNotificationFactory` is in the `Application.onCreate()` application lifecycle method (not activity).  This will allow the notification factory to be set correctly whenever your app process is active.

{% alert important %}
Creating your own notification from scratch is an advanced use case and should be done only with thorough testing and a deep understanding of Braze's push functionality (you must, for example, ensure your notification logs push opens correctly).
{% endalert %}

To unset your custom [`IAppboyNotificationFactory`][6] and return to default Braze handling for push, pass in `null` to our custom notification factory setter:

{% tabs %}
{% tab JAVA %}


```java
setCustomAppboyNotificationFactory(null);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
setCustomAppboyNotificationFactory(null)
```

{% endtab %}
{% endtabs %}

### Custom Handling For Push Receipts, Opens, Dismissals, and Key-Value Pairs

Braze broadcasts custom intents when push notifications are received, opened, or dismissed. If you have a specific use case for these scenarios (such as the need to listen for custom key-value pairs or proprietary handling of deep links), you will need to listen for these intents by creating a custom `BroadcastReceiver`.

#### Step 1: Register your BroadcastReceiver

Register your custom `BroadcastReceiver` to listen for Braze push opened and received intents in your [`AndroidManifest.xml`][71].

```xml
<receiver android:name="YOUR-BROADCASTRECEIVER-NAME" android:exported="false" >
  <intent-filter>
    <action android:name="${applicationId}.intent.APPBOY_PUSH_RECEIVED" />
    <action android:name="${applicationId}.intent.APPBOY_NOTIFICATION_OPENED" />
    <action android:name="${applicationId}.intent.APPBOY_PUSH_DELETED" />
  </intent-filter>
</receiver>
```

#### Step 2: Create Your BroadcastReceiver

Your receiver should handle intents broadcast by Braze and launch your activity with them:

- It should subclass [`BroadcastReceiver`][53] and override `onReceive()`.
- The `onReceive()` method should listen for intents broadcast by Braze.
  - An `APPBOY_PUSH_RECEIVED` intent will be received when a push notification arrives.
  - An `APPBOY_NOTIFICATION_OPENED` intent will be received when a push notification is clicked by the user.
  - An `APPBOY_PUSH_DELETED` intent will be received when a push notification is dismissed (swiped away) by the user.
- The receiver should perform your custom logic for each of these cases.  If your receiver will open deep links, be sure to turn off automatic deep link opening by setting `com_appboy_handle_push_deep_links_automatically` to `false` in your `braze.xml`.

For a detailed custom receiver example, please see the below:

{% tabs %}
{% tab JAVA %}

```java
public class CustomBroadcastReceiver extends BroadcastReceiver {
  private static final String TAG = CustomBroadcastReceiver.class.getName();

  @Override
  public void onReceive(Context context, Intent intent) {
    String packageName = context.getPackageName();
    String pushReceivedAction = packageName + AppboyNotificationUtils.APPBOY_NOTIFICATION_RECEIVED_SUFFIX;
    String notificationOpenedAction = packageName + AppboyNotificationUtils.APPBOY_NOTIFICATION_OPENED_SUFFIX;
    String notificationDeletedAction = packageName + AppboyNotificationUtils.APPBOY_NOTIFICATION_DELETED_SUFFIX;

    String action = intent.getAction();
    Log.d(TAG, String.format("Received intent with action %s", action));

    if (pushReceivedAction.equals(action)) {
      Log.d(TAG, "Received push notification.");
    } else if (notificationOpenedAction.equals(action)) {
      AppboyNotificationUtils.routeUserWithNotificationOpenedIntent(context, intent);
    } else if (notificationDeletedAction.equals(action)) {
      Log.d(TAG, "Received push notification deleted intent.");
    } else {
      Log.d(TAG, String.format("Ignoring intent with unsupported action %s", action));
    }
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomBroadcastReceiver : BroadcastReceiver() {
  override fun onReceive(context: Context, intent: Intent) {
    val packageName = context.packageName
    val pushReceivedAction = packageName + AppboyNotificationUtils.APPBOY_NOTIFICATION_RECEIVED_SUFFIX
    val notificationOpenedAction = packageName + AppboyNotificationUtils.APPBOY_NOTIFICATION_OPENED_SUFFIX
    val notificationDeletedAction = packageName + AppboyNotificationUtils.APPBOY_NOTIFICATION_DELETED_SUFFIX

    val action = intent.action
    Log.d(TAG, String.format("Received intent with action %s", action))

    when (action) {
      pushReceivedAction -> {
        Log.d(TAG, "Received push notification.")
      }
      notificationOpenedAction -> {
        AppboyNotificationUtils.routeUserWithNotificationOpenedIntent(context, intent)
      }
      notificationDeletedAction -> {
        Log.d(TAG, "Received push notification deleted intent.")
      }
      else -> {
        Log.d(TAG, String.format("Ignoring intent with unsupported action %s", action))
      }
    }
  }

  companion object {
    private val TAG = CustomBroadcastReceiver::class.java.name
  }
}
```

{% endtab %}
{% endtabs %}

{% alert tip %}
With notification action buttons, `APPBOY_NOTIFICATION_OPENED` intents fire when buttons with `opens app` or `deep link` actions are clicked. Deep-link and extras handling remains the same. Buttons with `close` actions don't fire `APPBOY_NOTIFICATION_OPENED` intents and dismiss the notification automatically.
{% endalert %}

#### Step 3: Access Custom Key-Value Pairs

Custom key-value pairs sent either via the dashboard or the messaging APIs will be accessible in your custom broadcast receiver for whatever purpose you choose:

{% tabs %}
{% tab JAVA %}

```java
// intent is the Braze push intent received by your custom broadcast receiver.
String deepLink = intent.getStringExtra(Constants.APPBOY_PUSH_DEEP_LINK_KEY);

// The extras bundle extracted from the intent contains all custom key-value pairs.
Bundle extras = intent.getBundleExtra(Constants.APPBOY_PUSH_EXTRAS_KEY);

// example of getting specific key-value pair from the extras bundle.
String myExtra = extras.getString("my_key");
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
// intent is the Braze push intent received by your custom broadcast receiver.
val deepLink = intent.getStringExtra(Constants.APPBOY_PUSH_DEEP_LINK_KEY)

// The extras bundle extracted from the intent contains all custom key-value pairs.
val extras = intent.getBundleExtra(Constants.APPBOY_PUSH_EXTRAS_KEY)

// example of getting specific key-value pair from the extras bundle.
val myExtra = extras.getString("my_key")
```

{% endtab %}
{% endtabs %}

{% alert note %}
Braze push data keys are documented [here](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Constants.html).
{% endalert %}

[5]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#setCustomAppboyNotificationFactory-com.appboy.IAppboyNotificationFactory-
[6]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyNotificationFactory.html
[8]: {{site.baseurl}}/help/best_practices/push/overview/
[16]: {% image_buster /assets/img_archive/fcm_api_insert.png %} "FCMKey"
[22]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[28]: {{site.baseurl}}/developer_guide/platform_integration_guides/fireos/push_notifications/integration/
[35]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#registerAppboyPushMessages-java.lang.String- "Manual Registration Method"
[37]: https://developer.android.com/guide/topics/ui/notifiers/notifications
[38]: {% image_buster /assets/img_archive/large_and_small_notification_icon.png %} "Large and Small Notification Icon"
[40]: http://developer.android.com/training/app-indexing/deep-linking.html "Google Deep Linking Documentation"
[41]: {% image_buster /assets/img_archive/deep_link_click_action.png %} "Deep Link Click Action"
[42]: {{site.baseurl}}/user_guide/personalization_and_dynamic_content/deep_linking_to_in-app_content/#what-is-deep-linking
[45]: https://firebase.google.com/docs/cloud-messaging/
[48]: https://developers.google.com/cloud-messaging/android/android-migrate-fcm
[49]: https://firebase.google.com/docs/android/setup
[50]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration_baidu/#baidu-integration
[52]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/#custom-handling-push-receipts-opens-and-key-value-pairs
[53]: https://developer.android.com/reference/android/content/BroadcastReceiver.html
[55]: {% image_buster /assets/img_archive/android_push_test.png %} "Android Push Test"
[56]: {{site.baseurl}}/user_guide/message_building_by_channel/push/creating_a_push_message/#creating-a-push-message
[57]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/troubleshooting/
[58]: https://console.firebase.google.com/
[59]: {% image_buster /assets/img_archive/finding_firebase_server_key.png %} "FirebaseServerKey"
[61]: {{site.baseurl}}/user_guide/message_building_by_channel/push/creating_a_push_message/#android-notification-options
[62]: https://developer.android.com/preview/features/notification-channels.html
[63]: {{site.baseurl}}/developer_guide/rest_api/messaging/#android-push-object
[64]: {{site.baseurl}}/user_guide/message_building_by_channel/push/notification_channels/#dashboard-fallback-channel
[65]: {{site.baseurl}}/developer_guide/platform_integration_guides/fireos/advanced_use_cases/runtime_configuration/#runtime-configuration
[66]: {{site.baseurl}}/developer_guide/rest_api/messaging/#sending-messages-immediately-via-api-only
[67]: https://developer.android.com/reference/android/app/Application.html#onCreate()
[68]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/runtime_configuration/#runtime-configuration
[70]: https://github.com/Appboy/appboy-android-sdk/blob/master/samples/firebase-push/src/main/AndroidManifest.xml "AndroidManifest.xml"
[71]: https://github.com/Appboy/appboy-android-sdk/blob/master/samples/custom-broadcast/src/main/AndroidManifest.xml "AndroidManifest.xml"
[72]: https://appboy.github.io/appboy-android-sdk/javadocs/com/braze/configuration/BrazeConfig.Builder.html#setDefaultNotificationChannelName-java.lang.String-
[73]: https://appboy.github.io/appboy-android-sdk/javadocs/com/braze/configuration/BrazeConfig.Builder.html#setDefaultNotificationChannelDescription-java.lang.String-
[74]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/AppboyFirebaseMessagingService.html#handleBrazeRemoteMessage-android.content.Context-RemoteMessage-
[75]: https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/RemoteMessage
[76]: https://developer.android.com/reference/android/app/Application
[77]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/push/BrazeNotificationPayload.html

---
nav_title: Deep Linking
platform: Android
page_order: 0
description: "This article covers how to implement the universal deep linking delegate for your Android app, as well as examples on how to deep link to app settings or a News Feed."

---

# Android Deep Linking

## Universal Deep Link Delegate

The Android SDK provides the ability to set a single delegate object to custom handle all deep links opened by Braze across Content Cards, in-app messages, and push notifications.

Your delegate object should implement the [`IAppboyNavigator`][udl-3] interface and be set using [`AppboyNavigator.setAppboyNavigator()`][udl-2]. In most cases, the delegate should be set in your app's `Application.onCreate()`.

The following is an example of overriding the default [`UriAction`][udl-1] behavior with custom intent flags and custom behavior for Youtube URLs. 

{% tabs %}
{% tab JAVA %}

```java
public class CustomNavigator implements IAppboyNavigator {
  private static final String TAG = AppboyLogger.getAppboyLogTag(CustomAppboyNavigator.class);

  @Override
  public void gotoNewsFeed(Context context, NewsfeedAction newsfeedAction) {
    newsfeedAction.execute(context);
  }

  @Override
  public void gotoUri(Context context, UriAction uriAction) {
    String uri = uriAction.getUri().toString();
    // Open Youtube URLs in the Youtube app and not our app
    if (!StringUtils.isNullOrBlank(uri) && uri.contains("youtube.com")) {
      uriAction.setUseWebView(false);
    }

    CustomUriAction customUriAction = new CustomUriAction(uriAction);
    customUriAction.execute(context);
  }

  public static class CustomUriAction extends UriAction {

    public CustomUriAction(@NonNull UriAction uriAction) {
      super(uriAction);
    }

    @Override
    protected void openUriWithActionView(Context context, Uri uri, Bundle extras) {
      Intent intent = getActionViewIntent(context, uri, extras);
      intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_SINGLE_TOP);
      if (intent.resolveActivity(context.getPackageManager()) != null) {
        context.startActivity(intent);
      } else {
        AppboyLogger.w(TAG, "Could not find appropriate activity to open for deep link " + uri + ".");
      }
    }
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomNavigator : IAppboyNavigator {

  override fun gotoNewsFeed(context: Context, newsfeedAction: NewsfeedAction) {
    newsfeedAction.execute(context)
  }

  override fun gotoUri(context: Context, uriAction: UriAction) {
    val uri = uriAction.uri.toString()
    // Open Youtube URLs in the Youtube app and not our app
    if (!StringUtils.isNullOrBlank(uri) && uri.contains("youtube.com")) {
      uriAction.useWebView = false
    }

    val customUriAction = CustomUriAction(uriAction)
    customUriAction.execute(context)
  }

  class CustomUriAction(uriAction: UriAction) : UriAction(uriAction) {

    override fun openUriWithActionView(context: Context, uri: Uri, extras: Bundle) {
      val intent = getActionViewIntent(context, uri, extras)
      intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_SINGLE_TOP
      if (intent.resolveActivity(context.packageManager) != null) {
        context.startActivity(intent)
      } else {
        AppboyLogger.w(TAG, "Could not find appropriate activity to open for deep link $uri.")
      }
    }
  }

  companion object {
    private val TAG = AppboyLogger.getAppboyLogTag(CustomAppboyNavigator::class.java)
  }
}
```

{% endtab %}
{% endtabs %}

## Deep Linking to App Settings

To allow for deep links to directly open your app's settings, you'll need a custom AppboyNavigator. In the following example, the presence of a custom key-value pair called `open_notification_page` will make the deep link open the app's settings page.

{% tabs %}
{% tab JAVA %}

```java
AppboyNavigator.setAppboyNavigator(new IAppboyNavigator() {
  @Override
  public void gotoUri(Context context, UriAction uriAction) {
    final Bundle extras = uriAction.getExtras();
    if (extras.containsKey("open_notification_page")) {
      Intent intent = new Intent();
      intent.setAction("android.settings.APP_NOTIFICATION_SETTINGS");
      intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);

      //for Android 5-7
      intent.putExtra("app_package", context.getPackageName());
      intent.putExtra("app_uid", context.getApplicationInfo().uid);

      // for Android 8 and above
      intent.putExtra("android.provider.extra.APP_PACKAGE", context.getPackageName());
      context.startActivity(intent);
    }
  }

  @Override
  public void gotoNewsFeed(Context context, NewsfeedAction newsfeedAction) {}
});
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
AppboyNavigator.setAppboyNavigator(object : IAppboyNavigator {
  override fun gotoUri(context: Context, uriAction: UriAction) {
    val extras = uriAction.extras
    if (extras.containsKey("open_notification_page")) {
      val intent = Intent()
      intent.action = "android.settings.APP_NOTIFICATION_SETTINGS"
      intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK

      //for Android 5-7
      intent.putExtra("app_package", context.packageName)
      intent.putExtra("app_uid", context.applicationInfo.uid)

      // for Android 8 and above
      intent.putExtra("android.provider.extra.APP_PACKAGE", context.packageName)
      context.startActivity(intent)
    }
  }

  override fun gotoNewsFeed(context: Context, newsfeedAction: NewsfeedAction) {}
})
```

{% endtab %}
{% endtabs %}

## Deep Linking to the News Feed {#Android_Deep_Advance}

To deep link to the Braze News Feed from a push notification, [create a custom deep link][1] for your News Feed activity.

Then, as you set up your push notification campaign (either through the [dashboard][2] or [API][3]), configure the notification to navigate to your News Feed Deep Link.

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/push_notifications/integration/#step-3-add-deep-links
[2]: {{site.baseurl}}/user_guide/message_building_by_channel/push/creating_a_push_message/#creating-a-push-message
[3]: {{site.baseurl}}/developer_guide/rest_api/messaging/#messaging
[udl-1]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/actions/UriAction.html
[udl-2]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/AppboyNavigator.html#setAppboyNavigator-com.appboy.IAppboyNavigator-
[udl-3]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyNavigator.html

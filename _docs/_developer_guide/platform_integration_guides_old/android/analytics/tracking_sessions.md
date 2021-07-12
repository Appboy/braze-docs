---
nav_title: Tracking Sessions
platform: Android
page_order: 0
description: "This reference article shows how to subscribe to session updates for your Android application."

---

# Session Tracking

The Braze SDK reports session data that is used by the Braze dashboard to calculate user engagement and other analytics integral to understanding your users. Based on the below session semantics, our SDK generates "start session" and "close session" data points that account for session length and session counts viewable within the Braze Dashboard.

## Session Lifecycle

If you have integrated Braze using our recommended [Activity Lifecycle Callback Integration] [session_tracking_8], `openSession()` and `closeSession()` will be called automatically for each Activity in your app. By default, sessions on Android are opened upon the first call to `openSession()` and are closed after an app has been out of the foreground for longer than 10 seconds.  Note that calling `closeSession()` does not close a session immediately. Rather, it closes a session in 10 seconds if the user doesn't call `openSession()` (e.g., by navigating to another Activity) in the interim.

An Android session times out after 10 seconds without any communication from the host application. This means if a user backgrounds the app and returns 9 seconds later, the same session will be continued.

__Note:__ If a session closes while the user has the app backgrounded, that data may not be flushed to the server until the app is opened again.

**Note**: If you need to force a new session, you can do so by changing users.

## Customizing Session Timeout
To customize the session timeout, add `com_appboy_session_timeout` to your [`braze.xml`][session_tracking_3] file:

```xml
<!-- The length of time before a session times out in seconds. The session manager will "re-open"
otherwise closed sessions if the call to StartSession comes within this interval. (default is 10) -->
<integer name="com_appboy_session_timeout">NUMBER_OF_SECONDS_UNTIL_SESSION_TIMEOUT</integer>
``` 

**Note**: The minimum value for `sessionTimeoutInSeconds` is 1 second.

## Testing Session Tracking

To detect sessions via your user, find your user on the dashboard and navigate to "App Usage" on the user profile. You can confirm that session tracking is working by checking that the "Sessions" metric increases when you would expect it to.

![test_session] [session_tracking_7]

## Subscribing to Session Updates

The Braze SDK provides a [`subscribeToSessionUpdates`][1] subscriber to listen for session updates.

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(this).subscribeToSessionUpdates(new IEventSubscriber<SessionStateChangedEvent>() {
  @Override
  public void trigger(SessionStateChangedEvent message) {
    if (message.getEventType() == SessionStateChangedEvent.ChangeType.SESSION_STARTED) {
      // A session has just been started
    }
  }
});
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(this).subscribeToSessionUpdates { message ->
  if (message.eventType == SessionStateChangedEvent.ChangeType.SESSION_STARTED) {
    // A session has just been started
  }
}
```

{% endtab %}
{% endtabs %}

[1]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#subscribeToSessionUpdates-com.appboy.events.IEventSubscriber-
[session_tracking_1]: {{ site.baseurl }}/developer_guide/platform_integration_guides/ios/initial_sdk_setup/#customizing-braze-on-startup
[session_tracking_3]: {{ site.baseurl }}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/#step-2-configure-the-braze-sdk-in-appboyxml
[session_tracking_5]: https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.initialize
[session_tracking_6]: http://msdn.microsoft.com/en-us/library/windows/apps/hh464925.aspx
[session_tracking_7]: {% image_buster /assets/img_archive/test_session.png %}
[session_tracking_8]: {{ site.baseurl }}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/#step-4-tracking-user-sessions-in-android

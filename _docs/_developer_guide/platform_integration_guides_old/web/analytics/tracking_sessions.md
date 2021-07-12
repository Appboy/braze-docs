---
nav_title: Tracking Sessions
platform: Web
page_order: 0

---

# Session Tracking

The Braze SDK reports session data that is used by the Braze dashboard to calculate user engagement and other analytics integral to understanding your users. Based on the below session semantics, our SDK generates "start session" and "close session" data points that account for session length and session counts viewable within the Braze Dashboard.

## Session Lifecycle

By default, sessions begin when `appboy.openSession()` is first called and remain open until there are at least 30 minutes of inactivity. This means that if the user navigates away from the site and then returns less than 30 minutes later, the same session will be continued. If they return after the 30 minutes have expired, a "close session" datapoint is automatically generated for the time at which they navigated away, and a new session opens.

**Note**: If you need to force a new session, you can do so by changing users.

## Customizing Session Timeout

To customize the session timeout, pass the `sessionTimeoutInSeconds` option to your [`initialize`][session_tracking_5] function.

```js
// Sets the session timeout to 15 minutes instead of the default 30
appboy.initialize('YOUR-API-KEY-HERE', { sessionTimeoutInSeconds: 900 });
``` 

If you have set a session timeout, then the above session semantics all extend to that customized timeout.

**Note**: The minimum value for `sessionTimeoutInSeconds` is 1 second.

### Testing Session Tracking

To detect sessions via your user, find your user on the dashboard and navigate to "App Usage" on the user profile. You can confirm that session tracking is working by checking that the "Sessions" metric increases when you would expect it to.

![test_session] [session_tracking_7]

[session_tracking_1]: {{ site.baseurl }}/developer_guide/platform_integration_guides/ios/initial_sdk_setup/#customizing-braze-on-startup
[session_tracking_3]: {{ site.baseurl }}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/#step-2-configure-the-braze-sdk-in-appboyxml
[session_tracking_5]: https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.initialize
[session_tracking_6]: http://msdn.microsoft.com/en-us/library/windows/apps/hh464925.aspx
[session_tracking_7]: {% image_buster /assets/img_archive/test_session.png %}
[session_tracking_8]: {{ site.baseurl }}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/#step-4-tracking-user-sessions-in-android


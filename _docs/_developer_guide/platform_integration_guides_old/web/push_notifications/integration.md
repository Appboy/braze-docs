---
platform: Web
page_order: 0

page_type: reference
description: "This article describes how to integrate Braze Web Push via the Braze SDK."

---

# Integration

A push notification is an alert that appears on the user's screen when an important update occurs. Push notifications can be received even when your web page is not currently open in the user's browser. Push notifications are a valuable way to provide your users with time-sensitive and relevant content or to re-engage them with your site.

![Sample Push][27]

Visit our [documentation][7] for additional best practices.

Web push notifications are implemented using the [W3C Push standard][1], which browsers are in the process of supporting. Currently, the browsers which support web push include most versions of Chrome, Firefox, and Opera. Web Push is not supported on any iOS browsers to date. It's expected that as the standard becomes more widely adopted, more browsers will continue to implement support. Additionally, desktop Safari (on Mac OS X) has a custom web push solution based on Apple Push Notification Services; Braze supports these Safari notifications as well.

## HTTPS Requirement

Web standards require that the domain requesting push notification permission be secure.

### What defines a secure site?

A site is deemed secure if it matches one of the following secure origin patterns:

- (https, , *)
- (wss, *, *)
- (, localhost, )
- (, .localhost, *)
- (, 127/8, )
- (, ::1/128, *)
- (file, *, —)
- (chrome-extension, *, —)

This is a security requirement in the open standards specification that Braze Web Push is built on, and prevents man-in-the-middle attacks.

### What if a secure site is not available?

While industry best practice is to make your whole site secure, customers who cannot secure their site domain can work around the requirement by using a secure modal. Read more in our guide to using [Alternate Push Domain][28] or view a working demo [here][4].

## Step 1: Configure your Site's Service Worker

- If you don't already have a Service Worker, create a new file named ```service-worker.js``` with the content below, and place it in the root directory of your website.

- Otherwise, if your site already registers a Service Worker, add the content below to the Service Worker file, and set the [```manageServiceWorkerExternally``` initialization option to ```true```](https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.initialize) when initializing the Web SDK.

<script src="https://gist-it.appspot.com/https://github.com/Appboy/appboy-web-sdk/blob/master/sample-build/service-worker.js?footer=minimal"></script>

{% alert important %}
Your web server must return a `Content-Type: application/javascript` when serving your Service Worker file. 
{% endalert %}

### What if I can't register a Service Worker in the root directory?

By default, a Service Worker can only be used within the same directory it is registered in. For example, if your Service Worker file exists in `/assets/service-worker.js`, then it would only be possible to register it within `example.com/assets/*` or a subdirectory of the `assets` folder, but not on your homepage (`example.com/`). For this reason, it is recommended to host and register the Service Worker in the root directory (i.e. `https://example.com/service-worker.js`).

If you are unable to register a Service Worker in your root domain, an alternative approach is to use the [`Service-Worker-Allowed`](https://w3c.github.io/ServiceWorker/#service-worker-script-response) HTTP Header when serving your Service Worker file. By configuring your server to return `Service-Worker-Allowed: /` in the response for the Service Worker, this will instruct the browser to broaden the scope and allow it to be used from within a different directory, like the homepage-even when the file exists in a deeper directory.


## Step 2: Browser Registration

In order for a browser to receive push notifications, you must register it for push by calling ```appboy.registerAppboyPushMessages()```. This will immediately request push permission from the user. 

If you wish to show your own push-related UI to the user _before_ requesting push permission (known as a soft push prompt), you can test to see if push is supported in the user's browser with ```appboy.isPushSupported()```. See [below for a soft push prompt example](#soft-push-prompts) using Braze In-App Messages. 

If you wish to unsubscribe a user, you can do so by calling ```appboy.unregisterAppboyPushMessages()```.

{% alert important %}
Recent versions of Safari and Firefox require that you call this method from a short-lived event handler (e.g. from a button click handler or soft push prompt). This is also consistent with [Chrome's user experience best practices](https://docs.google.com/document/d/1WNPIS_2F0eyDm5SS2E6LZ_75tk6XtBSnR1xNjWJ_DPE) for push registration.
{% endalert %}

## Step 3: Configure Safari Push

If you wish to support push notifications for Safari on Mac OS X, follow these additional instructions:

* [Generate a Safari Push Certificate following these "Registering with Apple" instructions][3]
* In the Braze dashboard, on the **Settings** page (where your API keys are located), select your Web app. Click **Configure Safari Push** and follow the instructions, uploading the push certificate you just generated.
* When you call ```appboy.initialize``` supply the optional `safariWebsitePushId` configuration option with the Website Push ID you used when generating your Safari Push Certificate. For example ```appboy.initialize('YOUR-API-KEY', {safariWebsitePushId: 'web.com.example.domain'})```

## Common Issues

__I followed the integration instructions but I'm still not receiving any push notifications.__

- Not all browsers can receive push messages. Please ensure that ```appboy.isPushSupported()``` returns true in the browser.
- Note that if a user has denied a site push access, they won't be prompted for permission again unless they remove the denied status from their browser preferences.
- Note that web push notifications require that your site be https.

## Soft Push Prompts

It's often a good idea for sites to implement a "soft" push prompt where you "prime" the user and make your case for sending them push notifications before requesting push permission. This is useful because the browser throttles how often you may prompt the user directly, and if the user denies permission you can never ask them again. This can be done simply through Braze's [triggered In-App Messages]({{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#in-app-messaging) for a seamless user experience. Instead of calling `appboy.registerAppboyPushMessages()` directly as described above, instead:

1. Create a "Prime for Push" in-app messaging Campaign on the Braze dashboard.
  - Make it a "Modal" In-App Message. Give it whatever text and styling you wish to present to the user ("Can we stay in touch?")
  - Give the in-app message a Button 1 Text value of "OK" (or whatever affirmative text you wish), and set the On-Click Behavior to "Close Message." You'll customize that behavior later.
  - Under the gear composer section, add a key-value pair.  Give it a key of `msg-id` and a value of `push-primer`.
  - Give the message a trigger action of the Custom Event 'prime-for-push' (you can create that custom event manually from the dashboard) if you need to)

2. In your Braze SDK integration, find and remove any calls to `appboy.display.automaticallyShowNewInAppMessages()` from within your loading snippet.

3. Replace the removed call with the following snippet:

```javascript

// Be sure to remove calls to appboy.display.automaticallyShowNewInAppMessages() 
// from your code as noted in the steps above

appboy.subscribeToInAppMessage(function(inAppMessage) {
  var shouldDisplay = true;

  if (inAppMessage instanceof appboy.InAppMessage) {
    // Read the key-value pair for msg-id
    var msgId = inAppMessage.extras["msg-id"];

    // If this is our push primer message
    if (msgId == "push-primer") {
      // We don't want to display the soft push prompt to users on browsers that don't support push, or if the user
      // has already granted/blocked permission
      if (!appboy.isPushSupported() || appboy.isPushPermissionGranted() || appboy.isPushBlocked()) {
        shouldDisplay = false;
      }
      if (inAppMessage.buttons[0] != null) {
        // Prompt the user when the first button is clicked
        inAppMessage.buttons[0].subscribeToClickedEvent(function() {
          appboy.registerAppboyPushMessages();
        });
      }
    }
  }

  // Display the message
  if (shouldDisplay) {
    appboy.display.showInAppMessage(inAppMessage);
  }
});
```

When you wish to display the soft push prompt to the user, call `appboy.logCustomEvent("prime-for-push")` - for instance, to prompt the user on every page load just after the Braze session begins, your code would look like:

```
appboy.openSession();
appboy.logCustomEvent("prime-for-push");
```

[1]: http://www.w3.org/TR/push-api/
[3]: https://developer.apple.com/library/mac/documentation/NetworkingInternet/Conceptual/NotificationProgrammingGuideForWebsites/PushNotifications/PushNotifications.html#//apple_ref/doc/uid/TP40013225-CH3-SW33
[4]: http://appboyj.com/modal-test.html
[7]: {{site.baseurl}}/help/best_practices/web_sdk/#web-push
[27]: {{site.baseurl}}/assets/img_archive/web_push2.png
[28]: {{ site.baseurl }}/developer_guide/platform_integration_guides/web/push_notifications/alternate_push_domain

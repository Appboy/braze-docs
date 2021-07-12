---
nav_title: Overview
platform: Web
page_order: 0

page_type: reference
description: "This reference article provides an overview of in-app messages, including best practices and use cases."
channel: in-app messages

---

# In-App Messages

__In-App Messages__ help you get content to your user without interrupting their day with a push notification. Customized and tailored in-app messages enhance the user experience and help your audience get the most value out of your app. With a variety of layouts and customization tools to choose from, in-app messages engage your users more than ever before.

{% comment %}
Embed video on the right. Demos all of the topics mentioned on this page.
{% endcomment %}

## When to Use In-App Messages

In-app messages are good for a lot of things. They can be used in web apps, Android apps, iOS apps, and more!

In-app messages don't deliver outside of the user's app and won't intrude on their home screen, like push notifications do. In-app messages, by their nature, exist within your app and come with context and are rarely unwelcome! They're always delivered when the user is active within your app.

#### Great Use Cases

- New App Features
- App Management
- Reviews
- App Upgrades/Updates
- Giveaways & Sweepstakes

To see examples of in-app messages, check out our [Client Integration Gallery][53].

## In-App Message Types

Braze currently offers the following default in-app message types: [`Slideup`][13], [`Modal`][17], and [`Full`][41] and [`HTML`][42].  Each in-app message type is customizable across content, images, icons, click actions, analytics, display, and delivery.

All in-app messages inherit their prototype from [`appboy.InAppMessage`][2], which defines basic behavior and traits for all in-app messages. The protypical subclasses are [appboy.SlideUpMessage][3], [appboy.ModalMessage][6], [appboy.FullScreenMessage][7], and [appboy.HtmlMessage][12].

## Expected Behaviors by Message Types

These are what it looks like for your users to open one of our out-of-the-box in-app message types.

{% tabs %}
  {% tab Slideup %}

  [`SlideUp`](https://js.appboycdn.com/web-sdk/latest/doc/ab.SlideUpMessage.html) in-app messages are so-named because traditionally on mobile platforms they "slide up" or "slide down" from the top or bottom of the screen. In the Braze Web SDK, these messages are displayed as more of a Growl or Toast style notification, to align with the web's dominant paradigm. They cover a small portion of the screen and provide an effective and non-intrusive messaging capability.

  <br>

  ![Slideup Behavior]({% image_buster /assets/img/slideup-behavior.gif %}){: style="border:0px;"}

  <br>

{% endtab %}
{% tab Modal %}

[`Modal`](https://js.appboycdn.com/web-sdk/latest/doc/ab.ModalMessage.html) in-app messages appear in the center of the screen and are framed by a translucent panel. Useful for more critical messaging, they can be equipped with up to two click action and analytics enabled buttons.

  <br>

  ![Modal Behavior]({% image_buster /assets/img/modal-behavior.gif %}){: style="border:0px;"}

  <br>

{% endtab %}
{% tab Full Screen %}

[`Full`](https://js.appboycdn.com/web-sdk/latest/doc/ab.FullScreenMessage.html) in-app messages are useful for maximizing the content and impact of your user communication. On narrow browser windows (e.g. the mobile web), `full` in-app messages take up the entire browser window. On larger browser windows, `full` in-app messages appear similarly to `modal` in-app messages. The upper half of a `full` in-app message contains an image and the lower half allows up to eight lines of text as well as up to two click action and analytics enabled buttons

<br>

![Full-Screen Behavior]({% image_buster /assets/img/full-screen-behavior.gif %}){: style="border:0px;"}

<br>

{% endtab %}
{% tab Custom HTML %}

[`HTML`](https://js.appboycdn.com/web-sdk/latest/doc/ab.HtmlMessage.html) in-app messages are useful for creating fully customized user content. User-defined HTML is displayed in an iframe and may contain rich content, such as images, fonts, videos, and interactive elements, allowing for full control over message appearance and functionality. These support a JavaScript `appboyBridge` interface to call methods on the Braze Web SDK from within your HTML, see [Best Practices]({{site.baseurl}}/help/best_practices/in-app_messages/web_browsers_only/#web-html-messages) for more details.

{% alert important %}

To enable HTML in-app messages, your SDK integration __must__ supply the `allowUserSuppliedJavascript` initialization option to Braze, e.g. `appboy.initialize('YOUR-API_KEY', {allowUserSuppliedJavascript: true})`. This is for security reasons - HTML in-app messages can execute JavaScript so we require a site maintainer to enable them.
<br> <br>
At Braze, the web SDK treats the "Email Web Capture Form" message type template as an HTML in-app message, therefore the same `allowUserSuppliedJavascript` option must be set.

{% endalert %}

The following example shows a paginated HTML in-app message:

![HTML5 Example]({% image_buster /assets/img_archive/ios-html-full-iam.gif %})

{% endtab %}
{% endtabs %}

[2]: https://js.appboycdn.com/web-sdk/latest/doc/ab.InAppMessage.html
[3]: https://js.appboycdn.com/web-sdk/latest/doc/ab.SlideUpMessage.html
[6]: https://js.appboycdn.com/web-sdk/latest/doc/ab.ModalMessage.html
[7]: https://js.appboycdn.com/web-sdk/latest/doc/ab.FullScreenMessage.html
[12]: https://js.appboycdn.com/web-sdk/latest/doc/ab.HtmlMessage.html
[13]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#slideup-in-app-messages
[17]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#modal-in-app-messages
[41]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#full-in-app-messages
[42]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#html-in-app-messages
[53]: {{site.baseurl}}/help/best_practices/client_integration_gallery/#client-integration-iam

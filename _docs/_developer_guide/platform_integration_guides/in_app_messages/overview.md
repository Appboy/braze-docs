---
nav_title: Overview
page_order: 0
description: ""
---

# In-App Messages

__In-App Messages__ help you get content to your user without interrupting their day with a push notification. Customized and tailored in-app messages enhance the user experience and help your audience get the most value out of your app. With a variety of layouts and customization tools to choose from, in-app messages engage your users more than ever before.

To see examples of in-app messages, check out our [Client Integration Gallery][83].

## When to Use In-App Messages

In-app messages are good for a lot of things. They can be used in web apps, Android apps, iOS apps, and more!

In-app messages don't deliver outside of the user's app and won't intrude on their home screen like push notifications do. In-app messages, by their nature, exist within your app and come with context and are almost never unwelcome! They're always delivered when the user is active within your app.

#### Great Use Cases

- New App Features
- App Management
- Reviews
- App Upgrades/Updates
- Giveaways & Sweepstakes
- Sales and Promotions
- Product Sales
- Encouraging and rewarding discovery
- [Permission Requests/Push Priming][30]

[Create an in-app message]({{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/) now!

## In-App Message Types

{% tabs %}
{% tab Android & FireOS %}

Braze offers several default in-app message types, each customizable with messages, images, [Font Awesome](http://fortawesome.github.io/Font-Awesome/) icons, click actions, analytics, editable styling and color schemes. The currently available types are `Slideup`, `Modal`, `Full`, and `HTML Full`. It is also possible to [define your own custom in-app message view]({{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/customization/#setting-a-custom-view-factory).

All in-app messages implement the [`IInAppMessage`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessage.html) interface, which defines basic behavior and traits for all in-app messages. [`InAppMessageBase`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageBase.html) is an abstract class that implements `IInAppMessage` and provides the foundational in-app message implementation. All in-app message classes are subclasses of `InAppMessageBase`.

In addition, there is a subinterface of `IInAppMessage` called [`IInAppMessageImmersive`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessageImmersive.html), which adds click action and analytics enabled [buttons](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/MessageButton.html), as well as header text and a close button. [`InAppMessageImmersiveBase`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageImmersiveBase.html) is an abstract class that implements `IInAppMessageImmersive` and provides the foundational `immersive` in-app message implementation. `Modal` and `Full` in-app messages are subclasses of `InAppMessageImmersiveBase`.

HTML Full in-app messages are [`InAppMessageHtmlFull`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageHtmlFull.html) instances, which implement [`IInAppMessageHtml`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessageHtml.html), another subclass of `IInAppMessage`.

{% endtab %}
{% tab iOS %}

Braze currently offers the following default in-app message types: `Slideup`, `Modal`, `Full` and `HTML Full`. Each in-app message type is highly customizable across content, images, icons, click actions, analytics, display, and delivery.

All in-app messages are subclasses of the `ABKInAppMessage`, which defines basic behavior and traits for all in-app messages. The in-app message class structures as following:

![ABKInAppMessage models]({% image_buster /assets/img_archive/ABKInAppMessage-models.png %})

{% alert important %}
By default, in-app messages are enabled after completing the standard SDK integration, including GIF support. 
<br><br>
__Note that integration of `SDWebImage` is required if you plan on using our Braze UI for displaying images__ within iOS In-App Messages, News Feed, or Content Cards.
{% endalert %}

{% endtab %}
{% tab Web %}

Braze currently offers the following default in-app message types: [`Slideup`]({{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#slideup-in-app-messages), [`Modal`]({{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#modal-in-app-messages), and [`Full`]({{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#full-in-app-messages) and [`HTML`]({{site.baseurl}}/developer_guide/platform_integration_guides/web/in_app_messaging/#html-in-app-messages).  Each in-app message type is customizable across content, images, icons, click actions, analytics, display, and delivery.

All in-app messages inherit their prototype from [`appboy.InAppMessage`](https://js.appboycdn.com/web-sdk/latest/doc/ab.InAppMessage.html), which defines basic behavior and traits for all in-app messages. The protypical subclasses are [appboy.SlideUpMessage](https://js.appboycdn.com/web-sdk/latest/doc/ab.SlideUpMessage.html), [appboy.ModalMessage](https://js.appboycdn.com/web-sdk/latest/doc/ab.ModalMessage.html), [appboy.FullScreenMessage](https://js.appboycdn.com/web-sdk/latest/doc/ab.FullScreenMessage.html), and [appboy.HtmlMessage](https://js.appboycdn.com/web-sdk/latest/doc/ab.HtmlMessage.html).

{% endtab %}
{% endtabs %}

### Expected Behaviors by Message Types

{% tabs %}
{% tab Android & FireOS %}

These are what each in-app message type will appear like for your users.

{% subtabs global %}
  {% subtab Slideup %}
  [`Slideup`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageSlideup.html) in-app messages are so-named because they "slide up" or "slide down" from the top or bottom of the screen.  They cover a small portion of the screen and provide an effective and non-intrusive messaging capability.

  <br>

  ![Slideup Behavior]({% image_buster /assets/img/slideup-behavior.gif %}){: style="border:0px;"}

  <br>

{% endsubtab %}
{% subtab Modal %}
[`Modal`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageModal.html) in-app messages appear in the center of the screen and are framed by a translucent panel. Useful for more critical messaging, they can be equipped with up to two click action and analytics enabled buttons.

  <br>

  ![Modal Behavior]({% image_buster /assets/img/modal-behavior.gif %}){: style="border:0px;"}

  <br>


{% endsubtab %}
{% subtab Full Screen %}
[`Full`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageHtmlFull.html) in-app messages are useful for maximizing the content and impact of your user communication.  The upper half of a `full` in-app message contains an image and the lower half displays text as well as up to two click action and analytics enabled buttons.

![Full Example]({% image_buster /assets/img_archive/In-App_Full.png %})


{% endsubtab %}
{% subtab Custom HTML %}
[`HTML Full`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessageHtml.html) in-app messages are useful for creating fully customized user content. User-defined HTML Full in-app message content is displayed in a {% if include.platform == "iOS" %}`WKWebView`{% elsif include.platform == "Android" %}`WebView`{% endif %} and may optionally contain other rich content, such as images and fonts, allowing for full control over message appearance and functionality. <br><br>Android in-app messages support a JavaScript `appboyBridge` interface to call methods on the Braze Web SDK from within your HTML, see <a href="https://www.braze.com/docs/help/best_practices/in-app_messages/previous_in-app_message_generations/">Best Practices</a> for more details.

<br>

![Full-Screen Behavior]({% image_buster /assets/img/full-screen-behavior.gif %}){: style="border:0px;"}

<br>

Full in-app message content is displayed in a `WKWebView` and may optionally contain other rich content, such as images and fonts, allowing for full control over message appearance and functionality.

{% alert important %}
Please note that we currently do not support display of custom HTML in-app messages in an iFrame on the iOS and Android platforms.
{% endalert %}

{% endsubtab %}
{% endsubtabs %}

#### In-Depth: Defining Custom In-App Message Types

Braze's `slideup` in-app message object extends [`InAppMessageBase`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageBase.html). Braze's `full` and `modal` type messages extend [`InAppMessageImmersiveBase`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/InAppMessageImmersiveBase.html). Extending one of these classes gives you the option of adding custom functionality to your locally generated in-app messages.

{% endtab %}
{% tab iOS %}

These are what it looks like for your users to open one of our out-of-the-box in-app message types.

{% subtabs %}
  {% subtab Slideup %}

  [`Slideup`](https://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_in_app_message_slideup.html) in-app messages are so-named because they "slide up" or "slide down" from the top or bottom of the screen.  They cover a small portion of the screen and provide an effective and non-intrusive messaging capability.

  <br>

  ![Slideup Behavior]({% image_buster /assets/img/slideup-behavior.gif %}){: style="border:0px;"}

  <br>

{% endsubtab %}
{% subtab Modal %}

[`Modal`](https://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_in_app_message_modal.html) in-app messages appear in the center of the screen and are framed by a translucent panel. Useful for more critical messaging, they can be equipped with up to two click action and analytics enabled buttons.

 <br>

  ![Modal Behavior]({% image_buster /assets/img/modal-behavior.gif %}){: style="border:0px;"}

 <br>

{% endsubtab %}
{% subtab Full Screen %}

[`Full`](https://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_in_app_message_full.html) in-app messages are useful for maximizing the content and impact of your user communication.  The upper half of a `full` in-app message contains an image and the lower half displays text as well as up to two click action and analytics enabled buttons.

<br>

![Full-Screen Behavior]({% image_buster /assets/img/full-screen-behavior.gif %}){: style="border:0px;"}

<br>

{% endsubtab %}
{% subtab Custom HTML %}

[`HTML Full`](https://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_in_app_message_h_t_m_l_full.html) in-app messages are useful for creating fully customized user content. User-defined HTML Full in-app message content is displayed in a `WKWebView`and may optionally contain other rich content, such as images and fonts, allowing for full control over message appearance and functionality. <br><br>iOS in-app messages support a JavaScript `appboyBridge` interface to call methods on the Braze Web SDK from within your HTML, see <a href="https://www.braze.com/docs/help/best_practices/in-app_messages/previous_in-app_message_generations/">Best Practices</a> for more details.

The following example shows a paginated HTML Full in-app message:

![HTML5 Example]({% image_buster /assets/img_archive/ios-html-full-iam.gif %})

> Full in-app message content is displayed in a WKWebView and may optionally contain other rich content, such as images and fonts, allowing for full control over message appearance and functionality. **Please note that we currently do not support display of custom HTML in-app messages in an iFrame on the iOS and Android platforms.**

> **Starting in iOS SDK version 3.19.0, the following JavaScript methods are no-ops in HTML in-app messages: `alert`, `confirm`, `prompt`.**

{% endsubtab %}
{% endsubtabs %}

{% endtab %}
{% tab Web %}

These are what it looks like for your users to open one of our out-of-the-box in-app message types.

{% subtabs %}
  {% subtab Slideup %}

  [`SlideUp`](https://js.appboycdn.com/web-sdk/latest/doc/ab.SlideUpMessage.html) in-app messages are so-named because traditionally on mobile platforms they "slide up" or "slide down" from the top or bottom of the screen. In the Braze Web SDK, these messages are displayed as more of a Growl or Toast style notification, to align with the web's dominant paradigm. They cover a small portion of the screen and provide an effective and non-intrusive messaging capability.

  <br>

  ![Slideup Behavior]({% image_buster /assets/img/slideup-behavior.gif %}){: style="border:0px;"}

  <br>

{% endsubtab %}
{% subtab Modal %}

[`Modal`](https://js.appboycdn.com/web-sdk/latest/doc/ab.ModalMessage.html) in-app messages appear in the center of the screen and are framed by a translucent panel. Useful for more critical messaging, they can be equipped with up to two click action and analytics enabled buttons.

  <br>

  ![Modal Behavior]({% image_buster /assets/img/modal-behavior.gif %}){: style="border:0px;"}

  <br>

{% endsubtab %}
{% subtab Full Screen %}

[`Full`](https://js.appboycdn.com/web-sdk/latest/doc/ab.FullScreenMessage.html) in-app messages are useful for maximizing the content and impact of your user communication. On narrow browser windows (e.g. the mobile web), `full` in-app messages take up the entire browser window. On larger browser windows, `full` in-app messages appear similarly to `modal` in-app messages. The upper half of a `full` in-app message contains an image and the lower half allows up to eight lines of text as well as up to two click action and analytics enabled buttons

<br>

![Full-Screen Behavior]({% image_buster /assets/img/full-screen-behavior.gif %}){: style="border:0px;"}

<br>

{% endsubtab %}
{% subtab Custom HTML %}

[`HTML`](https://js.appboycdn.com/web-sdk/latest/doc/ab.HtmlMessage.html) in-app messages are useful for creating fully customized user content. User-defined HTML is displayed in an iframe and may contain rich content, such as images, fonts, videos, and interactive elements, allowing for full control over message appearance and functionality. These support a JavaScript `appboyBridge` interface to call methods on the Braze Web SDK from within your HTML, see [Best Practices]({{site.baseurl}}/help/best_practices/in-app_messages/web_browsers_only/#web-html-messages) for more details.

{% alert important %}

To enable HTML in-app messages, your SDK integration __must__ supply the `allowUserSuppliedJavascript` initialization option to Braze, e.g. `appboy.initialize('YOUR-API_KEY', {allowUserSuppliedJavascript: true})`. This is for security reasons - HTML in-app messages can execute JavaScript so we require a site maintainer to enable them.
<br> <br>
At Braze, the web SDK treats the "Email Web Capture Form" message type template as an HTML in-app message, therefore the same `allowUserSuppliedJavascript` option must be set.

{% endalert %}

The following example shows a paginated HTML in-app message:

![HTML5 Example]({% image_buster /assets/img_archive/ios-html-full-iam.gif %})

{% endsubtab %}
{% endsubtabs %}

{% endtab %}
{% endtabs %}

[30]: {{site.baseurl}}/help/best_practices/push/creating_custom_opt-in_prompts/#creating-custom-opt-in-prompts
[83]: {{site.baseurl}}/help/best_practices/client_integration_gallery/#client-integration-iam


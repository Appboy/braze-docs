---
nav_title: Customize
platform: Message_Building_and_Personalization
subplatform: In-App Messages
page_order: 2
description: "In addition to the out-of-the-box In-App Message templates, Braze also offers customized messaging templates that allow custom HTML, Modals with custom CSS, Video, and more."
---

# Customization

Craft and customize the perfect In-App or In-Browser Message using your own HTML, CSS, and Javascript!

### SDK Requirements {#supported-sdk-versions}

{% alert important %}
This guide references newer features which have Braze SDK requirements. For our deprecated zip-upload messages, please [click here][1].
{% endalert %}

__SDK Requirements__
* Web SDK v2.5+ [Changelog]({{site.baseurl}}/developer_guide/platform_integration_guides/web/changelog/#250)
* iOS SDK v3.23.0+ [Changelog]({{site.baseurl}}/developer_guide/platform_integration_guides/ios/changelog/#3230)
* Android SDK v8.0.0+ [Changelog]({{site.baseurl}}/developer_guide/platform_integration_guides/android/changelog/#800)

Because users on older SDK versions will not receive the message, consider adopting this new message type once a significant portion of your user base is reachable, or target only those users whose app version is _above_ the requirements. [Learn More][2]

{% alert note %}
To enable HTML in-app messages in the Web SDK, your SDK integration must supply the `enableHtmlInAppMessages` initialization option to Braze: for example `appboy.initialize('YOUR-API_KEY', {enableHtmlInAppMessages: true})`. This is for security reasons - HTML in-app messages can execute javascript so we require a site maintainer to enable them.
{% endalert %}

## Quickstart Templates

Get a head start using our pre-built HTML templates. Just click "View Code", copy the HTML, and paste it into your In-App message campaign. Swap out images, text, and launch!

|Quickstart Template|Download Link|Screenshot|
|--------|----------|---------|
|Multiple Page Carousel| [View Code](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/1-braze-dashboard-carousel-modal/index.html){:target="_blank"}|![Screenshot](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/1-braze-dashboard-carousel-modal/screenshot.gif){: style="max-width:200px;width:100%"}|
|Simple Modal| [View Code](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/2-braze-dashboard-simple-modal/index.html){:target="_blank"}|![Screenshot](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/2-braze-dashboard-simple-modal/screenshot.png){: style="max-width:200px;width:100%"}|
|In-App Survey| [View Code](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/3-braze-dashboard-survey-modal/index.html){:target="_blank"}|![Screenshot](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/3-braze-dashboard-survey-modal/screenshot.gif){: style="max-width:200px;width:100%"}|
|Fullscreen Pagination| [View Code](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/5-fullscreen-pagination/index.html){:target="_blank"}|![Screenshot](https://raw.githubusercontent.com/braze-inc/in-app-message-templates/master/braze-templates/5-fullscreen-pagination/screenshot.gif){: style="max-width:200px;width:100%"}|

## Customizing your HTML

In addition to using custom CSS, Javascript, and HTML, you can also leverage your Braze SDK to log custom events, set custom user attributes, and more using the injected [`appboyBridge`][3] javascript SDK.

### Close Button

All good messages must come to an end! To close a message, use the `appboyBridge.closeMessage()` method. For example:

In a close button:

```html
<button onclick="appboyBridge.closeMessage()">✕</button>
```

Or, programatically:

```javascript
setTimeout(function(){
    appboyBridge.closeMessage();
}, 5000);
```

### Button Click Tracking

In addition to the usual conversion tracking, each message can also measure click-through rates of up to 3 buttons, called "Button 1", "Button 2", and "Body Clicks" for historical reasons.

Use `appboyBridge.logClick(id_string)` to programatically track these button click events. Currently, the only values supported are `0` and `1`, for Button 1 and Button 2, respectively.

|Button| Javascript|
|----|-----|
|Body Click| `appboyBridge.logClick()`|
|Button 1 Click| `appboyBridge.logClick('0')`|
|Button 2 Click| `appboyBridge.logClick('1')`|

{% alert important %}
This method replaces older automatic click tracking methods (i.e. `?abButtonId=0`), and is no longer limited to one click per-impression.
{% endalert %}

#### Migrating from older button tracking

| Before | After |
|:-------- |:------------|
|<code>&lt;a href="appboy://close"&gt;Close Button&lt;/a&gt;</code>|<code>&lt;a href="#" onclick="appboyBridge.logClick();appboyBridge.closeMessage()"&gt;Close Button&lt;/a&gt;</code>|
|<code>&lt;a href="app://deeplink?abButtonId=0">Track button 1&lt;/a&gt;</code>|<code>&lt;a href="app://deeplink" onclick="appboyBridge.logClick('0')"&gt;Track button 1&lt;/a&gt;</code>|
|<code>&lt;script&gt;<br>location.href = "appboy://close?abButtonId=1"<br>&lt;/script&gt;</code>|<code>&lt;script&gt;<br>window.addEventListener("ab.BridgeReady", function(){<br>&nbsp;&nbsp;appboyBridge.logClick("1");<br>&nbsp;&nbsp;appboyBridge.closeMessage();<br>});<br>&lt;/script&gt;</code>|

### Custom Events

Track custom events using `appboyBridge.logCustomEvent` and `appboyBridge.requestImmediateDataFlush`:

```javascript
document.getElementById('button').onclick = function(){
    appboyBridge.logCustomEvent("name", {
        key: "value"
    });
    appboyBridge.requestImmediateDataFlush();
};
```

### Custom Attributes


Set custom user attributes using `appboyBridge.getUser().setCustomUserAttribute` and `appboyBridge.requestImmediateDataFlush`:

```javascript
document.getElementById('button').onclick = function(){
    appboyBridge.getUser().setCustomUserAttribute("favorite color", "blue");
    appboyBridge.requestImmediateDataFlush();
};
```

### Other Braze SDK Methods {#javascript-bridge}

A subset of Braze Web SDK methods are available within HTML messages, allowing you to trigger custom Braze actions when users engage with your content. These methods can be accessed from with the global `appboyBridge` variable.

For example, to log a custom attribute, custom event, and then close the message, you could use the following JavaScript within your HTML in-app message:

```html
<button id="button">Set Favorite Color</button>
<script>
// wait for the `appboyBridge` ready event, "ab.BridgeReady"
window.addEventListener("ab.BridgeReady", function(){
  // event handler when the button is clicked
  document.querySelector("#button").onclick = function(){
    // track Button 1 clicks for analytics
    // Note: this requires Android SDK v8.0.0, Web SDK v2.5.0, and iOS SDK v3.23.0
    appboyBridge.logClick("0");
    // set the user's custom attribute
    appboyBridge.getUser().setCustomUserAttribute("favorite color", "blue");
    // track a custom event
    appboyBridge.logCustomEvent("completed survey");
    // send the enqueued data to Braze
    appboyBridge.requestImmediateDataFlush();
    // close this in-app message
    appboyBridge.closeMessage();
  };
}, false);
</script>
```
#### appboyBridge methods

The following JavaScript methods are supported within Braze's HTML in-app messages:

<style>
/* makes first column wider */
#article-main > table:first-of-type > tbody > tr td:first-child {
    min-width: 470px !important;
}
/* makes code column smaller font */
#article-main > table:first-of-type > tbody > tr td:first-child code {
    font-size:12px !important;
}
#article-main > table:first-of-type td {
  word-break: break-word;
}
</style>

{% include archive/appboyBridge.md %}

[1]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/customize/
[2]: {{ site.baseurl }}/user_guide/engagement_tools/campaigns/ideas_and_strategies/new_features/#filtering-by-most-recent-app-versions
[3]: #javascript-bridge

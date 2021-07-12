---
nav_title: Braze JavaScript Interface
platform: Android
page_order: 9
description: "This article shows how to add the Braze JavaScript Interface to WebViews."

---

# Adding The Braze JavaScript Interface to Webviews

Using Braze functionality from a WebView in your app can be done by adding the Braze JavaScript interface to your WebView as shown below. Once the interface has been added, the same API available for [HTML In-App Messages][1] will be available within your custom WebView.

{% tabs %}
{% tab JAVA %}

```java
String javascriptString = AppboyFileUtils.getAssetFileStringContents(context.getAssets(), "appboy-html-in-app-message-javascript-component.js");
myWebView.loadUrl("javascript:" + javascriptString);

final AppboyInAppMessageHtmlJavascriptInterface javascriptInterface = new AppboyInAppMessageHtmlJavascriptInterface(context, inAppMessage);
myWebView.addJavascriptInterface(javascriptInterface, "appboyInternalBridge");
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val javascriptString = AppboyFileUtils.getAssetFileStringContents(context.getAssets(), "appboy-html-in-app-message-javascript-component.js")
myWebView.loadUrl("javascript:" + javascriptString!!)

val javascriptInterface = AppboyInAppMessageHtmlJavascriptInterface(context, inAppMessage)
myWebView.addJavascriptInterface(javascriptInterface, "appboyInternalBridge")
```

{% endtab %}
{% endtabs %}

[1]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/customize/#custom-html-messages
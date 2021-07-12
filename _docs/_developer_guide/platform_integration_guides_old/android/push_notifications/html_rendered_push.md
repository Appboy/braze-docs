---
nav_title: HTML Push Notifications
platform: Android
page_order: 6
description: "This article covers hwo to implement HTML push notifications in your Android application."
channel:
  - push


---

# HTML Push Notifications

In Braze SDK version 3.1.1, HTML can be sent to a device to render multicolor text in push notifications.

![Multicolor push example][1]

The above example is rendered with the following HTML:

```html
<p><span style="color: #99cc00;">M</span>u<span style="color: #008080;">lti</span>Colo<span style="color: #ff6600;">r</span> <span style="color: #000080;">P</span><span style="color: #00ccff;">u</span><span style="color: #ff0000;">s</span><span style="color: #808080;">h</span></p>
```

```html
<p><em>test</em> <span style="text-decoration: underline; background-color: #ff6600;"><strong>message</strong></span></p>
```

> Android OS limits what HTML elements/tags are valid in push notifications. For example, `marquee` is not allowed.

## Implementation

In your `braze.xml`:

```xml
<bool translatable="false" name="com_appboy_push_notification_html_rendering_enabled">true</bool>
```

Or in your [BrazeConfig][2]:

{% tabs %}
{% tab JAVA %}

```java
BrazeConfig brazeConfig = new BrazeConfig.Builder()
  .setPushHtmlRenderingEnabled(true)
  .build();
Braze.configure(this, brazeConfig);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val brazeConfig = BrazeConfig.Builder()
    .setPushHtmlRenderingEnabled(true)
    .build()
Braze.configure(this, brazeConfig)
```

{% endtab %}
{% endtabs %}

[1]: {% image_buster /assets/img/multicolor_android_push.png %}
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/runtime_configuration/#runtime-configuration

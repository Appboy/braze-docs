---
nav_title: Accessibility
page_order: 4
platform: Android
description: "This reference article covers how to implement specific Android SDK accessibility features such as in-app message talkback into your Android application."

---

# Accessibility

The Braze Android SDK follows the [Android Accessibility Guidelines][1].

## In-App Message Talkback

In order to have Android Talkback/"VoiceOver" not read the contents behind an in-app message during display, enable the following SDK configuration:

{% tabs %}
{% tab braze.xml %}

```xml
<bool name="com_appboy_device_in_app_message_accessibility_exclusive_mode_enabled">true</bool>
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val brazeConfigBuilder = BrazeConfig.Builder()
brazeConfigBuilder.setIsInAppMessageAccessibilityExclusiveModeEnabled(true)
Braze.configure(this, brazeConfigBuilder.build())
```

{% endtab %}
{% tab JAVA %}

```java
BrazeConfig.Builder brazeConfigBuilder = new BrazeConfig.Builder()
brazeConfigBuilder.setIsInAppMessageAccessibilityExclusiveModeEnabled(true);
Braze.configure(this, brazeConfigBuilder.build());
```

{% endtab %}
{% endtabs %}


[1]: https://developer.android.com/guide/topics/ui/accessibility

---
nav_title: Tracking Custom Events
platform: Android
page_order: 2
description: "This reference article covers how to add and track custom events for your Android application."

---

# Tracking Custom Events

You can record custom events in Braze to learn more about your app's usage patterns and to segment your users by their actions on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs purchase events in our [Analytics Overview][0], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Adding A Custom Event

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).logCustomEvent(YOUR_EVENT_NAME);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).logCustomEvent(YOUR_EVENT_NAME)
```

{% endtab %}
{% endtabs %}

See the [Javadoc][2] for more information.

### Adding Properties

You can add metadata about custom events by passing a [Braze Properties][4] object with your custom event.

Properties are defined as key-value pairs.  Keys are `String` objects and values can be `String`, `int`, `float`, `boolean`, or [`Date`][3] objects.

{% tabs %}
{% tab JAVA %}

```java
AppboyProperties eventProperties = new AppboyProperties();
eventProperties.addProperty("key", "value");
Appboy.getInstance(YOUR_ACTIVITY.this).logCustomEvent(YOUR_EVENT_NAME, eventProperties);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val eventProperties = AppboyProperties()
eventProperties.addProperty("key", "value")
Appboy.getInstance(context).logCustomEvent(YOUR_EVENT_NAME, eventProperties)
```

{% endtab %}
{% endtabs %}

### Reserved Keys

The following keys are __RESERVED__ and __CANNOT__ be used as Custom Event Properties:

- `time`
- `event_name`

See the [Javadoc][6] for more information.

[0]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[2]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logCustomEvent(java.lang.String) "Javadocs"
[3]: http://developer.android.com/reference/java/util/Date.html
[4]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/outgoing/AppboyProperties.html
[6]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logCustomEvent(java.lang.String,%20com.appboy.models.outgoing.AppboyProperties)

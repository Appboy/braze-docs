---
nav_title: Tracking Custom Events
platform: FireOS
page_order: 2

page_type: reference
description: "This article covers how to track custom events for your FireOS app."

---

# Tracking Custom Events

You can record custom events in Braze to learn more about your app's usage patterns and to segment your users by their actions on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Analytics Overview][0], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Adding A Custom Event

```java
Appboy.getInstance(context).logCustomEvent(YOUR_EVENT_NAME);
```

See the [Javadoc][2] for more information.

### Adding Properties

You can add metadata about custom events by passing a [Braze Properties][4] object with your custom event.

Properties are defined as key-value pairs.  Keys are `String` objects and values can be `String`, `int`, `float`, `boolean`, or [`Date`][3] objects.

```java
AppboyProperties eventProperties = new AppboyProperties();
eventProperties.addProperty("key", "value");
Appboy.getInstance(context).logCustomEvent(YOUR_EVENT_NAME, eventProperties);
```

### Reserved Keys

The following keys are __RESERVED__ and __CANNOT__ be used as custom event properties:

- `time`
- `product_id`
- `quantity`
- `event_name`
- `price`
- `currency`

See the [Javadoc][6] for more information.

[0]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[2]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logCustomEvent(java.lang.String) "Javadocs"
[3]: http://developer.android.com/reference/java/util/Date.html
[4]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/outgoing/AppboyProperties.html
[6]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logCustomEvent(java.lang.String,%20com.appboy.models.outgoing.AppboyProperties)

---
nav_title: Tracking Custom Events
platform: Web
page_order: 2

page_type: reference
description: "This article covers how to track custom events via the Braze SDK."

---

# Tracking Custom Events

You can record custom events in Braze to learn more about your app's usage patterns and to segment your users by their actions on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][0]. You should also check out our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

```javascript
appboy.logCustomEvent(YOUR_EVENT_NAME);
```

See the [logCustomEvent documentation][1] for more information.

## Adding Properties {#properties-events}

You can optionally add metadata about custom events by passing a properties object with your custom event.

Properties are defined as key-value pairs.  Keys are strings and values can be `string`, `numeric`, `boolean`, or [`Date`][2] objects.

```javascript
appboy.logCustomEvent(YOUR_EVENT_NAME, {key: 'value'});
```

See the [logCustomEvent documentation][1] for more information.

[0]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[1]: https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.logCustomEvent
[2]: http://www.w3schools.com/jsref/jsref_obj_date.asp

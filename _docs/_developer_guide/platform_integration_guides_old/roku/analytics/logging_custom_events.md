---
nav_title: Tracking Custom Events
platform: Roku
page_order: 2

page_type: reference
description: "This page covers methods to record custom events via the Braze SDK."

---

# Tracking Custom Events

You can record custom events in Braze to learn more about your app's usage patterns and to segment your users by their actions on the dashboard. You should also check out our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Add a Custom Event

```javascript
m.Braze.logEvent("YOUR_EVENT_NAME")
```

### Adding Properties

You can add metadata about custom events by passing a properties dictionary with your custom event.

Properties are defined as key-value pairs.  Keys are `String` objects and values can be `String`, or `Integer`.

```javascript
m.Braze.logEvent("YOUR_EVENT_NAME", {"stringPropKey" : "stringPropValue", "intPropKey" : Integer intPropValue})
```


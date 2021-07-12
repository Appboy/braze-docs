---
nav_title: Tracking Custom Events
platform: Unity
page_order: 1
description: "This reference article covers how to log custom events on Unity platform."

---

# Logging Custom Events

You can record custom events in Braze to learn more about your app's usage patterns and to segment your users by their actions on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][4]. You should also check out our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

```csharp
AppboyBinding.LogCustomEvent("event name");
```

Braze also supports adding metadata about custom events by passing a `Dictionary` of event properties:

```csharp
AppboyBinding.LogCustomEvent("event name", properties(Dictionary<string, object>));
```

## REST API

You can also use our REST API to record events. Refer to the [user API documentation][5] for details.

[4]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#best-practices
[5]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data

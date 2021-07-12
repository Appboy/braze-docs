---
nav_title: Logging Custom Events
platform: React Native
subplatform: 
- Android
- FireOS
page_order: 1

page_type: reference
description: "This page provides resources and methods to log custom events for your Android or FireOS app running React Native."

---

## Logging Custom Events

See [the Android integration instructions][1] for in depth discussion of event tracking best practices and interfaces. You should also check out our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

```javascript
var properties = {};
properties["KeyOne"] = "Val1";
ReactAppboy.logCustomEvent("reactNativeCustomEventWithProperties", properties);
```

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/

---
nav_title: Logging Purchases
platform: React Native
subplatform: iOS
page_order: 3

page_type: reference
description: "This page provides resources and methods for logging purchases for your iOS app running React Native."

---

# Logging Purchases

See [the iOS integration instructions][1] for in depth discussion of revenue tracking best practices and interfaces.

```javascript
var properties = {};
properties["KeyOne"] = "ValueOne";
ReactAppboy.logPurchase("productId", "10.99", "USD", 5, properties);
```

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/logging_purchases/

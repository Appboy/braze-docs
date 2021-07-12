---
nav_title: Logging Purchases
platform: Web
page_order: 4

page_type: reference
description: "This article describes how to log purchases via the Braze SDK."

---

# Logging Purchases

Record in-app purchases so that you can track your revenue over time and across revenue sources, as well as segment your users by their lifetime value.

Braze supports purchases in multiple currencies. Purchases that you report in a currency other than USD will be shown in the dashboard in USD based on the exchange rate at the date they were reported.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][3].

To use this feature, add this method call after a successful purchase in your app:

```javascript
appboy.logPurchase(productId, price, "USD", quantity);
```

See the [JSdocs][8] for more information. Quantity must be less than or equal to 100.

## Adding Properties

You can add metadata about purchases by passing an object of key-value pairs with your purchase information. Keys are `string` objects and values can be `string`, `numeric`, `boolean`, or `Date` objects.

```javascript
appboy.logPurchase(productId, price, "USD", quantity, {key: "value"});
```

See the [Jsdocs][8] for more information.

## REST API

You can also use our REST API to record purchases. Refer to the [User API documentation][1] for details.

[1]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[3]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[8]: https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.logPurchase

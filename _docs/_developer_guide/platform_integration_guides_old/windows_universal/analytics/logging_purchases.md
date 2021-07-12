---
nav_title: Logging Purchases
platform: Windows_Universal
page_order: 4
description: "This reference article covers how to log purchases on the Windows Universal platform."

---

# Logging Purchases

Record in-app purchases so that you can track your revenue over time and across revenue sources, as well as segment your users by their lifetime value.

Braze supports purchases in multiple currencies. Purchases that you report in a currency other than USD will be shown in the dashboard in USD based on the exchange rate at the date they were reported.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][3]. You should also check out our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

To use this feature, add this method call after a successful purchase in your app:

Purchases are logged by using the `EventLogger`, which is a property exposed in IAppboy. To obtain a reference to the `EventLogger`, call `Appboy.SharedInstance.EventLogger`.

```csharp
bool LogPurchase(string productId, string currencyCode, decimal price)
```

## REST API

You can also use our REST API to record purchases. Refer to the [user API documentation][2] for details.

[2]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[3]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection

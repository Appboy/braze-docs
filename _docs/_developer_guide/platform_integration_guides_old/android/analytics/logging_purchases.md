---
nav_title: Logging Purchases
platform: Android
page_order: 4
description: "This reference article shows how to track in-app purchases and revenue and assign purchase properties in your Android application."

---

# Logging Purchases

Record in-app purchases so that you can track your revenue over time and across revenue sources, as well as segment your users by their lifetime value.

Braze supports purchases in multiple currencies. Purchases that you report in a currency other than USD will be shown in the dashboard in USD based on the exchange rate at the date they were reported.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Analytics Overview][3].

## Tracking Purchases & Revenue

To use this feature, add this method call after a successful purchase in your app:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).logPurchase(
   String productId,
   String currencyCode,
   BigDecimal price,
   int quantity
);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).logPurchase(
  productId: String,
  currencyCode: String,
  price: BigDecimal,
  quantity: Int
)
```

{% endtab %}
{% endtabs %}

__If the product Identifier is empty, the purchase will not be logged to Braze.__ See the [Javadoc][8] for more information.

{% alert tip %}
  If you pass in a value of `10 USD`, and a quantity of `3` then that will log to the user's profile as 3 purchases of 10 dollars for a total of 30 dollars. Quantities must be less than or equal to 100. Values of purchases can be negative.
{% endalert %}

### Adding Properties

You can add metadata about purchases by passing a [Braze Properties][4] object with your purchase information.

Properties are defined as key-value pairs.  Keys are `String` objects and values can be `String`, `int`, `float`, `boolean`, or [`Date`][5] objects.

{% tabs %}
{% tab JAVA %}

```java
AppboyProperties purchaseProperties = new AppboyProperties();
purchaseProperties.addProperty("key", "value");
Appboy.getInstance(context).logPurchase(..., purchaseProperties);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val purchaseProperties = AppboyProperties()
purchaseProperties.addProperty("key", "value")
Appboy.getInstance(context).logPurchase(..., purchaseProperties)
```

{% endtab %}
{% endtabs %}

See the [Javadoc][6] for more information.

### Reserved Keys

The following keys are __RESERVED__ and __CANNOT__ be used as Purchase Properties:

- `time`
- `product_id`
- `quantity`
- `event_name`
- `price`
- `currency`

### REST API

You can also use our REST API to record purchases. Refer to the [User API documentation][1] for details.

[1]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[3]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[4]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/outgoing/AppboyProperties.html
[5]: http://developer.android.com/reference/java/util/Date.html
[6]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logPurchase(java.lang.String,%20java.lang.String,%20java.math.BigDecimal,%20int,%20com.appboy.models.outgoing.AppboyProperties)
[8]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logPurchase(java.lang.String,%20java.lang.String,%20java.math.BigDecimal,%20int)

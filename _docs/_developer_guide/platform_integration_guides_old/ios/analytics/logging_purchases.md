---
nav_title: Logging Purchases
platform: iOS
page_order: 4
description: "This reference article shows how to track in-app purchases and revenue and assign purchase properties in your iOS application."

---

# Logging Purchases

Record in-app purchases so that you can track your revenue over time and across revenue sources, as well as segment your users by their lifetime value.

Braze supports purchases in multiple currencies. Purchases that you report in a currency other than USD will be shown in the dashboard in USD based on the exchange rate at the date they were reported.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][5], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Tracking Purchases & Revenue

To use this feature, add this method call after a successful purchase in your app:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] logPurchase:@"your product ID"
inCurrency:@"USD"
atPrice:[[[NSDecimalNumber alloc] initWithString:@"0.99"] autorelease]];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.logPurchase("your product ID", inCurrency: "USD", atPrice: NSDecimalNumber(string: "0.99"))
```

{% endtab %}
{% endtabs %}

- Supported currency symbols include: USD, CAD, EUR, GBP, JPY, AUD, CHF, NOK, MXN, NZD, CNY, RUB, TRY, INR, IDR, ILS, SAR, ZAR, AED, SEK, HKD, SPD, DKK, and more.
  - Any other provided currency symbol will result in a logged warning and no other action taken by the SDK.
- The product ID can have a maximum of 255 characters
- Please note that if the product identifier is empty, the purchase will not be logged to Braze.

### Adding Properties {#properties-purchases}
You can add metadata about purchases by passing an `NSDictionary` populated with `NSNumber`, `NSString`, or `NSDate` values.

Please see the [iOS Class Documentation for additional details][8].

### Adding Quantity
You can add a quantity to your purchases if customers make the same purchase multiple times in a single checkout. You can accomplish this by passing in a `NSUInteger` for the quantity.

* A quantity input must be in the range of [0, 100] for the SDK to log a purchase.
* Methods without a quantity input will have a default quantity value of 1.
* Methods with a quantity input have no default value and **must** receive a quantity input for the SDK to log a purchase.

Please see the [iOS Class Documentation for additional details][7].

>  If you pass in a value of 10 USD, and a quantity of 3 then that will log to the user's profile as 3 purchases of 10 dollars for a total of 30 dollars.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] logPurchase:@"your product ID"
inCurrency:@"USD"
atPrice:[[[NSDecimalNumber alloc] initWithString:@"0.99"] autorelease]
withProperties:@{@"key1":"value1"}];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.logPurchase("your product ID", inCurrency: "USD", atPrice: NSDecimalNumber(string: "0.99"), withProperties: ["key1":"value1"])
```

{% endtab %}
{% endtabs %}

See the [Technical Documentation][6] for more information.

### Reserved Keys

The following keys are __RESERVED__ and __CANNOT__ be used as Purchase Properties:

- `time`
- `product_id`
- `quantity`
- `event_name`
- `price`
- `currency`

**Additional Information

- See the method declaration within the [`Appboy.h` file][2]. - In addition, you may refer to the [logPurchase documentation]() for more information.

### REST API

You can also use our REST API to record purchases. Refer to the [user API documentation][4] for details.

[2]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/Appboy.h
[4]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[5]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[6]: http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#ad35bb238aaa4fe9d1ede0439a4c401db "logcustomevent:withproperties documentation"
[7]: http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#ab50403068be47c0acba9943583e259fa "logpurchase w/ quantity class documentation"
[8]: http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#aaca4b885a8f61ac9fad3936b091448cc "logpurchase w/ properties class documentation"

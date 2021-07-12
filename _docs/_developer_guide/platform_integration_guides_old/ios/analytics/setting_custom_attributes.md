---
nav_title: Setting Custom Attributes
platform: iOS
page_order: 3
description: "This reference article shows how to set custom attributes in your iOS application."

---

# Setting Custom Attributes

Braze provides methods for assigning attributes to users. You'll be able to filter and segment your users according to these attributes on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][1], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Assigning Standard User Attributes

To assign user attributes, you need to set the appropriate field on the shared `ABKUser` object.

The following is an example of setting the first name attribute:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[Appboy sharedInstance].user.firstName = @"first_name";
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.firstName = "first_name"
```

{% endtab %}
{% endtabs %}

The following attributes should be set on the `ABKUser` object:

- `firstName`
- `lastName`
- `email`
- `dateOfBirth`
- `country`
- `language`
- `homeCity`
- `phone`
- `userID`
- `avatarImageURL`
- `twitterAccountIdentifier`
- `gender`

## Assigning Custom User Attributes

Beyond the attributes above, Braze also allows you to define custom attributes using a number of different data types:
For more information regarding the segmentation options, each of these attributes will afford you, see our ["Best Practices" documentation][1] within this section.

### Custom Attribute with a String Value

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setCustomAttributeWithKey:@"your_attribute_key" andStringValue:"your_attribute_value"];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setCustomAttributeWithKey("your_attribute_key", andStringValue: "your_attribute_value")
```

{% endtab %}
{% endtabs %}

### Custom Attribute with an Integer Value

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setCustomAttributeWithKey:@"your_attribute_key" andIntegerValue:yourIntegerValue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setCustomAttributeWithKey("your_attribute_key", andIntegerValue: yourIntegerValue)
```

{% endtab %}
{% endtabs %}

### Custom Attribute with a Double Value

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setCustomAttributeWithKey:@"your_attribute_key" andDoubleValue:yourDoubleValue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setCustomAttributeWithKey("your_attribute_key", andDoubleValue: yourDoubleValue)
```

{% endtab %}
{% endtabs %}

>  Braze treats `float` and `double` values the same within our database.

### Custom Attribute with a Boolean Value

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setCustomAttributeWithKey:@"your_attribute_key" andBOOLValue:yourBOOLValue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setCustomAttributeWithKey("your_attribute_key", andBOOLValue: yourBoolValue)
```

{% endtab %}
{% endtabs %}

### Custom Attribute with a Date Value

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setCustomAttributeWithKey:@"your_attribute_key" andDateValue:yourDateValue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setCustomAttributeWithKey("your_attribute_key", andDateValue:yourDateValue)
```

{% endtab %}
{% endtabs %}

>  Dates passed to Braze with this method must either be in the [ISO 8601][2] format, e.g `2013-07-16T19:20:30+01:00` or in the `yyyy-MM-dd'T'HH:mm:ss:SSSZ` format e.g `2016-12-14T13:32:31.601-0800`

### Custom Attribute with an Array Value
The maximum number of elements in custom attribute arrays defaults to 25. The maximum for individual arrays can be increased to up to 100. If you would like this maximum increased, please reach out to your Customer Service Manager. Arrays exceeding the maximum number of elements will be truncated to contain the maximum number of elements. For more information on custom attribute arrays and their behavior, see our [documentation on arrays][8].

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Setting a custom attribute with an array value
[[Appboy sharedInstance].user setCustomAttributeArrayWithKey:@"array_name" array:@[@"value1",  @"value2"]];
// Adding to a custom attribute with an array value
[[Appboy sharedInstance].user addToCustomAttributeArrayWithKey:@"array_name" value:@"value3"];
// Removing a value from an array type custom attribute
[[Appboy sharedInstance].user removeFromCustomAttributeArrayWithKey:@"array_name" value:@"value2"];
// Removing an entire array and key
[[Appboy sharedInstance].user setCustomAttributeArrayWithKey:@"array_name" array:nil];
```

{% endtab %}
{% tab swift %}

```swift
// Setting a custom attribute with an array value
Appboy.sharedInstance()?.user.setCustomAttributeArrayWithKey("array_name", array: ["value1",  "value2"])
// Adding to a custom attribute with an array value
Appboy.sharedInstance()?.user.addToCustomAttributeArrayWithKey("array_name", value: "value3")
// Removing a value from an array type custom attribute
Appboy.sharedInstance()?.user.removeFromCustomAttributeArrayWithKey("array_name", value: "value2")
```

{% endtab %}
{% endtabs %}

### Unsetting a Custom Attribute

Custom Attributes can also be unset using the following method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user unsetCustomAttributeWithKey:@"your_attribute_key"];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.unsetCustomAttributeWithKey("your_attribute_key")
```

{% endtab %}
{% endtabs %}

### Incrementing/Decrementing Custom Attributes

This code is an example of an incrementing custom attribute. You may increment the value of a custom attribute by any positive or negative integer or long value.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user incrementCustomUserAttribute:@"your_attribute_key" by:incrementIntegerValue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.incrementCustomUserAttribute("your_attribute_key", by: incrementIntegerValue)
```

{% endtab %}
{% endtabs %}

### Setting a Custom Attribute via the REST API

You can also use our REST API to set user attributes. To do so refer to the [user API documentation][3].

### Custom Attribute Value Limits

Custom attribute values have a maximum length of 255 characters; longer values will be truncated.

#### Additional Information

- More details can be found within the [`ABKUser.h` file][5].
- Besides, you may refer to the [ABKUser documentation][6] for more information.

## Setting Up User Subscriptions

To set up a subscription for your users (either email or push), call the functions `setEmailNotificationSubscriptionType` or `setPushNotificationSubscriptionType`, respectively. Both of these functions take the enum type `ABKNotificationSubscriptionType` as arguments. This type has three different states:

| Subscription Status | Definition |
| ------------------- | ---------- |
| `ABKOptedin` | Subscribed, and explicitly opted in |
| `ABKSubscribed` | Subscribed, but not explicitly opted in |
| `ABKUnsubscribed` | Unsubscribed and/or explicitly opted out |
{: .reset-td-br-1 .reset-td-br-2}

Users who grant permission for an app to send them push notifications default to the status of `ABKOptedin` as iOS requires an explicit opt-in.

> Users will be set to `ABKSubscribed` automatically upon receipt of a valid email address, however, we suggest that you establish an explicit opt-in process and set this value to `OptedIn` upon receipt of explicit consent from your user. [See the User Guide for details][12].

### Setting Email Subscriptions

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setEmailNotificationSubscriptionType: ABKNotificationSubscriptionType]
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setEmailNotificationSubscriptionType(ABKNotificationSubscriptionType)
```

{% endtab %}
{% endtabs %}

### Setting Push Notification Subscriptions

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].user setPushNotificationSubscriptionType: ABKNotificationSubscriptionType]
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.user.setPushNotificationSubscriptionType(ABKNotificationSubscriptionType)
```

{% endtab %}
{% endtabs %}

For more information on implementing subscriptions, visit our page on [managing user subscriptions][10].

[1]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[2]: http://en.wikipedia.org/wiki/ISO_8601
[3]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[5]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/Appboy.h
[6]: http://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_user.html
[8]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#arrays
[10]: {{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#managing-user-subscriptions
[12]: {{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#managing-user-subscriptions

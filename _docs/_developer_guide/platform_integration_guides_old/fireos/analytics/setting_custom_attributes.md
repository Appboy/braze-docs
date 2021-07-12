---
nav_title: Setting Custom Attributes
platform: FireOS
page_order: 3

page_type: reference
description: "This article covers methods for assigning custom attributes to users."

---

# Setting Custom Attributes

Braze provides methods for assigning attributes to users. You'll be able to filter and segment your users according to these attributes on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Analytics Overview][7], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Assigning Standard User Attributes

To assign attributes to your users, call the `getCurrentUser()` method on your Braze instance to get a reference to the current user of your app. Once you have a reference to the current user, you can call methods to set predefined or custom attributes.

Braze provides predefined methods for setting the following user attributes within the [AppboyUser class][2]. See the [Javadocs for method specifications][2]:

- First Name
- Last Name
- Country
- Language
- Date of Birth
- Email
- Avatar Image URLs for Braze User Profiles
- Gender
- Home City
- Phone Number
- Facebook Data
- Twitter Data

All string values such as first name, last name, country, and home city are limited to 255 characters. Avatar Image URLs are limited to 1024 characters.

**Implementation Example**
This is what setting a first name would look like in code:

```java
Appboy.getInstance(context).getCurrentUser().setFirstName("first_name");
```

## Assigning Custom User Attributes

In addition to our predefined user attribute methods, Braze also provides custom attributes to track data from your applications. Braze custom attributes can be set with the following data types:

- Strings
- Arrays
  - Includes methods to set arrays, add items to existing arrays and delete items from existing arrays.
- Integers
- Booleans
- Dates
- Longs
- Floats

Full method specifications for custom attributes can be found here within the [AppboyUser class within the Javadocs][2].

### Setting a Custom Attribute with a String Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", "your_attribute_value");
```

### Setting a Custom Attribute with an Integer Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute, "your_attribute_key", YOUR_INT_VALUE);
// Integer attributes may also be incremented using code like the following:
Appboy.getInstance(context).getCurrentUser().incrementCustomUserAttribute("your_attribute_key", YOUR_INCREMENT_VALUE);
```

### Setting a Custom Attribute with a Boolean Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_BOOLEAN_VALUE);
```

### Setting a Custom Attribute with a Long Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_LONG_VALUE);
```

### Setting a Custom Attribute with a Float Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_FLOAT_VALUE);
```

### Setting a Custom Attribute with a Double Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_DOUBLE_VALUE);
```

### Setting a Custom Attribute with a Date Value

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_DATE_VALUE);
// This method will assign the current time to a custom attribute at the time the method is called:
Appboy.getInstance(context).getCurrentUser().setCustomUserAttributeToNow("your_attribute_key");
// This method will assign the date specified by SECONDS_FROM_EPOCH to a custom attribute:
Appboy.getInstance(context).getCurrentUser().setCustomUserAttributeToSecondsFromEpoch("your_attribute_key", SECONDS_FROM_EPOCH);
```

{% alert warning %}
  Dates passed to Braze with this method must either be in the [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) format (e.g `2013-07-16T19:20:30+01:00`) or in the `yyyy-MM-dd'T'HH:mm:ss:SSSZ` format (e.g `2016-12-14T13:32:31.601-0800`).
{% endalert %}

### Setting a Custom Attribute with an Array Value
The maximum number of elements in Custom Attribute Arrays defaults to 25. The maximum for individual arrays can be increased to up to 100. If you would like this maximum increased, please reach out to your Customer Service Manager. Arrays exceeding the maximum number of elements will be truncated to contain the maximum number of elements. For more information on Custom Attribute Arrays and their behavior, see our [Documentation on Arrays][6].

```java
// Setting a custom attribute with an array value
Appboy.getInstance(context).getCurrentUser().setCustomAttributeArray("your_attribute_key", testSetArray);
// Adding to a custom attribute with an array value
Appboy.getInstance(context).getCurrentUser().addToCustomAttributeArray("your_attribute_key", "value_to_add");
// Removing a value from an array type custom attribute
Appboy.getInstance(context).getCurrentUser().removeFromCustomAttributeArray("your_attribute_key", "value_to_remove");
```

#### Unsetting a Custom Attribute

Custom Attributes can also be unset using the following method:

```java
Appboy.getInstance(context).getCurrentUser().unsetCustomUserAttribute("your_attribute_key");
```

#### Setting a Custom Attribute via the REST API

You can also use our REST API to set user attributes. To do so refer to the [User API documentation][4].

#### Custom Attribute Length

Custom attribute keys and values have a maximum length of 255 characters.  Longer strings will be truncated to 255 characters.

Full class information can be found in the [javadocs][2].

### Setting Up User Subscriptions

To set up a subscription for your users (either email or push), call the functions `setEmailNotificationSubscriptionType()`  or `setPushNotificationSubscriptionType()`, respectively. Both of these functions take the enum type 'NotificationSubscriptionType' as arguments. This type has three different states:

| Subscription Status | Definition |
| ------------------- | ---------- |
| `OPTED_IN` | Subscribed, and explicitly opted in |
| `SUBSCRIBED` | Subscribed, but not explicitly opted in |
| `UNSUBSCRIBED` | Unsubscribed and/or explicitly opted out |
{: .reset-td-br-1 .reset-td-br-2}


{% alert important %}
No explicit opt-in is required by Android to send users push notifications. When a user is registered for push, they are set to `SUBSCRIBED` rather than `OPTED_IN` by default. For more information on implementing subscriptions and explicit opt-ins, visit the topic on [Braze Docs][10].
{% endalert %}

#### Sample Code

##### Setting Email Subscriptions

```java
Appboy.getInstance(context).getCurrentUser().setEmailNotificationSubscriptionType(emailNotificationSubscriptionType);
```

##### Setting Push Notification Subscription

```java
Appboy.getInstance(context).getCurrentUser().setPushNotificationSubscriptionType(pushNotificationSubscriptionType);
```

[2]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/AppboyUser.html "Javadocs"
[4]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[6]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#arrays
[7]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[10]: {{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#managing-user-subscriptions

---
nav_title: Setting Custom Attributes
platform: Web
page_order: 3

---

# Setting Custom Attributes

Braze provides methods for assigning attributes to users. You'll be able to filter and segment your users according to these attributes on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][7].

To assign attributes to your users, call the `appboy.getUser()` method to get a reference to the current user of your app. Once you have a reference to the current user, you can call methods to set predefined or custom attributes.

Braze provides predefined methods for setting the following user attributes within the [ab.User class][1]:

- First Name
- Last Name
- Biographical Strings
- Country
- Date of Birth
- Email
- Avatar Image URLs for Braze User Profiles
- Gender
- Home City
- Phone Number

## Implementation Examples

### Setting a first name

```javascript
appboy.getUser().setFirstName("SomeFirstName");
```

### Setting a gender

```javascript
appboy.getUser().setGender(appboy.User.Genders.FEMALE);
```

### Setting a date of birth

```javascript
appboy.getUser().setDateOfBirth(2000, 12, 25);
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

Full method specifications for custom attributes can be found here within the [ab.User class JSDocs][1].

### Implementation Examples

#### Setting a Custom Attribute with a String Value
```javascript
appboy.getUser().setCustomUserAttribute(
  YOUR_ATTRIBUTE_KEY_STRING,
  YOUR_STRING_VALUE
);
```

#### Setting a Custom Attribute with an Integer Value
```javascript
appboy.getUser().setCustomUserAttribute(
  YOUR_ATTRIBUTE_KEY_STRING,
  YOUR_INT_VALUE
);

// Integer attributes may also be incremented using code like the following
appboy.getUser().incrementCustomUserAttribute(
  YOUR_ATTRIBUTE_KEY_STRING,
  THE_INTEGER_VALUE_BY_WHICH_YOU_WANT_TO_INCREMENT_THE_ATTRIBUTE
);
```

#### Setting a Custom Attribute with a Date Value
```javascript
appboy.getUser().setCustomUserAttribute(
  YOUR_ATTRIBUTE_KEY_STRING,
  YOUR_DATE_VALUE
);

// This method will assign the current time to a custom attribute at the time the method is called
appboy.getUser().setCustomUserAttribute(
  YOUR_ATTRIBUTE_KEY_STRING,
  new Date()
);

// This method will assign the date specified by secondsFromEpoch to a custom attribute
appboy.getUser().setCustomUserAttribute(
  YOUR_ATTRIBUTE_KEY_STRING,
  new Date(secondsFromEpoch * 1000)
);
```
>  Dates passed to Braze with this method must be JavaScript Date objects.

#### Setting a Custom Attribute with an Array Value
The maximum number of elements in custom attribute arrays defaults to 25. The maximum for individual arrays can be increased to up to 100. If you would like this maximum increased, please reach out to your Customer Service Manager. Arrays exceeding the maximum number of elements will be truncated to contain the maximum number of elements. For more information on custom attribute arrays and their behavior, see our [Documentation on Arrays][6].

```javascript
appboy.getUser().setCustomUserAttribute(YOUR_ATTRIBUTE_KEY_STRING, YOUR_ARRAY_OF_STRINGS);

// Adding a new element to a custom attribute with an array value
appboy.getUser().addToCustomAttributeArray(YOUR_ATTRIBUTE_KEY_STRING, "new string");

// Removing an element from a custom attribute with an array value
appboy.getUser().removeFromCustomAttributeArray("custom_attribute_array_test", "value to be removed");
```

### Unsetting a Custom Attribute

Custom attributes can be unset by setting their value to null.

```javascript
appboy.getUser().setCustomUserAttribute(YOUR_ATTRIBUTE_KEY_STRING, null);
```

### Setting a Custom Attribute via the REST API

You can also use our REST API to set user attributes. To do so refer to the [User API documentation][4].

### Custom Attribute Length

Custom attribute keys and values have a maximum length of 255 characters. See the [full technical documentation][1] for complete details around valid custom attribute values.

### Setting Up User Subscriptions

To set up a subscription for your users (either email or push), call the functions `setEmailNotificationSubscriptionType()`  or `setPushNotificationSubscriptionType()`, respectively. Both of these functions take the enum type 'appboy.User.NotificationSubscriptionTypes' as arguments. This type has three different states:

| Subscription Status | Definition |
| ------------------- | ---------- |
| `OPTED_IN` | Subscribed, and explicitly opted in |
| `SUBSCRIBED` | Subscribed, but not explicitly opted in |
| `UNSUBSCRIBED` | Unsubscribed and/or explicitly opted out |
{: .reset-td-br-1 .reset-td-br-2}

>  When a user is registered for push, the browser forces them to choose to allow or block notifications, and if they choose to allow push, they are set `OPTED_IN` by default. For more information on implementing subscriptions and explicit opt-ins, visit the topic in [our docs][10].

### Sample Code

#### Unsubscribing a user from email:
```javascript
appboy.getUser().setEmailNotificationSubscriptionType(appboy.User.NotificationSubscriptionTypes.UNSUBSCRIBED);
```

#### Unsubscribing a user from push:
```java
appboy.getUser().setPushNotificationSubscriptionType(appboy.User.NotificationSubscriptionTypes.UNSUBSCRIBED);
```

[1]: https://js.appboycdn.com/web-sdk/latest/doc/ab.User.html "ab.User"
[4]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[6]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#arrays
[7]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[10]: {{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#managing-user-subscriptions

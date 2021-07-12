---
nav_title: Fine Network Traffic Control
platform: iOS
page_order: 1
description: "This article covers how to implement fine network traffic control for your iOS application"

---

# Fine Network Traffic Control

## Request Processing Policies

Braze allows the user the option to control network traffic using the following protocols:

### Automatic Request Processing

__*`ABKRequestProcessingPolicy` enum value: `ABKAutomaticRequestProcessing`*__

- This is the **default request policy** value.
- The Braze SDK will automatically handle all server communication, including:
    - Flushing custom events and attributes data to Braze's servers
    - Updating the News Feed, Content Cards, and Geofences
    - Requesting new in-app messages
- Immediate server requests are performed when user-facing data is required for any of Braze's features, such as in-app messages.
- To minimize server load, Braze performs periodic flushes of new user data every few seconds.

Data can be manually flushed to Braze's servers at any time using the following method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] flushDataAndProcessRequestQueue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.flushDataAndProcessRequestQueue()
```

{% endtab %}
{% endtabs %}

### Manual Request Processing

__*`ABKRequestProcessingPolicy` enum value: `ABKManualRequestProcessing`*__

- This protocol is the same as Automatic Request Processing **EXCEPT**:
    - Custom attributes and custom event data is not automatically flushed to the server throughout the user session.
- Braze will still perform automatic network requests for internal features, such as requesting in-app messages, Liquid Templating in In-App Messages, Geofences, and Location Tracking. For more details, see the `ABKRequestProcessingPolicy` declaration in [`Appboy.h`][4]. When these internal requests are made, locally stored custom attributes and custom event data may be flushed to the Braze server, depending on the request type.

Data can be manually flushed to Braze's servers at any time using the following method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] flushDataAndProcessRequestQueue];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.flushDataAndProcessRequestQueue()
```

{% endtab %}
{% endtabs %}

## Setting the Request Processing Policy

### Set Request Policy On Startup

These policies can be set at app startup time from the [`startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions`][3] method. In the `appboyOptions` dictionary, set the `ABKRequestProcessingPolicyOptionKey` to any of the following `ABKRequestProcessingPolicy` enum values defined below:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
typedef NS_ENUM(NSInteger, ABKRequestProcessingPolicy) {
  ABKAutomaticRequestProcessing,
  ABKManualRequestProcessing
};
```

{% endtab %}
{% tab swift %}

```swift
public enum ABKRequestProcessingPolicy : Int {
    case automaticRequestProcessing
    case manualRequestProcessing
}
```

{% endtab %}
{% endtabs %}

### Set Request Policy At Runtime

The request processing policy can also be set during runtime via the `requestProcessingPolicy` property on `Appboy`. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Sets the request processing policy to automatic (the default value)
[Appboy sharedInstance].requestProcessingPolicy = ABKAutomaticRequestProcessing;
```

{% endtab %}
{% tab swift %}

```swift
// Sets the request processing policy to automatic (the default value)
Appboy.sharedInstance()?.requestProcessingPolicy = ABKRequestProcessingPolicy.automaticRequestProcessing
```

{% endtab %}
{% endtabs %}

## Manual Shutdown of In-Flight Server Communication

If at any time an "in-flight" server communication needs to be halted, you must call the following method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] shutdownServerCommunication];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.shutdownServerCommunication();
```

{% endtab %}
{% endtabs %}

After calling this method, you must reset the request processing mode back to Automatic. For this reason, we only recommend calling this if the OS is forcing you to stop background tasks or something similar.

[3]: #customizing-appboy-on-startup
[4]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/Appboy.h

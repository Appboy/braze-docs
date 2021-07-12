---
nav_title: In-App Messages
platform: React Native
subplatform: iOS
page_order: 2
page_type: reference
description: "This article covers in-app messages for iOS apps using React Native, including customizing and logging analytics."
channel: in-app messages

---

# In-App Messages

Native in-app messages display automatically on iOS when using React Native.

## Customizing

### Accessing In-App Message Data
If you would like to access the in-app message data in the JavaScript layer, implement the `ABKInAppMessageControllerDelegate` delegate as described in our public documentation [for iOS][1]. In the `beforeInAppMessageDisplayed:` delegate method, you can access the `inAppMessage` data, send it to the JavaScript layer, and decide to show or not show the native message based on the return value (for more on these values, see our [iOS documentation][2]).

{% tabs %}
{% tab OBJECTIVE-C %}
```objc
// In-app messaging
- (ABKInAppMessageDisplayChoice) beforeInAppMessageDisplayed:(ABKInAppMessage *)inAppMessage {
  NSData *inAppMessageData = [inAppMessage serializeToData];
  NSString *inAppMessageString = [[NSString alloc] initWithData:inAppMessageData encoding:NSUTF8StringEncoding];
  NSDictionary *arguments = @{
    @"inAppMessage" : inAppMessageString
  };
  // Send to JavaScript
  [self.bridge.eventDispatcher
             sendDeviceEventWithName:@"inAppMessageReceived"
             body:arguments];
  // Note: return ABKDiscardInAppMessage if you would like
  // to prevent the Braze SDK from displaying the message natively.
  return ABKDisplayInAppMessageNow;
}
```
{% endtab %}
{% tab swift %}
```swift
// In-app messaging
func beforeInAppMessageDisplayed(inAppMessage: ABKInAppMessage!) -> ABKInAppMessageDisplayChoice {
    let inAppMessageData = inAppMessage?.serializeToData()
    var inAppMessageString: String? = nil
    if let inAppMessageData = inAppMessageData {
        inAppMessageString = String(data: inAppMessageData, encoding: .utf8)
    }
    let arguments = [
        "inAppMessage": inAppMessageString ?? ""
    ]
    // Send to JavaScript
    bridge.eventDispatcher.sendDeviceEvent(
        withName: "inAppMessageReceived",
        body: arguments)
    // Note: return ABKDiscardInAppMessage if you would like
    // to prevent the Braze SDK from displaying the message natively.
    return ABKDisplayInAppMessageNow
}
```
{% endtab %}
{% endtabs %}

### Receiving In-App Message in JavaScript

On the JavaScript side, this data can be used to instantiate a `BrazeInAppMessage`:
```javascript
this._listener = DeviceEventEmitter.addListener("inAppMessageReceived", function(event) {
    let inAppMessage = new ReactAppboy.BrazeInAppMessage(event.inAppMessage)
})
```

### Analytics

To log analytics using your `BrazeInAppMessage`, pass the instance into the desired analytics function:
- `logInAppMessageClicked`
- `logInAppMessageImpression`
- `logInAppMessageButtonClicked` (along with the button index)

For example:
```js
// Log a click
ReactAppboy.logInAppMessageClicked(inAppMessage);
// Log an impression
ReactAppboy.logInAppMessageImpression(inAppMessage);
// Log button index `0` being clicked
ReactAppboy.logInAppMessageButtonClicked(inAppMessage, 0);

```

A sample implementation of this is contained in AppboyProject, within the [React SDK][3]. Additional iOS implementation samples are contained in the [iOS SDK][4].

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/in-app_messaging/customization/#core-in-app-message-controller-delegate
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/in-app_messaging/customization/#custom-handling-in-app-message-display
[3]: https://github.com/Appboy/appboy-react-sdk
[4]: https://github.com/Appboy/appboy-ios-sdk

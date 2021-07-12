---
nav_title: In-App Messages
platform: React Native
subplatform: 
- Android
- FireOS
page_order: 2

page_type: reference
description: "This article covers in-app messages for Android or FireOS apps using React Native, including customizing and logging analytics."
channel: in-app messages

---

# In-App Messages

Native in-app messages display automatically on Android when using React Native.

## Customizing

### Accessing In-App Message Data
If you would like to access the in-app message data in the JavaScript layer, implement the `IInAppMessageManagerListener` as described in our public documentation [for Android][1]. In your `beforeInAppMessageDisplayed` implementation, you can access the `inAppMessage` data, send it to the JavaScript layer, and decide to show or not show the native message based on the return value (for more on these values, see our [Android documentation][2]).

```java
// In-app messaging
@Override
public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessage) {
    WritableMap parameters = new WritableNativeMap();
    parameters.putString("inAppMessage", inAppMessage.forJsonPut().toString());
    getReactNativeHost()
        .getReactInstanceManager()
        .getCurrentReactContext()
        .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
        .emit("inAppMessageReceived", parameters);
    // Note: return InAppMessageOperation.DISCARD if you would like
    // to prevent the Braze SDK from displaying the message natively.
    return InAppMessageOperation.DISPLAY_NOW;
}
```

### Receiving In-App Message in JavaScript

On the JavaScript side, this data can be used to instantiate a `BrazeInAppMessage`:
```js
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

A sample implementation of this is contained in AppboyProject, within the [React SDK][3]. Additional Android implementation samples are contained in the [Android SDK][4].

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/customization/#setting-a-custom-manager-listener
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/customization/#step-1-implement-an-in-app-message-manager-listener
[3]: https://github.com/Appboy/appboy-react-sdk
[4]: https://github.com/Appboy/appboy-android-sdk

---
nav_title: Other SDK Customizations
platform: iOS
description: "This document covers SDK customizations such as Log Level, IDFA Collection, and other customizations."
page_order: 3

---

# Other SDK Customizations

## Braze Log Level

The default LogLevel for the Braze iOS SDK is `8`. This level suppresses most logging so that no sensitive information is logged in a production released application.

To enable verbose logging for debugging, add a dictionary named `Braze` to your `Info.plist` file. Inside the `Braze` Dictionary, add the `LogLevel` String subentry and set the value to `0`. Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

LogLevel `0` is only intended to be used in development environments and should not be set in a released application.

Example `Info.plist` contents:

```
<key>Braze</key>
<dict>
	<key>LogLevel</key>
	<string>0</string>
</dict>
```

### Description of Log Levels

| LogLevel | Description |
|----------|-------------|
| 0        | All log information will be logged to the iOS console  |
| 8        | Default, minimal logging. |
{: .reset-td-br-1 .reset-td-br-2}

## Optional IDFA Collection

IDFA Collection is optional within the Braze SDK and disabled by default. IDFA Collection is only required within Braze if you intend to utilize our [install attribution integrations][21]. If you opt to store your IDFA, we will store it free of charge so you may take advantage of these options immediately upon release without additional development work.

As a result, we recommend continuing to collect the IDFA if you meet any of the following criteria:

- You are attributing app installation to a previously served advertisement
- You are attributing an action within the application to a previously served advertisement

### iOS 14 AppTrackingTransparency
In the future, Apple will require a new permission prompt in order to collect IDFA. For now, prompting for IDFA permission with `AppTrackingTransparency` is not required, but you should be prepared for a future iOS release from Apple which will require user opt-in.

When the `AppTrackingTransparency` prompt is required, in addition to implementing Braze's `ABKIDFADelegate` protocol, your application will need to request authorization using Apple's `ATTrackingManager` in the App Tracking Transparency framework. For more information, please reference this [Braze iOS 14 guide]({{site.baseurl}}/developer_guide/platform_integration_guides/ios/ios_14/#idfa-and-app-tracking-transparency), [Apple's Overview](https://developer.apple.com/app-store/user-privacy-and-data-use/), and [Apple's Developer Documentation](https://developer.apple.com/documentation/apptrackingtransparency). In iOS 14, collecting IDFA will require building with Xcode 12.

The prompt for App Tracking Transparency authorization also requires an `Info.plist` entry to explain your usage of the identifier:

```
<key>NSUserTrackingUsageDescription</key>
<string>To retarget ads and build a global profile to better serve you things you would like.</string>
```

### Implementing IDFA Collection

Follow these steps to implement IDFA Collection:

##### Step 1: Implement ABKIDFADelegate

Create a class that conforms to the [`ABKIDFADelegate`][29] protocol.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
#import "IDFADelegate.h"
#import <AdSupport/ASIdentifierManager.h>
#import <AppTrackingTransparency/AppTrackingTransparency.h>

@implementation IDFADelegate

- (NSString *)advertisingIdentifierString {
  return [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];
}

- (BOOL)isAdvertisingTrackingEnabledOrATTAuthorized {
  if (@available(iOS 14, *)) {
    return [ATTrackingManager trackingAuthorizationStatus] == ATTrackingManagerAuthorizationStatusAuthorized;
  }
  return [[ASIdentifierManager sharedManager] isAdvertisingTrackingEnabled];
}

@end
```

{% endtab %}
{% tab swift %}

```swift
import Appboy_iOS_SDK
import AdSupport
import AppTrackingTransparency

class IDFADelegate: NSObject, ABKIDFADelegate {
   func advertisingIdentifierString() -> String {
    return ASIdentifierManager.shared().advertisingIdentifier.uuidString
  }

  func isAdvertisingTrackingEnabledOrATTAuthorized() -> Bool {
    if #available(iOS 14, *) {
      return ATTrackingManager.trackingAuthorizationStatus ==  ATTrackingManager.AuthorizationStatus.authorized
    }
    return ASIdentifierManager.shared().isAdvertisingTrackingEnabled
  }
}
```
{% endtab %}
{% endtabs %}

##### Step 2: Set the delegate during Braze initialization

In the `appboyOptions` dictionary passed to `startWithApiKey:inApplication:withAppboyOptions:`, set the `ABKIDFADelegateKey` key to an instance of your `ABKIDFADelegate` conforming class.

## Approximate iOS SDK Size {#ios-sdk-size}

The approximate iOS SDK framework file size is 30MB and the approximate .ipa (addition to app file) size is between 1MB and 2MB.

Braze measures the size of our iOS SDK by observing the SDK's effect on `.ipa` size, per [Apple's recommendations on app sizing][31]. If you are calculating the iOS SDK's size addition to your application, we recommend following the steps under ["Getting an App Size Report"][31] to compare the size difference in your `.ipa` before and after integrating the Braze iOS SDK. When comparing sizes from the App Thinning Size Report, we also recommend looking at app sizes for thinned `.ipa` files, as universal `.ipa` files will be larger than the binaries downloaded from the App Store and installed onto user devices.

> If you are integrating via CocoaPods with `use_frameworks!`, set `Enable Bitcode = NO` in target's Build Settings for accurate sizing.


[21]: {{site.baseurl}}/partners/advertising_technologies/attribution/adjust/
[29]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKIDFADelegate.h
[31]: https://developer.apple.com/library/content/qa/qa1795/_index.html

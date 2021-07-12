---
nav_title: Carthage Integration
platform: iOS
page_order: 0.1
description: "This reference article shows how to integrate the Braze SDK using Carthage for iOS."

---

# Carthage Integration

Starting from version 3.24.0 of the SDK, you can integrate the Braze SDK using Carthage by including the following in your `Cartfile`:
```
binary "https://raw.githubusercontent.com/Appboy/appboy-ios-sdk/master/appboy_ios_sdk_full.json"
```

You can integrate earlier versions of the SDK by including the following in your `Cartfile`:
```
github "Appboy/Appboy-iOS-SDK" "<BRAZE_IOS_SDK_VERSION>"
```

Make sure to replace `<BRAZE_IOS_SDK_VERSION>` with the latest version of the Braze iOS SDK in "x.y.z" format. Release versions are available [here](https://github.com/Appboy/appboy-ios-sdk/releases).

For further instructions using Carthage, please refer to their [user guide][9] on Github.

Once you've synced the Braze SDK release artifacts (we support Carthage via a zip of release artifacts attached to our Github releases), integrate the `Appboy_iOS_SDK.framework` and `SDWebImage.framework` into your project. Then, in your Application delegate do:


{% tabs %}
{% tab OBJECTIVE-C %}

Add the following line of code to your `AppDelegate.m` file:

```objc
#import <Appboy-iOS-SDK/AppboyKit.h>
```

Within your `AppDelegate.m` file, add the following snippet within your `application:didFinishLaunchingWithOptions` method:

```objc
[Appboy startWithApiKey:@"YOUR-API-KEY"
         inApplication:application
     withLaunchOptions:launchOptions];
```

{% endtab %}
{% tab swift %}

If you are integrating the Braze SDK with CocoaPods or Carthage, add the following line of code to your `AppDelegate.swift` file:

```swift
import Appboy_iOS_SDK
```

For more information about using Objective-C code in Swift projects, please see the [Apple Developer Docs](https://developer.apple.com/library/ios/documentation/swift/conceptual/buildingcocoaapps/MixandMatch.html).

In `AppDelegate.swift`, add following snippet to your `application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool`:

```swift
Appboy.start(withApiKey: "YOUR-API-KEY", in:application, withLaunchOptions:launchOptions)
```

__Note__: Braze's `sharedInstance` singleton will be nil before `startWithApiKey:` is called, as that is a prerequisite to using any Braze functionality.

{% endtab %}
{% endtabs %}

{% alert important %}
Be sure to update `YOUR-API-KEY` with the correct value from your **Settings** page.
{% endalert %}

{% alert warning %}
Be sure to initialize Braze in your application's main thread. Initializing asynchronously can lead to broken functionality.
{% endalert %}

## Dependency-Free Integration
If you want to use SDWebImage in your project along with the Braze SDK, you can install a thin version of the Braze Carthage framework. To do so, include the following lines in your Cartfile:

```
binary "https://raw.githubusercontent.com/Appboy/appboy-ios-sdk/master/appboy_ios_sdk.json"
github "SDWebImage/SDWebImage"
```

## Core Only Integration
If you want to use the Core SDK without any UI components, you can install the core version of the Braze Carthage framework by including the following line in your Cartfile:

```
binary "https://raw.githubusercontent.com/Appboy/appboy-ios-sdk/master/appboy_ios_sdk_core.json"
```

## Specifying Your Custom Endpoint or Data Cluster

Your Braze representative should have already advised you of the [correct endpoint]({{site.baseurl}}/user_guide/administrative/access_braze/sdk_endpoints/).

### Compile-time Endpoint Configuration (Recommended)

{% alert note %}
Note that as of December 2019, custom endpoints are no longer given out, if you have a pre-existing custom endpoint, you may continue to use it. For a list of our available endpoints, <a href="{{site.baseurl}}/api/basics/#endpoints">click here</a>.
{% endalert %}

If given a pre-existing custom endpoint...
- Starting with Braze iOS SDK v3.0.2, you can set a custom endpoint using the `Info.plist` file. Add the Braze dictionary to your `Info.plist` file. Inside the `Braze` dictionary, add the `Endpoint` string subentry and set the value to your custom endpoint URLs authority (for example, `sdk.iad-01.braze.com`, not `https://sdk.iad-01.braze.com`). Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

### Runtime Endpoint Configuration

If given a pre-existing custom endpoint...
- Starting with Braze iOS SDK v3.17.0+, you can override set your endpoint via the `ABKEndpointKey` inside the `appboyOptions` parameter passed to `startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions:`. Set the value to your custom endpoint URLs authority (for example, `sdk.iad-01.braze.com`, not `https://sdk.iad-01.braze.com`).

{% alert note %}
Support for setting endpoints at runtime using `ABKAppboyEndpointDelegate` has been removed in Braze iOS SDK v3.17.0. If you already use `ABKAppboyEndpointDelegate`, note that in Braze iOS SDK versions v3.14.1 to v3.16.0, any reference to `dev.appboy.com` in your `getApiEndpoint()` method must be replaced with a reference to `sdk.iad-01.braze.com`.
{% endalert %}

{% alert important %}
To find out your specific cluster or custom endpoint, please ask your Customer Success Manager or reach out to our support team.
{% endalert %}

[9]: https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos

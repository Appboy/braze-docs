---
nav_title: CocoaPods Integration
platform: iOS
page_order: 1
description: "This reference article shows how to integrate the Braze SDK using CocoaPods for iOS."

---

# CocoaPods Integration

## Step 1: Install CocoaPods

Installing the iOS SDK via [CocoaPod][apple_initial_setup_1] automates the majority of the installation process for you. Before beginning this process please ensure that you are using [Ruby version 2.0.0][apple_initial_setup_2] or greater. Don't worry, knowledge of Ruby syntax isn't necessary to install this SDK.

Simply run the following command to get started:

```bash
$ sudo gem install cocoapods
```

__Note__: If you are prompted to overwrite the `rake` executable please refer to the [Getting Started Directions on CocoaPods.org][apple_initial_setup_3] for further details.

__Note__: If you have issues regarding CocoaPods, please refer to the [CocoaPods Troubleshooting Guide][apple_initial_setup_25].

## Step 2: Constructing the Podfile

Now that you've installed the CocoaPods Ruby Gem, you're going to need to create a file in your Xcode project directory named `Podfile`.

Add the following line to your Podfile:

```
target 'YourAppTarget' do
  pod 'Appboy-iOS-SDK'
end
```

__Note__: We suggest you version Braze so pod updates automatically grab anything smaller than a minor version update. This looks like 'pod 'Appboy-iOS-SDK' ~> Major.Minor.Build'. If you want to integrate the latest version of Braze SDK automatically even with major changes, you can use `pod 'Appboy-iOS-SDK'` in your Podfile.

> We recommend that integrators import our full SDK as outlined above. However, if you are certain that you are only going to integrate a particular Braze feature then you have the option to import just the desired UI subspec instead of the full SDK.

| Subspec | Details |
| ------- | ------- |
| `pod 'Appboy-iOS-SDK/InAppMessage'` | The `InAppMessage` subspec contains the Braze In-App Message UI and the Core SDK.|
| `pod 'Appboy-iOS-SDK/ContentCards'` | The `ContentCards` subspec contains the Braze Content Card UI and the Core SDK. |
| `pod 'Appboy-iOS-SDK/NewsFeed'` | The `NewsFeed` subspec contains the Braze News Feed UI and the Core SDK. |
| `pod 'Appboy-iOS-SDK/Core'` | The `Core` subspec contains support for analytics, such as custom events and attributes. |
{: .ws-td-nw-1}

## Step 3: Installing the Braze SDK

To install the Braze SDK Cocoapod, navigate to the directory of your Xcode app project within your terminal and run the following command:
```
pod install
```

At this point, you should be able to open the new Xcode project workspace created by CocoaPods.

![New Workspace][apple_initial_setup_15]

## Step 4: Updating your App Delegate

{% tabs %}
{% tab OBJECTIVE-C %}

Add the following line of code to your `AppDelegate.m` file:

```objc
#import "Appboy-iOS-SDK/AppboyKit.h"
```

Within your `AppDelegate.m` file, add the following snippet within your `application:didFinishLaunchingWithOptions` method:

```objc
[Appboy startWithApiKey:@"YOUR-APP-IDENTIFIER-API-KEY"
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
Appboy.start(withApiKey: "YOUR-APP-IDENTIFIER-API-KEY", in:application, withLaunchOptions:launchOptions)
```

__Note__: Braze's `sharedInstance` singleton will be nil before `startWithApiKey:` is called, as that is a prerequisite to using any Braze functionality.

{% endtab %}
{% endtabs %}

{% alert important %}
Be sure to update `YOUR-APP-IDENTIFIER-API-KEY` with the correct value from your **Settings** page. For more information on where to find your App Identifier API key, check out our [API documentation]({{site.baseurl}}/api/api_key/#the-app-identifier-api-key).
{% endalert %}

{% alert warning %}
Be sure to initialize Braze in your application's main thread. Initializing asynchronously can lead to broken functionality.
{% endalert %}


## Step 5: Specify Your Data Cluster

{% alert note %}
Note that as of December 2019, custom endpoints are no longer given out, if you have a pre-existing custom endpoint, you may continue to use it. For a list of our available endpoints, <a href="{{site.baseurl}}/api/basics/#endpoints">click here</a>.
{% endalert %}

### Compile-time Endpoint Configuration (Recommended)

If given a pre-existing custom endpoint...
- Starting with Braze iOS SDK v3.0.2, you can set a custom endpoint using the `Info.plist` file. Add the Braze dictionary to your `Info.plist` file. Inside the `Braze` dictionary, add the `Endpoint` string subentry and set the value to your custom endpoint URL's authority (for example, `sdk.iad-01.braze.com`, not `https://sdk.iad-01.braze.com`). Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

Your Braze representative should have already advised you of the [correct endpoint]({{site.baseurl}}/user_guide/administrative/access_braze/sdk_endpoints/).

### Runtime Endpoint Configuration

If given a pre-existing custom endpoint...
- Starting with Braze iOS SDK v3.17.0+, you can override set your endpoint via the `ABKEndpointKey` inside the `appboyOptions` parameter passed to `startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions:`. Set the value to your custom endpoint URL's authority (for example, `sdk.iad-01.braze.com`, not `https://sdk.iad-01.braze.com`).

{% alert important %}
To find out your specific cluster, please ask your Customer Success Manager or reach out to our support team.
{% endalert %}

## SDK Integration Complete

Braze should now be collecting data from your application and your basic integration should be complete. {% if include.platform == 'iOS' %}Please see the following sections in order to enable custom event tracking, push messaging, the news-feed and the complete suite of Braze features.{% endif %}

## Updating the Braze SDK via CocoaPods

To update a Cocoapod simply run the following commands within your project directory:

```
pod update
```

## Customizing Braze On Startup

If you wish to customize Braze on startup, you can instead use the Braze initialization method `startWithApiKey:inApplication:withLaunchOptions:withAppboyOptions` and pass in an optional `NSDictionary` of Braze startup keys.
{% tabs %}
{% tab OBJECTIVE-C %}

In your `AppDelegate.m` file, within your `application:didFinishLaunchingWithOptions` method, add the following Braze method:

```objc
[Appboy startWithApiKey:@"YOUR-APP-IDENTIFER-API-KEY"
          inApplication:application
      withLaunchOptions:launchOptions
      withAppboyOptions:appboyOptions];
```

{% endtab %}
{% tab swift %}

In `AppDelegate.swift`, within your `application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool` method, add the following Braze method:

```swift
Appboy.start(withApiKey: "YOUR-APP-IDENTIFIER-API-KEY",
                 in:application,
                 withLaunchOptions:launchOptions,
                 withAppboyOptions:appboyOptions)
```

where `appboyOptions` is a `Dictionary` of startup configuration values.

{% endtab %}
{% endtabs %}

__Note__: This method would replace the `startWithApiKey:inApplication:withLaunchOptions:` initialization method from above.

This method is called with the following parameters:

- `YOUR-APP-IDENTIFIER-API-KEY` – Your [App Identifier]({{site.baseurl}}/api/api_key/#the-app-identifier-api-key) API Key from the Braze Dashboard.
- `application` – The current app
- `launchOptions` – The options `NSDictionary` that you get from `application:didFinishLaunchingWithOptions:`
- `appboyOptions` – An optional `NSDictionary` with startup configuration values for Braze

See [Appboy.h][apple_initial_setup_5] for a list of Braze startup keys.

## Appboy.sharedInstance() and Swift nullability
Differing somewhat from common practice, the `Appboy.sharedInstance()` singleton is optional. The reason for this is that, as noted above, `sharedInstance` is `nil` before `startWithApiKey:` is called, and there are some non-standard but not-invalid implementations in which a delayed initialization can be used.

If you call `startWithApiKey:` in your `didFinishLaunchingWithOptions:` delegate before any access to Appboy's `sharedInstance` (the standard implementation), you can use optional chaining, like `Appboy.sharedInstance()?.changeUser("testUser")`, to avoid cumbersome checks. This will have parity with an Objective-C implementation that assumed a non-null `sharedInstance`.

[apple_initial_setup_1]: http://cocoapods.org/
[apple_initial_setup_2]: https://www.ruby-lang.org/en/installation/
[apple_initial_setup_3]: http://guides.cocoapods.org/using/getting-started.html "CocoaPods Installation Directions"
[apple_initial_setup_5]: https://github.com/braze-inc/braze-ios-sdk/blob/master/AppboyKit/include/Appboy.h
[apple_initial_setup_15]: {% image_buster /assets/img_archive/podsworkspace.png %}
[apple_initial_setup_25]: http://guides.cocoapods.org/using/troubleshooting.html "CocoaPods Troubleshooting Guide"

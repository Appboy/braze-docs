---
nav_title: Initial SDK Setup
platform: React Native
subplatform: iOS
page_order: 0

page_type: reference
description: "This article covers initial SDK setup steps for iOS apps using React Native."

---

# Initial SDK Setup

Installing the Braze SDK will provide you with analytics functionality, as well as push and in-app messages.

## Step 1: Get the SDK

### iOS with podspec
1. `npm install react-native-appboy-sdk@latest --save`
2. Add `pod 'react-native-appboy-sdk', :path => '../node_modules/react-native-appboy-sdk'` to your app target in your Podfile. Note that the `react-native-appboy-sdk` pod has a dependency on the `React` pod.
3. Run `pod install` from your `ios` directory.

### iOS without podspec

1.  Install the Braze iOS SDK into your iOS project.  See instructions for CocoaPods [here][1]. For manual integration, please check [here][2].  See notes below for further information.
2. `npm install react-native-appboy-sdk@latest --save`
3. In the Xcode's "Project navigator", right-click on your project's Libraries folder ➜ `Add Files to <...>`
4. Go to `node_modules` ➜ `react-native-appboy-sdk` ➜ `ios` ➜ `AppboyReactBridge` ➜ select `AppboyReactBridge.xcodeproj`
5. Add `libAppboyReactBridge.a` to `Build Phases -> Link Binary With Libraries`
6. Update the 'Header Search Paths' in the AppboyReactBridge Xcode project to reference the headers directory of your installation of the Braze iOS SDK.

### iOS Completing the Integration
1.  Complete your [iOS SDK integration][3].  You must pass your Braze API key to the SDK in `startWithApiKey` in your App delegate's `didFinishLaunchingWithOptions:` method. While custom endpoints are no longer assigned by Braze, if you have been given a pre-existing custom endpoint, you need to set up your custom endpoint in your `Info.plist` file.
2.  When you need to make Braze calls from JavaScript, use the following declaration to import the JavaScript module:

```javascript
const ReactAppboy = require('react-native-appboy-sdk');
```

#### Note on Deep Links on iOS

For deep links to work on iOS from a cold start, you will need to add the following to your App delegate file's `didFinishLaunchingWithOptions` method:

{% tabs %}
{% tab OBJECTIVE-C %}
```objc
[[AppboyReactUtils sharedInstance] populateInitialUrlFromLaunchOptions:launchOptions];
```
{% endtab %}
{% tab swift %}
```swift
AppboyReactUtils.sharedInstance()?.populateInitialUrl(fromLaunchOptions: launchOptions)
```
{% endtab %}
{% endtabs %}



[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/initial_sdk_setup/cocoapods/#cocoapods-integration
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/advanced_use_cases/manual_sdk_integration/
[3]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/initial_sdk_setup/

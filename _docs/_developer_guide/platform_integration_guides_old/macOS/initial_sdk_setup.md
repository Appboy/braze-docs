---
nav_title: Initial SDK Setup
platform: MacOS
page_order: 0

page_type: reference
description: "This page provides resources for initial SDK setup steps on macOS."

---

# Initial SDK Setup

As of version [3.32.0][1], the Braze SDK supports macOS for apps using [Mac Catalyst][2] when integrating through Swift Package Manager. Currently, the SDK does not support Mac Catalyst when using Cocoapods or Carthage.

{% alert note %}
To build your app with Mac Catalyst, please reference <a href="https://developer.apple.com/documentation/uikit/mac_catalyst">Apple's documentation here</a>.
{% endalert %}

Once your app supports Catalyst, follow [these instructions to use Swift Package Manager][3] to import the Braze SDK into your app.

## Supported Features

Braze supports push notifications and location features when running on Mac Catalyst. To integrate push in your Catalyst app, follow the [iOS SDK instructions here][4]. For enabling automatic location collection, follow the [setup instructions here][5].

Please note that Push Stories, Rich Push, and Geofences are not supported on MacOS.

[1]:https://github.com/Appboy/appboy-ios-sdk/releases/tag/3.32.0
[2]:https://developer.apple.com/mac-catalyst/
[3]:{{site.baseurl}}/developer_guide/platform_integration_guides/ios/initial_sdk_setup/swift_package_manager/
[4]:{{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/integration/
[5]:{{site.baseurl}}/developer_guide/platform_integration_guides/ios/analytics/location_tracking/

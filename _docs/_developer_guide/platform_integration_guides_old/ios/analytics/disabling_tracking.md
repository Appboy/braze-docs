---
nav_title: Disabling iOS SDK Tracking
platform: iOS
page_order: 8
description: "This article shows how to disable data collection for your iOS application."

---

# Disabling Data Collection

In order to comply with data privacy regulations, data tracking activity on the iOS SDK can be stopped entirely using the method [`disableSDK`](http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#a8d3b78a98420713d8590ed63c9172733). This method will cause all network connections to be canceled, and the Braze SDK will not pass any data to Braze's servers. If you wish to resume data collection at a later point in time, you can use the [`requestEnableSDKOnNextAppRun`](http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#a781078a40a3db0de64ac82dcae3b595b) method in the future to resume data collection.

Additionally, you can use the method [`wipeDataAndDisableForAppRun`](http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#ac8d580f60ec0608cd91240a8a3aa23a3) to fully clear all client-side data stored on the device.

Unless a user uninstalls *all* apps from a vendor on a given device, the next Braze SDK/app runs after calling `wipeDataAndDisableForAppRun()` will result in our server re-identifying that user via their device identifier (IDFV). In order to fully remove all user data, you should combine a call to `wipeDataAndDisableForAppRun` with a request to delete data on the server via the [Braze REST API]({{site.baseurl}}/developer_guide/rest_api/user_data/#user-delete-endpoint).

---
nav_title: iOS 15 Upgrade Guide
page_order: 7
platform: iOS
description: "This reference article covers the iOS 15 SDK update, highlighting changes such as geofences, location targeting, IDFA, and more."

---

# iOS 15 SDK Upgrade Guide

> This guide outlines changes introduced in iOS 15 (WWDC21) and the required upgrade steps for your Braze iOS SDK integration.

> For a complete list of new iOS 15 updates, see Apple's [iOS 15 Page](https://www.apple.com/ios/ios-15/).

{% alert important %}
This is a working document and will evolve as Apple releases new Beta versions and information on upcoming changes.
{% endalert %}

<!-- ## Upgrade Requirements

<style>
table th:nth-child(1),
table th:nth-child(2),
table td:nth-child(1),
table td:nth-child(2) {
    min-width:230px;
}
table td {
    word-break: break-word;
}
</style>

|If Your App Uses:|Upgrade Recommendation|Description|
|------|--------|---|
|Xcode 13|**Upgrade to iOS SDK v4.0 or above**|Customers using Xcode 13 must use [v4.0.0+][1] for compatibility. If you experience any issues or questions related to our iOS 15 compatibility, please open a new [Github Issue][2].| -->

## iOS 15 Changes

### Location Buttons

iOS 15 introduces a new convenient way for users to temporarily allow location access within an app. 

The new Location Button builds off of the existing "Allow Once" permission, but without repetitive prompts when clicked multiple times in the same session.

For more information, watch Apple's [Meet The Location Button](https://developer.apple.com/videos/play/wwdc2021/10102/) video from this year's WWDC conference.

#### Using Location Buttons with Braze

No additional integration is required when using Location Buttons with Braze. Your app should continue passing a user's location - once they've granted permission - as usual.

According to Apple, users who have already shared background location access or "While Using App" will continue to grant that level of permission, and will not be restricted after upgrading to iOS 15.

### New Notification Settings

#### Focus Settings

iOS 15 users can now create "Focus Settings" - custom profiles used to determine the types of notifications that can __break through__ with prominent notifications.

![Focus Settings]({% image_buster /assets/img/ios/ios15-focus-settings.png %}){: style="float:right;max-width:25%;margin-left:15px;border:0"}

Starting in iOS 15, push notifications can be sent with one of four interuption levels:

* Passive (new) - No sound, no vibration, no screen waking, no breakthrough of focus settings
* Active (default) - Allows sound, vibration, screen waking, no breakthrough of focus settings
* Time Sensitive (new) - Allows sound, vibration, screen waking, can breakthrough system controls if allowed
* Critical - Allows sound, vibration, screen waking, can breakthrough system controls and bypass ringer switch

### Mail Privacy Protection

### Hide My Email

### Safari IP Address Location

In iOS 15, users will be able to configure Safari to either anonymize or generalize the location that can be  IP Addresses while browsing the web.

### iCloud Private Relay

### 

[1]: https://github.com/Appboy/appboy-ios-sdk/releases/tag/4.0.0
[2]: https://github.com/Appboy/appboy-ios-sdk/issues

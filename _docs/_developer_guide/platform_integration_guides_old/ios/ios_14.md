---
nav_title: iOS 14 Upgrade Guide
page_order: 7
platform: iOS
description: "This reference article covers the iOS 14 SDK update, highlighting changes such as geofences, location targeting, IDFA, and more."

---

# iOS 14 SDK Upgrade Guide

This guide describes Braze-related changes introduced in iOS 14 and the required upgrade steps for your Braze iOS SDK integration.

For a complete list of new iOS 14 updates, see Apple's [iOS 14 Page](https://www.apple.com/ios/ios-14/).

{% alert tip %}
As of iOS 14.5, **IDFA** collection and [certain data sharing](https://developer.apple.com/app-store/user-privacy-and-data-use/#permission-to-track) will require the new [AppTrackingTransparency](https://developer.apple.com/documentation/apptrackingtransparency) Framework permission prompt ([Learn More](#idfa)).
{% endalert %}

#### Summary of iOS 14 breaking changes

- Apps targeting iOS 14 / Xcode 12 must use our [official iOS 14 release][1].
- Geofences are [no longer supported by iOS][4] for users who choose the new  _approximate location_ permission.
- Use of the "Last Known Location" targeting features will require an upgrade to Braze iOS SDK v3.26.1+ for compatibility with _approximate location_ permission. Note that if you are using XCode 12, you will need to upgrade to at least v3.27.0.
- As of iOS 14.5, IDFA collection and [certain data sharing][5] require the new [AppTrackingTransparency](https://developer.apple.com/documentation/apptrackingtransparency) Framework permission prompt.
- If you use the “Ad Tracking Enabled” field for campaign targeting or analytics, you will need to upgrade to Xcode 12 and use the new AppTrackingTransparency framework to report end users’ opt-in status.

## Upgrade Summary

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
|Xcode 12|**Upgrade to iOS SDK v3.27 or above**|Customers using Xcode 12 must use v3.27.0+ for compatibility. If you experience any issues or questions related to our iOS 14 compatibility, please open a new [Github Issue][2].|
|Most Recent Location| **Upgrade to iOS SDK v3.26.1 or above**|If you use the Most Recent Location targeting feature and are still using XCode 11, you should upgrade to at least iOS SDK v3.26.1 which supports the new  _Approximate Location_ feature. Older SDKs will not be able to reliably collect location when a user upgrades to iOS 14 _and_ choose Approximate Location.<br><br>Even though your app might not target iOS 14, your end users may upgrade to iOS 14 and begin to use the new location accuracy option. Apps that do not upgrade to iOS SDK v3.26.1+ will not be able to reliably collect location attributes when users provide their _approximate location_  on iOS 14 devices.|
|IDFA Ad Tracking ID| **Upgrade to Xcode 12 and iOS SDK v3.27 may be required**|Sometime in 2021, Apple will begin to require a permission prompt for the collection of the IDFA. At that time, apps must upgrade to Xcode 12 and use the new `AppTrackingTransparency` framework in order to continue collecting IDFA. If you pass IDFA to the Braze SDK you must also upgrade to v3.27.0+ at that time.<br><br>Apps that do not use the new iOS 14 APIs will be unable to collect IDFA, and will instead collect a blank ID (`00000000-0000-0000-0000-000000000000`) once Apple begins to enforce this change in 2021. For more information on whether or not this applies to your app, [read the details below](#idfa).|


## iOS 14 Behavior Changes

### Approximate Location Permission

![Precise Location]({% image_buster /assets/img/ios/ios14-approximate-location.png %}){: style="float:right;max-width:45%;margin-left:15px;"}

#### Overview

When requesting location permission, users will now have a choice to provide their _precise location_ (previous behavior), or the new _approximate location_. Approximate location will return a larger radius in which the user is located, instead of their exact coordinates.

#### Geofences {#geofences}

Geofences are [no longer supported by iOS][4] for users who choose the new  _approximate location_ permission. While no updates are required for your Braze SDK integration, you may need to adjust your [location-based marketing strategy](https://www.braze.com/blog/geofencing-geo-targeting-beaconing-when-to-use/) for campaigns that rely on geofences.

#### Location Targeting {#location-tracking}

To continue to collect users' _last known location_ when _approximate location_ is granted, your app will need to upgrade to at least v3.26.1 of the Braze iOS SDK. Keep in mind that the location will be less precise, and based on our testing has been upwards of 12,000 meters (7+ miles). When using the _last known location_ targeting options in the Braze Dashboard, be sure to increase the location's radius to account for new _approximate locations_ (we recommend at least a 1 mile/1.6km radius).

Apps that do not upgrade the Braze iOS SDK to at least v3.26.1 will no longer be able to use location tracking when _approximate location_ is granted on iOS 14 devices.

Users who have already granted location access will continue to provide _precise location_ after upgrading.

Note that if you are using XCode 12, you will need to upgrade to at least v3.27.0.

For more information on Approximate Location, see Apple's [What's New In Location](https://developer.apple.com/videos/play/wwdc2020/10660/) WWDC Video.

### IDFA and App Tracking Transparency {#idfa}

#### Overview

IDFA (Identifier for Advertisers) is an identifier provided by Apple for use with advertising and attribution partners for cross-device tracking and is tied to an individual's Apple ID.

Starting in iOS 14.5, a new permission prompt (launched by the new `AppTrackingTransparency` framework) must be shown to collect explicit user consent for IDFA. This permission prompt to "track you across apps and websites owned by other companies" will be requested similarly to how you’d prompt users to request their location.

If a user does not accept the prompt, or if you do not upgrade to Xcode 12's `AppTrackingTransparency` framework, then a blank IDFA value (`00000000-0000-0000-0000-000000000000`) will be returned, and your app will not be allowed to prompt the user again.

{% alert important %}
These IDFA updates will take effect once end-users upgrade their device to iOS 14.5. Please ensure your app uses the new `AppTransparencyFramework` with Xcode 12 if you plan to collect IDFA.
{% endalert %}

#### Changes to Braze IDFA collection
![IDFA]({% image_buster /assets/img/ios/ios14-idfa.png %}){: style="float:right;max-width:25%;margin-left:15px;border:0"}

1. Braze will continue to allow apps to provide a user's IDFA value _to_ the Braze SDK.

2. The `ABK_ENABLE_IDFA_COLLECTION` compilation macro, which would conditionally compile in optional automatic IDFA collection, will no longer function in iOS 14 and has been removed in 3.27.0. 

3. If you use the “Ad Tracking Enabled” field for campaign targeting or analytics, you will need to upgrade to Xcode 12 and use the new AppTrackingTransparency framework to report end users’ opt-in status. The reason for this change is that in iOS 14, the old [`advertisingTrackingEnabled`](https://developer.apple.com/documentation/adsupport/asidentifiermanager/1614148-advertisingtrackingenabled) field will always return No.

4. If your app has used IDFA or IDFV as your Braze External ID, we strongly recommend migrating away from these identifiers in favor of a UUID. For more information on migrating External IDs, see our new [External ID Migration API Endpoint]({{site.baseurl}}/api/endpoints/user_data/external_id_migration/).

Read more from Apple about their [Privacy Updates](https://developer.apple.com/app-store/user-privacy-and-data-use/) and the new [App Tracking Transparency framework](https://developer.apple.com/documentation/apptrackingtransparency).

### Push Authorization {#push-provisional-auth}

{% alert important %}
No changes to Provisional Push Authorization are included in iOS 14. In an earlier beta version of iOS 14, Apple introduced a change which has since been reverted back to prior behavior.
{% endalert %}


## iOS 14 New Features

### App Privacy and Data Collection Overview {#app-privacy}

Since Dec 8, 2020, all submissions to the App Store require additional steps to adhere to [Apple's new App Privacy standards](https://developer.apple.com/app-store/app-privacy-details/).

#### Apple Developer Portal Questionnaire

On the _Apple Developer Portal_:
* You will be asked to fill out a questionnaire to describe how your app or third-party partners collect data.
  * The questionnaire is expected to always be up-to-date with your most recent release in the App Store.
  * The questionnaire may be updated even without a new app submission.
* You will be required to paste a link to your app's Privacy Policy URL.

As you fill out your questionnaire, please consult your legal team, and consider how your usage of Braze for the following fields may affect your disclosure requirements.

#### Braze Default Data Collection
**Identifiers** - An anonymous device identifier is always collected by the Braze SDK. This is currently set to the device IDFV (identifier for vendor).

**Usage Data** - This can include Braze’s session data, as well as any event or attribute collection you use to measure product interaction.

#### Optional Data Collection
Data you may optionally be collecting through your usage of Braze:

**Location** - Both Approximate Location and Precise Location can optionally be collected by the Braze SDK. These feature are disabled by default.

**Contact Info** - This can include events and attributes related to the user's identity.

**Purchases** - This can include events and purchases logged on behalf of the user.

{% alert important %}
Note that this is not an exhaustive list. If you manually collect other information about your users in Braze that apply to other categories in the App Privacy Questionnaire, you will need to disclose those as well.
{% endalert %}

To learn more about this feature, see [Apple's Privacy and Data Use](https://developer.apple.com/app-store/user-privacy-and-data-use/).

[1]: https://github.com/Appboy/appboy-ios-sdk/releases/tag/3.27.0
[2]: https://github.com/Appboy/appboy-ios-sdk/issues
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager/3600215-accuracyauthorization
[5]: https://developer.apple.com/app-store/user-privacy-and-data-use/#permission-to-track

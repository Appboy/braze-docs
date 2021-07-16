---
nav_title: Data Collected by Default
page_order: 1

page_type: reference
description: "This reference article addresses the data that is collected by default from the Braze SDK."
tool: Dashboard
---

# Data Collected by Default

Braze is designed to allow for flexible data collection via our SDKs and APIs. For ease of integration, a number of fields are automatically tracked or stored once the SDKs are initialized; those fields are listed below:

## Data Collected by Default

### Attributes that can be Disabled

| Attribute | Platform | Description | Why it's Collected |
| --------- | -------- | ----------- | ------------------ |
| Browser Name | Web | The name of the browser. | Used to ensure messages are only sent to compatible browsers. Can also be used for browser-based segmentation. |
| Time Zone | Web, iOS, Android | The device/browser time zone. | Used to ensure messages are sent at the appropriate time, according to each user’s local time zone. |
| Resolution | Web, iOS, Android | The device/browser resolution. | Can optionally be used for device-based message targeting. |
| Browser Language | Web | The browser language. | Used when translating messages to a user’s preferred language. |
| User Agent | Web | The [user agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent). | Used to ensure messages are only sent to compatible devices. Can also be used within segmentation. |
| Device Locale | iOS, Android | The default locale of the device. | Used when translating messages to a user’s preferred language. |
| Device Model | iOS, Android | The specific hardware of the device. | Used to ensure messages are only sent to compatible devices. Can also be used within segmentation. |
| Device Wireless Carrier | iOS, Android | The mobile carrier. | Can optionally be used for message targeting. |
| IDFA | iOS | The advertiser identifier. | This value is only sent to Braze when (1) users opt-in; and (2) the IDFA is set in the Braze SDK integration. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Attributes that cannot be Disabled

| Attribute | Platform | Description | Why it's Collected |
| --------- | -------- | ----------- | ------------------ |
| OS and OS Version | Web, iOS, Android | The current reported device/browser and device/browser version. | Used to ensure messages are only sent to compatible devices. Can also be used within segmentation to target users to upgrade app versions. |
| IDFV | iOS | Vendor Identifier. | Used to identify anonymous users, differentiate users' devices, and ensure messages are sent to the correct intended device.  |
| Device ID | Web, iOS, Android | Device Identifier, an out-of-the-box randomly generated string. | Used to identify anonymous users, differentiate users' devices, and ensure messages are sent to the correct intended device. |
| Session ID & Session Timestamp | Web, iOS, Android | Session Identifier and Session Timestamp an out-of-the-box randomly generated string. | Used to determine if the user is starting a new or existing session, the re-eligibility of messages intended for this user, and calculate session analytics.<br><br>Certain channels like in-app messages and Content Cards are transmitted to the device upon session start. Our backend will then use data related to when it last contacted our servers (which the device stores and sends back) to know if new data should be sent to the device.|
| SDK Version | Web, iOS, Android | The current SDK Version. | Used to ensure messages are only sent to compatible devices, and to ensure no disruption of the service.<br><br>The SDK version is used in order to identify the devices in place of a device ID on iOS. This is being removed in future iOS SDK versions and replaced with a randomly generated string. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

In addition to this tracked and stored data, Braze generates campaign interaction data and derives data based on tracked data, i.e., total sessions are derived from tracking multiple session start events. 

## Strictly Necessary Data Collection

In addition to ease of integration, the Braze SDKs are designed to be configurable to remove automatically captured fields, per the following: 

- Android: [whitelisting fields](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/enums/DeviceKey.html) 
- iOS: [whitelisting fields](https://github.com/Appboy/appboy-ios-sdk/blob/16e893f2677af7de905b927505d4101c6fb2091d/AppboyKit/headers/AppboyKitLibrary/Appboy.h#L181) 
- Web: [whitelisting fields](https://js.appboycdn.com/web-sdk/2.3/doc/module-appboy.html#toc2)

Listed below is the strictly necessary data generated and received by Braze when an integrator chooses to initialize the SDK for communication. These elements are non-configurable and are essential in out-of-the-box platform functions. 

When the SDK is initialized:
- Braze generates:
    - __OS and OS Version, IDFV, Device ID, Session ID & Session Timestamp, and SDK Version__: Please refer to the chart above to see why these generated attributes are non configurable.
    - __Analytics Relating to SDK Messages__ (In-app message impressions, in-app message buttons, push direct opens (Android, Web)): This information is required to ensure communication isn't resent once received. These analytics are only generated once a campaign has been sent to a user. The receipt of these analytics can be the source of further downstream messages. For example, a Canvas that is "send a push notification that when clicked on, advances a user down <this branch> of a Canvas" is a common use case. Similarly with in-app message impressions, they can be used to advance a user through a journey. Not sending these will compromise our product.
- Braze receives from the backend and stores:
    - __In-App Messages, Content Cards, News Feed__: Displays mechanisms for communications sent through these channels. This is only done when a campaign has been sent to a user.

Braze Service/Backend Generates:
1. Calculated Metrics (Number of sessions over time, conversion events, random bucket numbers, etc.)
2. Analytics related to non-SDK messages (i.e., SMS sends)

To implement the following minimal SDK integration, configurable device properties listed above must be disabled. These disabled settings can be found and initialized [here]({{site.baseurl}}/developer_guide/platform_integration_guides/web/cookies_and_storage/#device-properties).

{% alert important %}
While this data is critical for communication, you may opt to use certain elements for other Braze features, like retargeting, that might warrant approval of consent for more privacy-conscious clients.
{% endalert %} 

### Additional Data Collection 

To track data that is not listed above, please follow our documentation on tracking custom attributes and custom events. 

## Partner Considerations
If you are interested in minimal data collection and integrate with mParticle and Segment on:
- Mobile Platforms: You must manually update the code for these configurations, mParticle and Segment do not offer a way to do this through their platform. 
- Web: Minimal data collection is not possible due to the Braze Web SDK being dynamically loaded, making initialization options non-configurable.



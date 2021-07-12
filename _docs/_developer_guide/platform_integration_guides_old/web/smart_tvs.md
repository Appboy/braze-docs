---
nav_title: Smart TV Integrations using the Web SDK
platform: Web
page_order: 20
description: "This article covers how to use the Braze Web SDK to integrate with Smart TVs (Samsung and LG)"

---

# Smart TV Integration

The Braze Web SDK lets you collect analytics and display rich In-App Messages and Content Card messages to Smart TV users, including [Samsung Tizen TVs][1] and [LG TVs (webOS)][2].

For a complete technical reference, please see our [Javascript Documentation][3] or check out our [sample apps][9] to see the Web SDK running on a TV.

## Install the Braze SDK

To get started, please follow our [Initial SDK Setup][4] guide for the Web SDK.

There are two changes required when integrating with Smart TVs:

1. When downloading or importing the Web SDK, be sure to use the "core" bundle which can be found here: [https://www.npmjs.com/package/@braze/web-sdk-core][6].

2. When initializing the Web SDK, you must set the `disablePushTokenMaintenance` and `manageServiceWorkerExternally` initialization options to `true`.

## Analytics

All of the same Web SDK methods for analytics can be used on Smart TVs.

For a complete guide to tracking custom events, custom attributes, and more, please read our [Analytics Documentation]({{site.baseurl}}/developer_guide/platform_integration_guides/web/analytics/tracking_sessions/).

## In-App Messages and Content Cards

Braze's Web SDK supports both [In-App Messages][7] and [Content Cards][8] on Smart TVs. Please note: You must use the ["Core" Web SDK][6] as rendering In-App Messages and Content Cards is not supported using our standard UI display, and should instead be customized by your app to fit into your TV App's experience.

To learn more on how your Smart TV App can receive and display In-App Messages, see our guide to [manually displaying In-App Messages][5].


[1]: https://developer.samsung.com/smarttv/develop/specifications/tv-model-groups.html
[2]: http://webostv.developer.lge.com/discover/discover-webos-tv/
[3]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html
[4]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/initial_sdk_setup/
[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/in-app_messaging/in-app_message_delivery/#manual-in-app-message-display
[6]: https://www.npmjs.com/package/@braze/web-sdk-core
[7]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/in-app_messaging/overview/
[8]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/content_cards/overview/
[9]: https://github.com/Appboy/smart-tv-sample-apps

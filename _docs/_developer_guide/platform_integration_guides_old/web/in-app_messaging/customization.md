---
nav_title: Customization
platform: Web
page_order: 3

page_type: reference
description: "This article covers customization of in-app messaging via the Braze SDK."
channel: in-app messages

---

# Customization {#in-app-message-customization}

All of Braze’s in-app message types are highly customizable across messages, images, [Font Awesome][15]  icons, click actions, analytics, editable styling, custom display options, and custom delivery options. Multiple options can be configured on a per in-app message basis from [within the dashboard]({{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/). Braze additionally provides multiple levels of advanced customization to satisfy a variety of use cases and needs.

## Key Value Pair Extras

In-app message objects may carry key-value pairs as their `extras` property. These are specified on the dashboard under "Additional Message Settings" when creating an in-app message campaign. These can be used to send data down along with an in-app message for further handling by your site. For example:

```javascript
appboy.subscribeToInAppMessage(function(inAppMessage) {
  if (inAppMessage instanceof appboy.InAppMessage) {
    var extras = inAppMessage.extras;
    for (var key in extras) {
      if (data.hasOwnProperty(key)) {
         console.log("key: " + key + ", value: " + extras[key]);
      }
    }
  }

  appboy.display.showInAppMessage(inAppMessage);
});
```

### In-App Message Default z-index

By default, In-App Messages are displayed using `z-index: 1050`. This is configurable using the `inAppMessageZIndex ` [initialization option][41] in the scenario that your website styles elements with higher values than that.

**Note**: This option was introduced in Web SDK v3.3.0. Older SDKs must be upgraded in order to use this option.

```javascript
import braze from "@braze/web-sdk";
braze.initialize("YOUR-API-KEY", {
    baseUrl: "YOUR-API-ENDPOINT",
    inAppMessageZIndex: 9001
});
```

### Custom Styling

Braze UI elements come with a default look and feel that create a neutral in-app message experience and aims for consistency with other Braze mobile platforms. Braze's default styles are defined in CSS within the Braze SDK. By overriding selected styles in your application, it is possible to customize our standard in-app message types with your own background images, font families, styles, sizes, animations, and more. For instance, the following is an example override that will cause an in-app message's headers to appear italicized:

```css
  body .ab-in-app-message .ab-message-header {
    font-style: italic;
  }
```

See the [JSDocs][2] for more information.

## Open Message Link in New Tab

To set your in-app message links to open in a new tab, set the `openInAppMessagesInNewTab` option to `true` to force all links from in-app message clicks open in a new tab or window.

```javascript
appboy.initialize('api-key', { openInAppMessagesInNewTab: true} );
```

[2]: https://js.appboycdn.com/web-sdk/latest/doc/ab.InAppMessage.html
[15]: https://fontawesome.com/?from=io
[41]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#initializationoptions

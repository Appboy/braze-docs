---
nav_title: Cookies and Storage
platform: Web
page_order: 15

page_type: reference
description: "This reference article describes the different cookies used by the Braze Web SDK."

---

# Cookies and Storage

This article describes the different cookies used by the Braze Web SDK.

Before reading on, please note that the Braze Web SDK will not store any data in the browser (cookies or otherwise) _until your website [initializes][5] the SDK_.

Additionally, these values are subject to change and should not be accessed directly through your integration. Instead, please see our [Javascript documentation][1] for our public API interfaces.

## Cookies {#cookies}

This section provides information on how cookies in the Braze Web SDK can be set and managed. 
The Braze Web SDK is built to provide you with maximum flexibility, legal compliance, and messaging relevance.

When Braze creates cookies, they are stored with a 1 year expiration that automatically renews on new sessions.

### Disabling Cookies {#disable-cookies}

To disable all cookies, use the [`noCookies`][6] option when initializing the Web SDK. 
Disabling cookies will prevent you from associating anonymous users who navigate across sub-domains, and will result in a new user on each subdomain.

```javascript
import braze from "@braze/web-sdk";
braze.initialize("API-KEY", {
    baseUrl: "BASE-URL",
    noCookies: true
});
```

To stop Braze tracking in general, or to clear _all_ stored browser data, please see the [`stopWebTracking`][3] and [`wipeData`][4] SDK methods, respectively. These two methods can be useful should a user revoke consent or you wish to stop all Braze functionality after the SDK has already been initialized.

### List of Cookies

|Cookie|Description|Size|
|---|----|---|---|
|`ab.storage.userId.[your-api-key]`|Used to determine whether the currently-logged-in user has changed, and to associate events with the current user.|Based on the size of the value passed to `changeUser`|
|`ab.storage.sessionId.[your-api-key]`|Out-of-the-box randomly generated string used to determine whether the user is starting a new or existing session, in order to sync messages and calculate session analytics.|~200 bytes|
|`ab.storage.deviceId.[your-api-key]`|Out-of-the-box randomly generated string used to identify anonymous users, and to differentiate users' devices and enables device-based messaging.|~200 bytes|
|`ab.optOut`|Used to store a user's opt-out preference when `stopWebTracking` is called|~40 bytes|
|`ab._gd`|Temporarily created (and then deleted) to determine the root-level cookie domain which allows the SDK to work properly across sub-domains.|n/a|
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

## Device Properties

By default, Braze will collect the following device-level properties to allow device, language, and timezone based message personalization:

* BROWSER
* BROWSER_VERSION
* LANGUAGE
* OS
* RESOLUTION
* TIME_ZONE
* USER_AGENT

You can disable or specify the properties you wish to collect by setting the `devicePropertyAllowlist` initialization option to a list of [`DeviceProperties`][2]. Note that without some properties, not all features will function properly. For instance, without the time zone, local timezone delivery will not function.

```javascript
import braze from "@braze/web-sdk";
braze.initialize("API-KEY", {
    baseUrl: "BASE-URL",
    devicePropertyAllowlist: [ braze.DeviceProperties.LANGUAGE ] // list of `DeviceProperties` you want to collect
});
```


[1]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html
[2]: https://js.appboycdn.com/web-sdk/latest/doc/classes/appboy.deviceproperties.html
[3]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#stopwebtracking
[4]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#wipedata
[5]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#initialize
[6]: https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#initializationoptions

---
nav_title: Initial SDK Setup
platform: Web
page_order: 0

page_type: reference
description: "This article covers initial SDK setup for the Braze Web SDK."

---

# Initial SDK Setup

The Braze Web SDK lets you collect analytics and display rich In-App Messages, Push, and Content Card messages to your web users.

For a complete technical reference, please see our [JavaScript Documentation][9].

## Step 1:  Install the Braze Library

There are three easy ways to integrate the Web SDK, to include analytics and messaging components on your site. Be sure to view our [Push Integration Guide][16] if you plan to use Web Push features. 

If your website uses a `Content-Security-Policy`, then please follow our [CSP Header Guide][19] in addition to the integration steps below.

### Option 1: NPM or Yarn {#install-npm}

If your site uses NPM or Yarn package managers, you can add the [Braze NPM package](https://www.npmjs.com/package/@braze/web-sdk) as a dependency.

Typescript definitions are now included as of v3.0.0 🎉. For notes on upgrading from 2.x to 3.x, please see our [Changelog][17].

```bash
npm install --save @braze/web-sdk
# or, using yarn:
# yarn add @braze/web-sdk
```

Once installed, you can `import` or `require` the library in the typical fashion:

```javascript
import appboy from "@braze/web-sdk";
// or, using `require`
const appboy = require("@braze/web-sdk");
```

### Option 2: Google Tag Manager {#install-gtm}

The Braze Web SDK can be quickly installed from the Google Tag Manager Template Library. Two tags are supported:

1. Initialization Tag - loads the Web SDK onto your website, and optionally sets the External User ID

2. Actions Tag - used to trigger custom events, purchases, change user IDs, or stop/resume SDK tracking

For more information, please see the [Google Tag Manager Integration Guide][18].

### Option 3: Braze CDN {#install-cdn}

Add the Braze Web SDK directly to your HTML by referencing our CDN-hosted script to load the library asynchronously.

<style>
.gist-it-gist {
    max-width:700px;
}
</style>

<script src="https://gist-it.appspot.com/https://github.com/Appboy/appboy-web-sdk/blob/master/snippets/loading-snippet.js?footer=minimal"></script>

## Step 2: Initialize Braze

Once the Braze Web SDK is added to your website, initialize the library with the `API Key` and [SDK Endpoint URL]({{site.baseurl}}/user_guide/administrative/access_braze/sdk_endpoints) found in **Manage Settings** > **Settings** within your Braze Dashboard.

**Note**: If you've configured your Braze initialization options in a Tag Manager, you can skip this step.

For a complete list of options for `appboy.initialize()` please see our [JavaScript Documentation](https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.initialize).


```javascript
// initialize the SDK
appboy.initialize('YOUR-API-KEY-HERE', {
    baseUrl: "YOUR-SDK-ENDPOINT-HERE"
});

// optionally show all In-App Messages without custom handling
appboy.display.automaticallyShowNewInAppMessages();

// optionally set the current user's External ID
if (isLoggedIn){
    appboy.changeUser(userIdentifier);
}

// start (or continue) a session
appboy.openSession();
```

For all other JavaScript methods, please see our complete [JavaScript Reference Documentation][9].

{% alert note %}
Anonymous users on mobile or web devices may be counted towards your [MAU]({{site.baseurl}}/user_guide/data_and_analytics/your_reports/understanding_your_app_usage_data/#monthly-active-users). As a result, you may want to conditionally load or initialize the SDK to exclude these users from your MAU count.
{% endalert %}

## Step 3: (Optional) Web Push

To use Web Push Notifications, additional setup is required. 

Please see our [Push Notifications][16] section for instructions.

## Troubleshooting {#error-logging}

To assist in troubleshooting, you can enable verbose logging in the SDK. This is useful for development but is visible to all users, so you should remove this option or provide an alternate logger with `appboy.setLogger()` in your production environment.

```javascript
appboy.initialize("YOUR-API-KEY-HERE", {
    baseUrl: "",
    enableLogging: true
});

// or, after initialization:

appboy.toggleAppboyLogging()
```

## Upgrading the SDK

When you reference the Braze Web SDK from our content delivery network, for example, `https://js.appboycdn.com/web-sdk/a.a/appboy.min.js` (as recommended by our default integration instructions), your users will receive minor updates (bug fixes and backward compatible features, versions `a.a.a` through `a.a.z` in the above examples) automatically when they refresh your site. 

When we release major changes however, we require you to upgrade the Braze Web SDK manually to ensure that nothing in your integration will be impacted by any breaking changes. Additionally, if you download our SDK and host it yourself, you won't receive any version updates automatically and should upgrade manually to receive the latest features and bug fixes.

You can keep up-to-date with our latest release [following our release feed](https://github.com/Appboy/appboy-web-sdk/tags.atom) with the RSS Reader or service of your choice, and see [our changelog](https://github.com/Appboy/appboy-web-sdk/blob/master/CHANGELOG.md) for a full accounting of our Web SDK release history. To upgrade the Braze Web SDK:

* Update the Braze library version by changing the version number of `https://js.appboycdn.com/web-sdk/[OLD VERSION NUMBER]/appboy.min.js`, or in your package manager's dependencies.
* If you have web push integrated, update the service worker file on your site - by default, this is located at `/service-worker.js` at the root directory of your site, but the location may be customized in some integrations. Please note that you must be able to access the root directory to host a service worker file. 

These two files must be updated in coordination with each other to ensure proper functionality.

## Alternative Integration Methods

### AMD Module Loader
If you are using Google Tag Manager alongside an AMD module loader such as RequireJS to load Braze's SDK you will need to use the RequireJS-compatible integration snippet in your `<head>` tag.

For further instruction on this please see the appropriate section of our [Braze Web SDK Github Repository][2].

### Tealium iQ

Tealium iQ offers a basic turnkey Braze integration. To configure the integration, just search for Braze in the Tealium Tag Management interface, and provide the Web SDK API key from your dashboard.

For more details, or in-depth Tealium configuration support, check out our [integration documentation]({{site.baseurl}}/partners/data_and_infrastructure_agility/customer_data_platform/tealium/#about-tealium) or reach out to your Tealium Account Manager.

### Other Tag Managers

Braze may also be compatible with other tag management solutions by following our integration instructions within a custom HTML tag. Please reach out to a Braze representative if you need help evaluating these solutions.

[2]: https://github.com/Appboy/appboy-web-sdk#getting-started "Braze Web SDK Github Repository"
[9]: https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html "JSDocs"
[16]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/push_notifications/integration/
[17]: https://github.com/Appboy/appboy-web-sdk/blob/master/CHANGELOG.md#300
[18]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/google_tag_manager/
[19]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/content_security_policy/
<!-- wesley wanted an empty line at the end -->

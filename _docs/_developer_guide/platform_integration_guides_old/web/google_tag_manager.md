---
nav_title: Google Tag Manager
platform: Web
page_order: 20
description: "This article covers how to use Google Tag Manager to deploy Braze to your website."

---

# Google Tag Manager

This article provides a step-by-step guide on how to add the Braze Web SDK to your website using Google Tag Manager.

[Google Tag Manager][2] lets you remotely add, remove, and edit tags on your website, without requiring a production code release or engineering resources.

## Braze Tag Templates

There are two Google Tag Manager templates built by Braze: the [Initialization Tag](#initialization-tag) and [Actions Tag](#actions-tag).

Both tags can be added to your workspace from [Google's Community Gallery][15], or by searching for Braze when adding a new tag from the Community Templates.

![Community Gallery Search][3]

### Initialization Tag Template {#initialization-tag}

Use the Initialization Tag to add the Braze Web SDK to your website.

#### Step 1: Choose the "Initialization Tag" from the Community Gallery

Search for "Braze" in the Community Template Gallery, and choose the Braze Initialization Tag as shown below:

![Initialization Tag Template][4]

#### Step 2. Configure Settings

Enter your Braze SDK API Key and SDK Endpoint, which can be found in your Dashboard's [Settings][6] page.

#### Step 3. Choose Initialization Options

Choose from the optional set of additional initialization options as described in the [Initial Setup][7] guide.

#### Step 4: Verify and QA

Once you've deployed this tag, there are two ways you can verify a proper integration:

First, using Google Tag Manager's Debug Mode, you should see the Braze Initialization Tag has been triggered on your configured pages or events.

Second, you should see network requests made to Braze, and the global `window.appboy` library should now be defined on your webpage.

### Actions Tag Template {#actions-tag}

The Braze Actions Tag template lets you trigger custom events, track purchases, change user IDs, and stop/resume tracking for privacy requirements.

![Actions Tag Template][5]

#### Changing User External ID {#external-id}

The __Change User__ Tag Type calls the [`changeUser` method](https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#changeuser).

Use this tag whenever a user logs in, or is otherwise identified with their unique "external_id" identifier.

Be sure to enter the current user's unique ID in the "External User ID" field, typically populated using a datalayer variable sent by your website.

![Change User Tag][8]

#### Log Custom Events {#custom-events}

The __Custom Event__ Tag Type calls the [`logCustomEvent` method](https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#logcustomevent).

Use this tag to send Custom events to Braze, optionally including custom event properties

Enter the __Event Name__, either using a variable or by typing in an event name.

Use the __Add Row__ button to add event properties.

![Custom Event Tag][9]

#### Track Purchase {#purchases}

The __Purchase__ Tag Type calls the [`logPurchase` method](https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#logpurchase).

Use this tag to track purchase to Braze, optionally including Purchase properties.

The _Product ID_ and _Price_ fields are required.

Use the __Add Row__ button to add Purchase properties.

![Purchase Tag][10]

#### Stop and Resume Tracking {#stop-tracking}

Sometimes, you might be required to disable or re-enable Braze tracking on your website, for example, after a user indicates they've opted out of web tracking for privacy reasons.

Use the __Disable Tracking__ or __Resume Tracking__ Tag Type to disable web tracking or re-enable web tracking, respectively.

#### Custom User Attributes {#custom-attributes}

Custom user attributes are not available due to a limitation in Google Tag Manager's scripting language. To log custom attributes, create a Custom HTML tag with the following content:

```html
<script>
window.appboy.getUser().setCustomUserAttribute("attribute name", "attribute value");
</script>
```

## Troubleshooting Steps {#troubleshooting}

### Enable Tag Debugging {#debugging}

Each Braze Tag template has an optional "GTM Tag Debugging" checkbox which can be used to log debug messages to your webpage's Javascript console.

![Tag Debugging Option][12]

### Enter Debug Mode

Another way to help debug your Google Tag Manager integration is using Google's [Preview Mode][14] feature.

This will help identify what values are being sent from your webpage's datalayer to each triggered Braze tag, and will also explain which tags were or were not triggered.

![Preview Mode][13]


[2]: https://support.google.com/tagmanager/answer/6103696
[3]: {% image_buster /assets/img/web-gtm/gtm-community-gallery-search.png %}
[4]: {% image_buster /assets/img/web-gtm/gtm-initialization-tag.png %}
[5]: {% image_buster /assets/img/web-gtm/gtm-actions-tag.png %}
[6]: {{ site.baseurl }}/user_guide/administrative/app_settings/manage_app_group/app_group_management/#app-group-management
[7]: {{ site.baseurl }}/developer_guide/platform_integration_guides/web/initial_sdk_setup/#step-2-initialize-braze
[8]: {% image_buster /assets/img/web-gtm/gtm-change-user.png %}
[9]: {% image_buster /assets/img/web-gtm/gtm-custom-event.png %}
[10]: {% image_buster /assets/img/web-gtm/gtm-purchase.png %}
[12]: {% image_buster /assets/img/web-gtm/gtm-tag-debugging.png %}
[13]: {% image_buster /assets/img/web-gtm/gtm-debug-mode.png %}
[14]: https://support.google.com/tagmanager/answer/6107056
[15]: https://tagmanager.google.com/gallery/#/?filter=braze

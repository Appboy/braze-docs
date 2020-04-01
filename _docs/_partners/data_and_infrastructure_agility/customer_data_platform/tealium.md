---
nav_title: Tealium
page_order: 2
alias: /partners/tealium/

description: "This article outlines the partnership between Braze and Tealium, a universal data hub that enables you to connect mobile, web, and alternative data to other third-party sources."
page_type: partner

---

# About Tealium

> Tealium is a universal data hub that enables you to connect mobile, web, and alternative data to other third-party sources.

Tealium’s connection to Braze enables a data flow of custom events, user attributes, and purchases that empower you to act on your data in real-time.

# Pre-Requisites

| Requirement| Origin| Access| Description|
| ---| ---| ---|
| Tealium Account & Account Information | Tealium | https://my.tealiumiq.com/ | You must have an active Tealium Account to utilize their services with Braze. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

# Data In Integration
Integrate Braze into your web app using the Tealium Tag Manager. In order to set up this integration correctly, there are a number of steps you need to take in order to configure the core integration. It’s then important to be able to understand how you start sending data to Braze by setting up custom events/custom attributes.

1. Set up Braze as a “Tag” in your Tealium dashboard.
2. From the Tag Configuration dialogue box, enter your API Key and your appropriate Endpoint.
  * Find your API Key and Endpoint in your Braze account or confirm it with your onboarding manager or support representative.
  * This API key is for the app identifier, rather than the REST API key
3. From the Tealium Code Centre, copy the code snippet for the environment you are currently building (dev, QA, prod) and paste it at the top of body tag within your HTML.
5. Verify that the Braze SDK is being loaded by Tealium by opening the browser dev tools and in the console typing “appboy”.
  * The list of available functions should then be printed to the console.

# Customizing Your Integration
To customize your integration (like logging custom events or custom attributes), click on the data layer tab in your Tealium dashboard and begin adding the custom data you require.

* In order for Tealium to recognize these data points, copy and paste the updated code snippet from the code center again with the ``utag_data`` containing all your data.
* To customize when the Braze SDK is loaded, click on the __Load Rules__ tab of your Tealium dashboard, then choose on which pages the SDK should initialize.

{% alert warning %}
If the data layer is not configured correctly, or you incorrectly enter your [Endpoint]({{ site.baseurl }}/developer_guide/rest_api/basics/#endpoints), your integration may fail or not return correct results.
{% endalert %}

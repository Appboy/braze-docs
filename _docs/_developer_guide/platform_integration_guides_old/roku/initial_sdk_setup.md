---
nav_title: Initial SDK Setup
platform: Roku
page_order: 0

page_type: reference
description: "This page describes initial setup steps for the Braze Roku SDK."

---

# Initial Roku SDK Integration

Installing the Braze Roku SDK will provide you with basic analytics and segmentation functionality.

## Step 1: Add Files

Braze SDK files can be found in the `sdk_files` directory in the [Braze Roku SDK repo](https://github.com/Appboy/appboy-roku-sdk).

1. Add `BrazeSDK.brs` to your app in the `source` directory.
2. Add `BrazeTask.brs` and `BrazeTask.xml` to your app in the `components` directory.

## Step 2: Add References

Add a reference to `BrazeSDK.brs` in your main scene using the following `script` element:

```
<script type="text/brightscript" uri="pkg:/source/BrazeSDK.brs"/>
```

## Step 3: Configure

Within `main.brs`, set the Braze configuration on the global node:

```
globalNode = screen.getGlobalNode()
config = {}
config_fields = BrazeConstants().BRAZE_CONFIG_FIELDS
config[config_fields.API_KEY] = "YOUR_API_KEY_HERE"
' example endpoint: "https://sdk.iad-01.braze.com/"
config[config_fields.ENDPOINT] = "YOUR_ENDPOINT_HERE"
config[config_fields.HEARTBEAT_FREQ_IN_SECONDS] = 5
globalNode.addFields({brazeConfig: config})
```

## Step 4: Initialize Braze

Initialize the Braze instance:

```
m.BrazeTask = createObject("roSGNode", "BrazeTask")
m.Braze = getBrazeInstance(m.BrazeTask)
```

## Basic SDK Integration Complete

Braze should now be collecting data from your application with the Braze Roku SDK. Please see the following sections on how to [log attributes]({{site.baseurl}}/developer_guide/platform_integration_guides/roku/analytics/setting_custom_attributes/), [events]({{site.baseurl}}/developer_guide/platform_integration_guides/roku/analytics/logging_custom_events/), and [purchases]({{site.baseurl}}/developer_guide/platform_integration_guides/roku/analytics/logging_purchases/) to our SDK.

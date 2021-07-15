---
nav_title: Tinyclues
permalink: /partners/tinyclues/
description: "This article outlines the partnership between Braze and Tinyclues, which offers an audience-building feature to help you send to more targeting campaigns, find new product opportunities, and elevate revenue using an incredibly user-friendly UI."
page_type: partner
hidden: true

---

# Tinyclues

> [Tinyclues](https://www.tinyclues.com/) is an audience-building feature that offers the capability to increase the number of campaigns and revenue without harming customer experience and analytics to track the performance of CRM campaigns both online and offline.

Together, the Braze and Tinyclues integration offers users a path to better CRM planning and strategy, allowing users to send more targeting campaigns, find new product opportunities, and elevate revenue using an incredibly user-friendly UI.

## Overview

To integrate Braze and Tinyclues, you must export an existing Tinyclues campaign and create a user cohort segment in the Braze platform to filter future campaigns.

1. Configure the Tinyclues platform with the appropriate settings.
2. Each time a campaign is executed on Tinyclues Action, the associated audience is automatically pushed to Braze.
3. From the Braze platform, create a segment of the Tinyclues campaign users.
3. Create a Braze campaign and use the dedicated audience filtering option to target your Tinyclues cohort segment.

## Integration Requirements
- An active Tinyclues account.
- An active Braze account with the ability to use the Tinyclues integration.
- The API key corresponding to Tincylues' integration must be communicated to your Tinyclues Data Operation representative to set up the integration.

## Implementation Process

### Step 1: Get the Braze Data Import Key
In your Braze account, navigate to __Technology Partners__ and select __Tinyclues__. Here, you will find your REST Endpoint and generate your Braze data import key. 

### Step 2: Share API Key with TinyClues Data Operations

You will need to provide the data import key and your REST endpoint to the Tinyclues Data Operations Team for the integration to be complete. Tinyclues will then establish the connection and reach out to you once the setup is complete. 

### Step 3: Export a Campaign from the Tinyclues Platform

Each time you want to create a cohort of Tinyclues users to use in Braze, you'll have to first export it from the Tinyclues platform.

In Tinyclues, select the campaign(s) you want to export and click __Export Campaigns__.

![Tinyclues][1]

Upon export, the audience will be automatically uploaded to your Braze account.

### Step 4: Use the Tinyclues Custom Audience in Braze

Next on the Braze platform, navigate to __Segments__, name your Tinyclues cohort segment, and select __Tinyclues Cohorts__ as your filter. From here, you can choose which Tinyclues cohort you wish to include. Once created, you will be able to select your Tinyclues cohort segment as an audience filter when creating a campaign or Canvas.

Having trouble locating your cohort? Check out our [troubleshooting](#troubleshooting) section for guidance. 

![Tinyclues][3]{: style="max-width:70%;"}
![Tinyclues][4]{: style="max-width:70%;"}

### Step 5: Retrieve your Audience in Braze

On the Braze platform, create a campaign or Canvas, and on the target audience step, select the Tinyclues cohort segment you just built.

## Troubleshooting
Are you having trouble finding the right cohort within the list? Verify the name in Tinyclues UI by clicking on the campaign and checking the "Export File Name".

![Tinyclues][2]{: style="max-width:30%;"}

Still having trouble retrieving your audience, contact the Tinyclues team for additional support: support@tinyclues.com.

[1]: {% image_buster /assets/img/tinyclues/tinyclues_1.png %} 
[2]: {% image_buster /assets/img/tinyclues/tinyclues_2.png %} 
[3]: {% image_buster /assets/img/tinyclues/tiny_clues3.png %} 
[4]: {% image_buster /assets/img/tinyclues/tiny_clues4.png %} 
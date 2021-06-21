---
nav_title: Reporting
page_order: 20
description: "Braze provides you with a detailed report of each of your email campaigns. This article covers the different components of the report and where it can be found in the dashboard."

tool:
  - Dashboard
  - Reports

channel:
  - email
---

# Email Reporting

Braze provides you with a detailed report of each of your email campaigns. Navigate to the **Campaigns** tab on your dashboard and select your desired campaign to open the **Details** tab. On this page, you will be able to comprehensively view and analyze the success of your campaign in an organized format. Here, you will also be able to adjust the date filter of your report. Please note that updating this filter will result in your report only displaying events that happened during your selected date range.

<!---
Add screenshot from Wendy here
--->

## Conversion Events

For email messages that are a part of multichannel or multivariate campaigns, Braze provides three additional conversion metrics that attribute conversion actions to message interaction:

- **Received and Converted:** The number and percentage of unique email recipients who have, within the selected conversion window, received the email and then converted.
- **Opened and Converted:** The number and percentage of unique email recipients who have, within the selected conversion window, opened the email and then converted.
- **Clicked and Converted:** The number and percentage of unique email recipients who have, within the selected conversion window, clicked the email and then converted.

{% alert note %}
In multivariate campaigns, these metrics will be broken out separately for each variant, allowing you to compare attributed conversions for each version of your test.
{% endalert %}

## Heat Maps

Additionally, you can see how successful different links in a single email campaign are using heat maps. Under **Email Performance**, expand the **Total Clicks** dropdown and click **View Heat Map** to bring up a visual view of your email that shows the overall frequency and location of clicks within the lifespan of the campaign. Note that date ranges are not taken into consideration for email heat maps.

![email_analytics][63]

If you want to see what our metrics mean, check out our [Email Analytics Glossary]({{site.baseurl}}/user_guide/message_building_by_channel/email/analytics_glossary/).

[63]: {% image_buster /assets/img_archive/email_click_results_heatmap.gif %}

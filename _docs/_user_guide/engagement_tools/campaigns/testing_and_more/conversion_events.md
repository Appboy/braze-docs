---
nav_title: Conversion Events
platform: Campaigns
subplatform: Testing and More
page_order: 0

page_type: tutorial
description: "This how-to article goes over what conversion events are, how to use them and define your success metrics within Braze, and how to use these tools to see how engaged your users are"
tool: Campaigns
---
# Conversion Events

> This how-to article goes over what conversion events are, how to use them and define your success metrics within Braze, as well as how to use these tools to see how engaged your users are.
> <br>
> <br>
> By using conversion events, you can make sure you are collecting relevant, useful information that you can later use to gain insight into your campaign. 

In order to track engagement metrics and the necessary details regarding how messaging drives your KPIs, Braze allows you to set conversion events for each of your campaigns and Canvases.

A conversion event is a type of success metric that tracks whether a recipient of your messaging performs a high-value action within a set amount of time after receiving your engagement. With this, you can begin to attribute these valuable actions to the different points of engagement reaching the user. For example, if you're creating a personalized holiday campaign for active users, a conversion event of "Starts Session" within 2 or 3 days may be appropriate, as it will then allow you to gather a sense of the rate at which your engagement helped nudge users to come back upon receiving your message.

For more on conversions, check out our [Campaign Setup LAB course](http://lab.braze.com/campaign-setup-delivery-targeting-conversions)!

Along with "Make a Purchase,” events like "Start a Session,” "Upgrade App," or any of your Custom events can be selected as conversion events. Below are further details on the feature, as well as the steps needed to implement them.

## Primary Conversion Event
The Primary Conversion Event is the first event added during the campaign or Canvas creation, and it is the one that has the most bearing on your engagement and reporting. It is used to:

- Compute the winning message variation in multivariate campaigns or Canvases.
- Determine the window in which revenue is calculated for the campaign or Canvas.
- Adjust message distributions for campaigns and Canvases using Intelligent Selection.

## Step 1: Create a Campaign with Conversion Tracking
Navigate to the [Braze Campaigns page][1] in your company dashboard and click **Create Campaign**, then select the type of campaign you'd like to create.

After setting up your campaign's messages and—for non-API campaigns—schedule, you'll have the option to add up to four conversion events for tracking. We highly recommend using as many as you feel is necessary, as the addition of a second (or third) conversion event can significantly enrich your reporting. 

For example, if you had a campaign or Canvas targeting lapsing users, although a retention-centric conversion event of "Starts Session" within 3 days is valuable, perhaps you also want to add a secondary conversion event of performing another high-value Custom Event. This way, you can dive back into the dashboard and understand not only the extent to which your campaign or Canvas is ushering users back into your application but also how involved and active these sessions are.

## Step 2: Add Conversion Events

For each conversion event you wish to track, select the event and conversion deadline:

1. Select the general type of event you'd like to use.<br><br>

    ![Conversion Event Selection][2]

    <br><br>
    - __Opens App:__ A user is counted as having converted when they open any one of the apps that you specify (defaults to all apps in the app group).
    - __Makes Purchase:__ A user is counted as having converted when they purchase the product you specify (defaults to any product).
    - __Performs Custom Event:__ A user is counted as having converted when they perform one of your existing custom events (no default, you must specify the event).
    - __Upgrade App:__ A user is counted as having converted when they upgrade the app version on any one of the apps that you specify (defaults to all apps in the app group). Braze will perform a best-efforts numerical comparison to determine if the version change was, in fact, an upgrade.
    <br><br>For example, a user would convert if they upgrade from version 1.2.3 to 1.3.0 of the application, while Braze will not register a conversion if a user downgrades from 1.2.3 to 1.2.2. However, if the app's version names contain strings, such as "1.2.3-beta2", then Braze will not be able to determine if a version change was, in fact, an upgrade. In that situation, Braze will count it as a conversion when the user's most recent app version changes.
    <br><br>
 
2. Set a **Conversion Deadline**. You can allow up to a 30-day window during which a conversion will be counted if the user takes the specified action.  

Once you've selected your conversion events, continue the campaign creation process and begin sending your campaign.

## Step 3: View Results

Navigate to the **Details** page to view details for each conversion event associated with the campaign you just created. Regardless of your selected conversion events, you can also see the total revenue that can be attributed to this specific campaign—as well as specific variants—during the window of the primary conversion event.

{% alert note %}
If no conversion events were selected during campaign creation, the time period defaults to 3 days. 
{% endalert %}

Additionally, for multivariate messages, you can see the number of conversions and conversion percentages for your control group and each variant.

![View Results][3]

### Email Messages

For email messages that are a part of multichannel or multivariate campaigns, Braze provides three additional conversion metrics that attribute conversion actions to message interaction:

- **Received and Converted:** The number and percentage of unique email recipients who have, within the selected conversion window, received the email and then converted.
- **Opened and Converted:** The number and percentage of unique email recipients who have, within the selected conversion window, opened the email and then converted.
- **Clicked and Converted:** The number and percentage of unique email recipients who have, within the selected conversion window, clicked the email and then converted.

{% alert note %}
In multivariate campaigns, these metrics will be broken out separately for each variant, allowing you to compare attributed conversions for each version of your test.
{% endalert %}

## Conversion Tracking Rules

Conversion events allow you to attribute user action back to a point of engagement. That said, there are a few things to note regarding how Braze handles conversions when there are multiple in play. Please find these scenarios outlined below.

- A user can only convert once on each conversion event for a campaign or Canvas. For instance, assume a campaign has only one conversion event which is "makes any purchase." If a user who receives this campaign makes 2 separate purchases within the conversion deadline, only one conversion will be counted.
- If a user performs one conversion event within the conversion deadlines of two separate campaigns or Canvases that they received, the conversion will register on both.
- A user will count as converted if they performed the specific conversion event in the window even if they did not open/click the message.

[1]: https://dashboard-01.braze.com/engagement/campaigns/ "Campaigns Page"
[2]: {% image_buster /assets/img_archive/conversion_event_selection.png %}
[3]: {% image_buster /assets/img_archive/conversion_event_details.png %}

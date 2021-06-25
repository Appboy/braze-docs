---
nav_title: Creating an Email Campaign
platform: Message_Building_and_Personalization
subplatform: Email
page_order: 1
description: "This reference article covers how to create an email campaign using the Braze platform. Included are best practices on how to compose your messages, preview your content, and schedule your campaign."

tool:
  - Dashboard
  - Campaigns

channel:
  - email
---

# Creating an Email Campaign

Email messages are great for delivering content to the user on their terms. They are also excellent tools to re-engage users who may have even uninstalled your app! Customized and tailored email messages will enhance the user experience and help your user get the most value out of your app.

To see examples of email campaigns, check out our [Client Integration Gallery][9].

## Step 1: Create a New Campaign
Click __Create Campaign__ in the top right corner of the __Campaigns__ page.

## Step 2: Compose Your Email or Use a Template {#step-2-choose-your-template-and-compose-your-email}

Click on the appropriate tile to choose select wether to use our drag & drop editing or our HTML editor. Once selected, you may either select an existing [HTML][10] or [ drag & drop][10] email template, [upload one from a file][18] (HTML editor only), or create one from scratch and then draft your message.

![email1][3]

Once you choose a template for your email, you'll see an overview of your email, where you can quickly jump to the fullscreen editor, change your sending info, and view warnings about deliverability or law compliance.

![newemailoverview][14]

### Step 2a: Adding Email Headers

Email headers contain information about the email being sent.  These key-value pairs typically have information about the sender, recipient, authentication protocols, and contain email routing information.  Braze automatically adds the necessary header information required by the RFC for emails to be delivered to your inbox provider properly.  However, Braze does allow our customers flexibility to add additional email headers as needed for advanced use cases.  There are a few reserved fields that the Braze platform will overwrite during sending.  Avoid using the following keys:
'x-sg-id', 'x-sg-eid', 'received', 'dkim-signature', 'Content-Type', 'Content-Transfer-Encoding', 'To', 'From', 'Subject', 'Reply-To', 'CC', 'BCC', 'Received', 'DKIM-Signature', 'MIME-Version'

### Step 2b: Preview and Test Your Message

After you finish composing your perfect email, you need to test it before sending it out! Navigate to the test page by clicking the "Preview and Test" button at the bottom of the overview screen. Use **Preview as User** to make sure that your Connected Content and personalization calls are working as they should and to get a sense of how your message may view on desktop vs. mobile. Use **Test Sends** to ensure that your email displays properly on a variety of devices and email clients.

![newemailtest][15]

### Step 2c: Check for Email Errors

The new Email Editor will call out problems with your message before you send it. Here's a list of errors that are accounted for in our editor:

- From Display Name and Header not specified together.
- Invalid From and Reply-To Emails.
- Duplicate Header Keys.
- Liquid Syntax Problems.
- [Email bodies larger than 400kb; bodies are highly recommended to be smaller than 102kb.][16]
- Emails with blank Bodies or Subjects.
- Emails with no unsubscribe link.
- The email you are sending from is not whitelisted, thus sends will be highly limited to ensure deliverability.


## Step 3: Schedule Your Messaging Campaign

You can schedule your campaigns against three types of delivery:
- Scheduled (sending your campaign at a designated time),
- Action-Based (sending when your users perform specified actions), and
- API-Triggered (sending according to an API request).

{% tabs %}
{% tab Scheduled Delivery %}
Scheduled Delivery allows you to specify the time at which you want the message to send, either immediately or at a future time (you can also consider local time in your scheduling). You can also use Intelligent Timing to send the message when the user is most likely to engage. Braze makes this calculation based on a statistical analysis of the user's interactions with your app.

Messages can also be configured to recur daily, weekly (optionally on specific days), or monthly.

Unless you select the option to "Message Repeatedly", each user will only receive the contents of a campaign once, and only new users that meet the criteria will receive the campaign on subsequent deliveries.

![Schedule]({% image_buster /assets/img_archive/schedule_new.png %}){: height="80%"" width="80%"}

{% endtab %}
{% tab Action-Based Delivery %}

Action-Based Delivery allows you to specify the time a message will send after a user takes a particular action (selected from the __New Trigger Action__ dropdown.)

![Action]({% image_buster /assets/img_archive/action_delivery_new.png %}){: height="80%"" width="80%"}

{% endtab %}
{% tab API-Triggered Delivery %}
Check out our [API-Triggered section of the Developer Guide]({{site.baseurl}}/developer_guide/rest_api/messaging/#sending-messages-via-api-triggered-delivery) for more on API-Triggered Delivery.

![API]({% image_buster /assets/img_archive/api_delivered_new.png %}){: height="80%"" width="80%"}
{% endtab %}
{% endtabs %}

## Step 4: Choose Conversion Events

Braze allows you to track how often users perform specific actions (Conversion Events) after receiving a campaign. You can specify any of the following actions as a "Conversion Event":

- Opens App
- Makes Purchase
  - This can be a generic purchase or a specific item
- Performs specific custom event
- Opens Email
- Clicks Email

You have the option of allowing up to a 30-day window during which a conversion will be counted if the user takes the specified action. While Braze automatically tracks opens and clicks for your campaign, you may wish to set the conversion event to be when a user opens or clicks on an email address to take advantage of Braze's [Intelligent Selection feature][13].

## Step 5: Choose Your Target Segment

Next, you need to choose the target segment from the dropdown menu. You'll automatically be given a snapshot of what that segment population looks like right now, including how many users within that segment are reachable via email. Keep in mind that exact segment membership is always calculated just before the message is sent.

![Target Segment][5]

Optionally, you can also choose to limit delivery to a specified number of users within the segment or allow users to receive the same message twice upon a recurrence of the campaign.

## Step 6: Review and Deploy

The final page will give you a summary of the campaign you've just designed. Clicking "Start Campaign" will enable it for sending. Confirm all the relevant details and watch the data roll in!

![email2][6]

## Results Data

Braze will show you the number of emails sent, opened, clicked through, sent to spam, and bounced for each campaign you deploy. Additionally, Braze's email analytics tool allows for the ability to see how your users' clicks are divided across the links in an email campaign. Clicking on each variation expands/collapses information on what percentage of users clicked on which link in an email campaign.

![email_analytics][17]

When looking at clicks for your variant, the clicks will reflect unique clicks. When you expand each variation, the clicks for each link within the variation will reflect total clicks (not unique). Braze also gives you the ability to visualize where users are clicking within a given email campaign.

{% alert note %}
Braze will de-duplicate sends by email address. However, opens are not de-duplicated to prevent the illusion that an email opened by a user with multiple [User Profiles]({{site.baseurl}}/user_guide/data_and_analytics/user_data_collection/user_profile_lifecycle/) under a single email address would only be counted towards a single User Profile.
{% endalert %}

[1]: {% image_buster /assets/img_archive/newcampaign_new.png %}
[3]: {% image_buster /assets/img_archive/choose_email_creation.png %}
[5]: {% image_buster /assets/img_archive/targetsegment_email_new.png %}
[6]: {% image_buster /assets/img_archive/confirm_email.png %}
[9]: {{site.baseurl}}/help/best_practices/client_integration_gallery/#client-integration-email
[10]: {{site.baseurl}}/user_guide/message_building_by_channel/email/creating_an_email_template/#creating-an-email-template
[13]: {{site.baseurl}}/user_guide/engagement_tools/campaigns/testing_and_more/multivariate_testing/#intelligent-selection
[14]: {% image_buster /assets/img/email.png %}
[15]: {% image_buster /assets/img_archive/newEmailTest.png %}
[16]: {{site.baseurl}}/help/best_practices/email/email_styling_tips/#email-size
[17]: {% image_buster /assets/img_archive/email_click_results_heatmap.gif %}
[18]: {{site.baseurl}}/user_guide/message_building_by_channel/email/creating_an_email_template/#upload-an-html-email-template

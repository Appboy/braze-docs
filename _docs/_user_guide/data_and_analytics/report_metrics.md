---
page_order: 0
nav_title: Report Metrics Glossary
layout: glossary_page
glossary_top_header: "Report Metrics Glossary"
glossary_top_text: "These are terms you'll find in your reports in your Braze account. Search for the metrics you need or filter by channel. <br>  <br> This glossary does not necessarily include metrics you might see in Currents or other downloaded reports outside of your Braze account."

page_type: glossary
description: "This glossary defines terms you'll find in your reports in your Braze account."
tool: Reports

glossary_tag_name: Channels
glossary_filter_text: "Select Channels below to narrow the glossary:"

# channel to icon/fa or image mapping
glossary_tags:
  - name: Content Cards
  - name: Email
  - name: In-App Message
  - name: News Feed
  - name: Web Push
  - name: iOS Push
  - name: Android Push
  - name: Webhook
  - name: SMS

glossaries:
  - name: Variation
    description: Variation of a campaign, differing as defined by the creator.
    calculation: Count
    tags:
      - All
  - name: Audience
    description: Percentage of users who received a particular message. This number is received from Braze.
    tags:
      - All
  - name: Unique Recipients
    description: Unique Daily Recipients. The number of users who received a particular message in a day. This number is received from Braze.
    calculation: Count
    tags:
      - All
  - name: Total Impressions
    description: The number of times the in-app message or card has been viewed (if a user is shown a message twice, they will be counted twice). This number is a sum of the number of impression events that Braze receives from the SDKs.
    calculation: Count
    tags:
      - In-App Message
      - News Feed
      - Content Cards
  - name: Unique Impressions
    description: The total number of users who received and viewed a given in-app message or card. The count should not increment the second time a user views an in-app message or card. This number is received from Braze.
    calculation: Count
    tags:
      - In-App Message
      - News Feed
      - Content Cards
  - name: Sends
    description: The total number of messages sent in a campaign. This number is received from Braze.
    calculation: Count
    tags:
      - All
  - name: Sends to Carrier
    description: This stat is the sum of Confirmed Deliveries, Rejections, and Sends where delivery or rejection was not confirmed by the carrier. There are instances where carriers do not provide delivery or rejected confirmation, as some carriers do not provide this confirmation or were unable to do so at the time of send.
    calculation: Count
    tags:
      - SMS
  - name: Deliveries
    description: The total number of message requests that are accepted by the receiving email server.
    calculation: Count
    tags:
      - All Push
      - Email
      - Web Push
      - iOS Push
      - Android Push
  - name: Confirmed Deliveries
    description: The carrier has confirmed that the SMS was delivered to the target phone number. As a Braze customer, deliveries are charged toward your SMS allotment.
    calculation: Count
    tags:
      - SMS
  - name: Bounces
    description: The total number of messages that were unsuccessful. This could occur because there is not a valid push token, the email addresses were incorrect or deactivated, or the user unsubscribed after the campaign was launched. <br> <i> An email bounce for customers using Sendgrid consists of hard bounces, spam, and emails sent to invalid addresses. </i>
    calculation: (Bounces) / (Sends)
    tags:
      - All Push
      - Email
      - Web Push
      - iOS Push
  - name: Rejections
    description: The SMS has been rejected by the carrier. This can happen for a number of reasons including carrier content filtering, availability of the destination device, the phone number is no longer in service, etc. As a Braze customer, rejections are charged toward your SMS allotment.
    calculation: Count
    tags:
      - SMS
  - name: Delivery Failures
    description: The SMS could not be sent due to queues overflow (sending SMS at a rate higher than your long or short codes can handle).
    calculation: (Sends) - (Sends to Carrier)
    tags:
      - SMS
  - name: Spam
    description: The total number of emails delivered that were marked as "spam."
    calculation: (Marked as Spam) / (Sends)
    tags:
      - Email
  - name: Errors
    description: The number of errors returned by webhook events (incremented during the sending process).
    tags:
      - Webhook
  - name: Total Opens
    description: The total number of messages that were opened.
    calculation: (Opens) / (Deliveries) (for Email); (Direct Opens) / (Deliveries) (for Web Push); (Unique Opens) / (Deliveries) (for iOS Push, Android, Kindle)
    tags:
      - Email
      - iOS Push
      - Android Push
      - Web Push
      - All Push
  - name: Unique Opens
    description: The total number of delivered emails that have been opened by a single user at least once. This is tracked over a 7 day period for Email.
    calculation: (Unique Opens) / (Deliveries)
    tags:
      - Email
  - name: Direct Opens
    description: The total number (and percentage) of push notifications that were directly opened from that push.
    calculation: (Direct Opens) / (Deliveries)
    tags:
      - iOS Push
      - Android Push
  - name: Influenced Opens
    description: The total number (and percentage) of users who opened the app after the push notification was sent, without directly opening the push.
    calculation: (Influenced Opens) / (Deliveries)
    tags:
      - iOS Push
      - Android Push
  - name: Total Clicks
    description: The total number (and percentage) of users who clicked within the delivered email or card. Please note that clicks on an unsubscribe link will not be counted towards total clicks.
    calculation: (Total Clicks) / (Deliveries) (for Email) or (Total Clicks) / (Total Impressions) (for Content Cards)
    tags:
      - Email
      - News Feed
      - Content Cards
  - name: Unique Clicks
    description: Distinct number of recipients who have clicked within a message at least once. This is tracked over a 7 day period for Email. Note that clicks on Braze-provided unsubscribe links are counted as unique clicks.
    calculation: (Unique Clicks) / (Deliveries) (for Email) or (Unique Clicks) / (Unique Impressions) (for Content Cards)
    tags:
      - Email
      - News Feed
      - Content Cards
  - name: Body Clicks
    description: Occurs when someone clicks on a slide-up, modal, or full-screen in-app message that has no buttons.
    calculation: (Body Clicks) / (Impressions)
    tags:
      - In-App Message
  - name: Button 1 Clicks
    description: Total number of clicks on Button 1 of the message.
    calculation: (Button 1 Clicks) / (Impressions)
    tags:
      - In-App Message
  - name: Button 2 Clicks
    description: Total number of clicks on Button 2 of the message.
    calculation: (Button 2 Clicks) / (Impressions)
    tags:
      - In-App Message
  - name: Body Click
    description: Push Story Notifications record a body click when the notification is clicked. It will not be recorded when a message is expanded, or for action button clicks.
    calculation: (Body Clicks) / (Impressions)
    tags:
      - iOS Push
      - Android Push
  - name: Unsubscribes
    description: The number of recipients whose subscription state changed to unsubscribed as a result of clicking the Braze provided unsubscribe URL.
    calculation: (Unsubscribes) / (Deliveries)
    tags:
      - Email
  - name: Revenue
    description: The total revenue in dollars from campaign recipients within the set primary conversion window.
    tags:
      - All
  - name: Primary Conversions (A) or Primary Conversion Event
    description: The number of times a defined event occurred after interacting with or viewing a received message from a Braze campaign. This defined event is determined by the marketer when building the campaign. For Email, Push and Webhooks, we start tracking conversions after the initial send. For Content Cards and In-App Messages, this count begins when they view a Content Card or Message for the first time.
    tags:
      - All
  - name: Conversion Rate
    description: The percentage of times a defined event occurred compared to all recipients of a sent message. This defined event is determined when you build the campaign.
    calculation: (Primary Conversions) / (Unique Recipients)
    tags:
      - All
  - name: Conversions (B, C, D)
    description: The percentage of times a defined event occurred compared to all recipients of a sent message. This defined event is determined when you build the campaign.
    tags:
      - All
  - name: Confidence
    description: The percentage of confidence that a certain variant of a message is outperforming the control group.
    tags:
      - All
  - name: Pending Retry
    description: The number of requests that were temporarily rejected, by the receiving server, but still attempted for re-delivery by the ESP. The ESP will retry delivery until a timeout period is reached (typically after 72 hours).
    tags:
      - Email
  - name: Total Dismissals
    description: The number of times Content Cards from a campaign have been dismissed. If a user dismisses a message twice, they will be only counted once.
    calculation: Count
    tags:
      - Content Cards
  - name: Unique Dismissals
    description: The number of users who have dismissed Content Cards from a campaign. A user dismissing a Content Card from a campaign multiple times represents one unique dismissal.
    calculation: (Unique Dismissals) / (Unique Impressions)
    tags:
      - Content Cards
  - name: AMP Clicks
    description: The total number of users who clicked into the AMP version of your AMP HTML Email.
    tags:
      - Email
  - name: Received
    description: Content Cards - Received when users view the card in the app.<br>Push - Received when messages are sent from the Braze server to the push provider.<br>Email - Received when messages are sent from the Braze server to the email service provider.<br>SMS/MMS - "Delivered" once the SMS provider receives confirmation from the upstream carrier and destination device.<br>In-App Message - Received at the time of display based on the trigger action defined. 
    tags:
      - Email
      - Content Cards
      - In-App Message
      - Web Push
      - iOS Push
      - Android Push
      - SMS
  - name: Opened and Converted
    description: The number and percentage of unique email recipients who have, within the selected conversion window, opened the email and then converted.
    tags:
      - Email
  - name: Clicked and Converted
    description: The number and percentage of unique email recipients who have, within the selected conversion window, clicked the email and then converted.
    tags: 
      - Email
  - name: Received and Converted
    description: The number and percentage of unique email recipients who have, within the selected conversion window, received the email and then converted.
    tags:
      - Email
---

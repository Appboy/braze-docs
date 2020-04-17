---
nav_title: Data Points
page_order: 4

description: "This page outlines what Data Points are at Braze and how you can be aware of their usage."
---

# Data Points

At Braze, data means action: each piece of data that arrives in Braze updates segment membership, can trigger & cancel messaging, is immediately available for messaging personalization, and more. As such, data points are Braze’s way of defining a billing and pricing structure, based on pieces of information logged against user profiles. See [Consumption Count](#consumption-count) below to see what data counts toward your data point allocation.

Our Customer Success team can help recommend data best practices to fit your specific needs. You can find a more detailed breakdown of this definition in your Braze contract.

{% alert important %}
This pricing structure only applies to customers using in-app or in-browser messages, push, Content Cards, webhooks, and email services.
{% endalert %}

## Management & Usage

You can view your Data Points Usage on the __Subscriptions and Usage__ section in any of the tabs. Just go to your name in the top-right corner, click the drop down, and select __Subscriptions and Usage__.

{% alert tip %}
__Only update your deltas (changing data)!__

To prevent using up your allocated data points, we recommend setting up a program that will prevent sending the same, unchanging data from your app to Braze over and over.
{% endalert %}


## Consumption Count

In sum, data points are accumulated when a user's profile data is updated or when they perform specific actions. Essentially, data points are counts of each of your users' `session starts`, `session ends`, `events`, and `purchases`.

You can find a breakdown of how Braze accumulates data points below, but there are nuances beyond what you see here, which may affect the number of remaining points you expect to see. If you ever have any questions about your billing, reach out to your Braze account manager.

{% alert tip %}
Connected Content does not consume data points - using Connected Content is a great way to reference data from other platforms without needing to bulk upload to Braze and use up your points!
{% endalert %}

{% tabs %}
{% tab General %}

### General

|Data Type | Data Point | Does it count towards consumption? |
|---|---|---|
|Profile Data | First Name | Yes |
|Profile Data | Last Name | Yes |
|Profile Data | User ID | Yes |
|Profile Data | Email Address | Yes |
|Profile Data | Gender | Yes |
|Profile Data | Age Group | Yes |
|Profile Data | Country | Yes |
|Profile Data | City | Yes |
|Profile Data | Language | Yes |
|Profile Data | Most Recent Device Locale | Yes |
|Profile Data | Time Zone | Yes |
|Profile Data | Date of Birth (DOB) | Yes |
|Profile Data | Bio | Yes |
|Profile Data | Phone Number  | Yes |
|Profile Data | Avatar Image URL | Yes |
|App Usage Data |Session Start | Yes |
|App Usage Data |Session End | Yes |
|Custom Attributes | All Custom Attributes | Yes |
|Recent Devices | Number of Devices | No |
|Recent Devices | Most Recent Watch | No |
|Recent Devices | App Version | No |
|Recent Devices | Device | No |
|Recent Devices | Device OS | No |
|Custom Events | All Custom Events | Yes |
|Purchases | All Purchases | Yes |
|Amplitude Cohort Assignment | All Assignments | Yes |
|Mixpanel Cohort Assignment | All Assignments | Yes |

  {% endtab %}
{% tab Location %}

### Location

|Data Type | Data Point | Does it count towards consumption? |
|---|---|---|
|Most Recent Location | All Most Recent Locations | Yes |

  {% endtab %}
{% tab Engagement %}

### Engagement

|Data Type | Data Point | Does it count towards consumption? |
|---|---|---|
| Contact Settings | Email Subscribed | No |
| Contact Settings |  Push Subscribed | No |
| Contact Settings |  Apps Registered for Push | No |
|Campaigns Received | Email Address | No |
|Segments | Gender | No |
|Communication Stats | Age Group | No |
|Communication Stats | Country | No |
|Communication Stats | City | No |
|Communication Stats | Language | No |
|Communication Stats | Most Recent Device Locale | No |
|Communication Stats | Most Recent Device Locale | No |
|News Feed Cards Clicked | News Feed Cards Clicked | No |
|Install Attribution | Install Source | No |
|Install Attribution | Campaign | No |
|Install Attribution | Ad Group | No |
|Install Attribution | Ad | No |
|Miscellaneous | Random Bucket Number | No |
|Canvas Messages Received | Canvas Messages Received | No |

 {% endtab %}
{% tab Social %}

### Social

|Data Type | Data Point | Does it count towards consumption? |
|---|---|---|
|Twitter | Username | Yes |
|Twitter | Followers | No |
|Twitter | Following | No |
|Twitter | Number of Tweets | No |
|Facebook | Likes | No |

 {% endtab %}
{% endtabs %}

---
nav_title: "Simple Survey"
page_order: 1.5
page_type: reference
hidden: false
description: "This reference article covers how to collect user attributes, insights, and preferences to power your campaign strategy using the new in-app message surveys."
channel:
  - in-app messages
tool:
  - Media
  - Templates
---
<br>

# Simple Survey In-App Message

Use the new **Simple Survey** In-App Message template to collect user attributes, insights, and preferences that power your campaign strategy.

For example, ask users how they'd like to use your app, learn more about their personal preferences, or even ask about their satisfaction with a particular feature.

![examples]({% image_buster /assets/img/iam/iam-survey.png %})

## SDK Requirements {#supported-sdk-versions}

This in-app message will only be delivered to devices that support [Flex CSS](https://caniuse.com/?search=flex), and must have at least the [SDK versions]({{ site.baseurl }}/user_guide/engagement_tools/campaigns/ideas_and_strategies/new_features/#filtering-by-most-recent-app-versions) below:

{% sdk_min_versions web:2.5.0 android:8.0.0 ios:3.23.0 %}

## Creating a Survey {#create}

When creating an [in-app message][1], select **Simple Survey** for your **Message Type**.

![Simple Survey Message Type]({% image_buster /assets/img/iam/survey-message-type.png %}){: style="max-width:80%"}

This survey template is supported for both mobile apps and web browsers. Remember to check that your SDKs are on the [minimum versions](#supported-sdk-versions) required for this feature.

### Step 1: Add Your Survey Question

To get started building your survey, add your question to the survey **Header** field. If desired, you can add an optional **Body** message that will appear below your survey question.

![Simple Survey Question]({% image_buster /assets/img/iam/iam-survey2.png %})

{% alert tip %} These fields can include both Liquid and emojis, so get fancy! {% endalert %}

### Step 2: Choose Between Single or Multiple-choice {#single-multiple-choice}

Use the Single vs. Multiple Choice option to control whether a user can select only one choice or multiple choices. You can add up to 12 choices in a survey.

![Single Multiple Choice]({% image_buster /assets/img/iam/single-multiple-choice.png %}){: style="max-width:70%"}

{% alert tip %} Your **Helper text** will automatically update when you switch between **Single-choice selection** and **Multiple-choice selection** to let users know how many choices they can select. {% endalert %}

### Step 3: Collect Custom Attributes {#custom-attributes}

Select **Log attributes upon submission** to collect attributes based on the user's submission. You can use this option to create new segments and retargeting campaigns. For example, in a satisfaction survey, you could send a follow-up email to all users who were not happy.

![Custom Attributes]({% image_buster /assets/img/iam/collect-attributes.png %}){: style="max-width:70%"}

To add a custom attribute to each choice, select an existing attribute name from the dropdown, and the value to set when this choice is submitted. You can create a new custom attribute in your [Settings Page][5].

For example, in a notification preferences survey, you might make each choice a boolean (true/false) attribute to allow users to select which topics they're interested in. If a user checks the "Promotions" choice, that will update their [User Profile][3] with the custom attribute `Promotions Topic` set to `true`. If they leave the choice unchecked, that same attribute will remain unchanged.

![Choice Custom Attributes]({% image_buster /assets/img/iam/iam-survey3.png %}){: style="max-width:70%"}

You can then create a segment for users with `Promotions Topic = true` to make sure that only users interested in your promotions will receive the relevant campaigns.

{% alert important %} When custom attribute collection is enabled, choices that share the same custom attribute name will be combined into an array.{% endalert %}

### Step 4: Choose Submission Behavior

Once a user submits their response, you can optionally show a confirmation page, or simply close the message.

A confirmation page is a great place to thank users for their time or provide additional information. You can customize the Call To Action on this page to guide users to another page of your app or website.

Edit your button text and on-click behavior in the **Submit Button** section at the bottom of the **Survey** tab:

![Confirmation Option]({% image_buster /assets/img/iam/confirmation-option.png %}){: style="max-width:70%"}

If you elect to add a confirmation page, switch to the **Confirmation Page** tab to customize your message:

![Confirmation Page]({% image_buster /assets/img/iam/confirmation-page.png %}){: style="max-width:70%"}

If you want to guide users to another page of your app or website, change the button’s **On-click behavior**.

### Step 5: Stylize Your Message (Optional) {#styling}

You can customize the font color and accent color of the message using the **Color Theme** picker.

![Color Theme Picker]({% image_buster /assets/img/iam/color-theme-picker.png %}){: style="max-width:80%"}

## Analyze Results {#analytics}

Once your campaign has launched, you can analyze results in real-time to see the breakdown of each selected choice. If you've enabled [Custom Attribute collection](#custom-attributes), you'll also be able to create new segments or follow-up campaigns for users who have submitted the survey.

{% alert note %}
Deleted survey choices will still appear in analytics, but will not be shown as a choice to new users.
{% endalert %}

For definitions of survey metrics, refer to the [Report Metrics Glossary][11] and filter by "In-App Message".

![Analytics]({% image_buster /assets/img/iam/iam-survey-analytics.png %}){: style="max-width:90%"}

Check out [Reporting & Analytics][4] for a breakdown of your campaign metrics.

### Currents {#currents}

Selected choices will automatically flow through to Currents, under the [__In-App Message Click Events__][6] `button_id` field. Each choice will be sent with its universally unique identifier (UUID).

## Use Cases

### User Satisfaction

**Goal:** Measure customer satisfaction and send win-back campaigns to users who left low scores.

Here we're using single-choice selection, with choices ranging from "Very Dissatisfied" to "Very Satisfied". Each choice has the custom attribute `customer_satisfaction` set to a number from 1 to 5, with 1 being the least satisfied and 5 being the most satisfied. 

After you've launched your survey, you can then target your win-back campaigns to users who reported being "Very Dissatisfied" or "Dissatisfied", which are users with `customer_satisfaction` set to 1 or 2.

![User Satisfaction][7]

### Identify Customer Goals

**Goal:** Identify top reasons why users visit your app.

Here we're using single-choice selection, with each choice being a common reason a user might be visiting your app. Each choice has the custom attribute `product_goal` set to the use case topic. 

For example, if the user selects "Upgrading my account", that will set `product_goal = upgrade` on the user's profile.

![Identify Customer Goals][8]

### Improve Conversion Rates

**Goal:** Understand why customers aren’t upgrading or purchasing.

Here we're using single-choice selection, with each choice being a common reason why a user might not upgrade to a Premium account. Each choice has the custom attribute `upgrade_reason` set to the user's selection. 

For example, if the user selects "Too Expensive", that will set `upgrade_reason = expensive` on the user's profile. You can then target these users for promotional campaigns, like discounts or free trials.

![Improve Conversion Rates][9]

### Favorite Features

**Goal:** Understand which features customers enjoy using.

Here we're using multiple-choice selection, with each choice being an app feature. Each choice has the custom attribute `favorite_features` set to the user's selection. Because this use case involves multiple choice, once the user has completed the survey, their profile will be updated with the `favorite_features` attribute set to an array of all selected options.

![Favorite Features][10]

[1]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/
[2]: {{site.baseurl}}/user_guide/data_and_analytics/custom_data/custom_attributes/#custom-attribute-data-types
[3]: {{site.baseurl}}/user_guide/data_and_analytics/user_data_collection/user_profile_lifecycle/
[4]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/reporting/
[5]: {{site.baseurl}}/user_guide/administrative/app_settings/manage_app_group/custom_event_and_attribute_management/
[6]: {{site.baseurl}}/user_guide/data_and_analytics/braze_currents/event_glossary/message_engagement_events/#api_fzzdoylmrtwe

[7]: {% image_buster /assets/img_archive/simple_survey_use_case_1.png %}
[8]: {% image_buster /assets/img_archive/simple_survey_use_case_2.png %}
[9]: {% image_buster /assets/img_archive/simple_survey_use_case_3.png %}
[10]: {% image_buster /assets/img_archive/simple_survey_use_case_4.png %}

[11]: {{site.baseurl}}/user_guide/data_and_analytics/report_metrics/

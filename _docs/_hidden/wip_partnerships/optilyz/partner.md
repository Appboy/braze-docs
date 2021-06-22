---
nav_title: optilyz
page_order: 1

description: "This article outlines the partnership between Braze and optilyz, which enables you to run more customer-centric, sustainable, and profitable direct mail campaigns."
alias: /partners/optilyz/

page_type: partner
hidden: true
---

# optilyz Direct Mail Automation

> [optilyz][1] enables you to run more customer-centric, sustainable, and profitable direct mail campaigns.<br><br>The tool is used by hundreds of companies across Europe and empowers you to integrate letters, postcards, and self-mailers into your cross-channel marketing as well as automate and better personalize campaigns.

You can access optilyz’ services through Braze’s webhook feature and send direct mail to your customers.

## Requirements

You need both a Braze and an optilyz account.

| Requirement | Origin | Access | Description |
|---|---|---|---|
| optilyz API Key<br>“OPTILYZ_API_KEY” | optilyz | Your optilyz customer success manager will provide you with your optilyz API Key. | This API Key will enable you to connect your Braze and optilyz accounts. |
| optilyz automation ID<br>“OPTILYZ_AUTOMATION_ID” | optilyz | The automation ID is shown in a box on the page header. | When logged into optilyz you can navigate to the automation you want to send data into.<br>The automation must be activated first. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Integration

To integrate with optilyz you use the optilyz API to send recipient data through Braze’s webhook feature.<br>The optilyz API documentation can be found here: [https://www.optilyz.com/doc/api/][2]

### Step 1: Create a Webhook Template in Braze

To do this, follow this manual on [Creating a Webhook Template][3] in Braze

![optilyz_webHook_details][4]

### Step 2: Fill Out Your Template

- Webhook URL:
  
https://www.optilyz.com/api/v2/automations/OPTILYZ_AUTOMATION_ID/recipient

- Request Body (Raw Text):

```json
{% raw %}
{
    "address": {
        "title": "{{custom_attribute.${salutation}}}",
        "firstName": "{{${first_name}}}",
        "lastName": "{{${last_name}}}",
        "street": "{{custom_attribute.${street}}}",
        "houseNumber": "{{custom_attribute.${houseNumber}}}",
        "address2": "{{custom_attribute.${address2}}}",
        "zipCode": "{{custom_attribute.${zipCode}}}",
        "city": "{{custom_attribute.${city}}}",
        "country": "{{custom_attribute.${country}}}"
    },
    "variation": {{custom_attribute.${designVariation}}}
}
{% endraw %}
```

In the request body you can use any Liquid Personalization Tags and build a custom request template according to optilyz’ API documentation.

“variation” is optional and can be used to define which design inside the automation should be used. If variation is omitted, optilyz will assign one of the defined variations randomly.

![optilyz_compose][5]

### Step 3: Define Request Headers and HTTP Method

- HTTP Method: POST
- Request Headers:
  - Content-Type: application/json
  - Authorization: Basic ``` {{ 'OPTILYZ_API_KEY:' | base64_encode }} ``` -> please notice the “:” after the key!

![optilyz_settings][6]

### Step 4: Preview Your Request and Save Your Template

You should be able to preview your request in the left-hand panel or navigate to the Test tab, where you can select a random user, an existing user, or customize your own to test your webhook.

![optilyz_testing_1][7]
![optilyz_testing_2][8]

{% alert important %}
Remember to save your template before leaving the page!
{% endalert %}

## Using This Integration

Create a campaign, select “Webhook” and choose your template from the “Saved Webhook Template” list.<br>Create your [Webhook][9]!

## Use Cases

Running direct mail like a digital channel means moving away from mass mailings and leveraging the channel as part of your (digital) customer journeys.

Reach out to your optilyz customer success manager if you’re interested in understanding how companies in your industry leverage letters, postcards and more to enhance their customer experience. Our best practice and benchmark database helps us assist you in setting up high-performing automations. These may be geared at converting one-time buyers into loyal fans, exciting your most valuable customers, and preventing churn.

The benefits of a modern approach to direct mail are:
- Increased conversion rates via increased relevance, additional use cases, easier A/B testing & cross-channel effects
- Reduced effort via automation & an end-to-end solution
- Reduced cost via frame contracts & cost transparency

[1]: https://optilyz.com
[2]: https://www.optilyz.com/doc/api/
[3]: https://www.braze.com/docs/user_guide/message_building_by_channel/webhooks/webhook_template/
[4]: {% image_buster /assets/img/optilyz/optilyz_webHook_details.png %}
[5]: {% image_buster /assets/img/optilyz/optilyz_compose.png %}
[6]: {% image_buster /assets/img/optilyz/optilyz_settings.png %}
[7]: {% image_buster /assets/img/optilyz/optilyz_testing_1.png %}
[8]: {% image_buster /assets/img/optilyz/optilyz_testing_2.png %}
[9]: {{site.baseurl}}/user_guide/message_building_by_channel/webhooks/creating_a_webhook/


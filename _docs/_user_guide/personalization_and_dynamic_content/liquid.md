---
nav_title: Liquid
page_order: 0
layout: featured
guide_top_header: "Personalization Using Liquid Tags"
guide_top_text: "Braze can automatically substitute values from a given user into your messages. Put your expression inside of two sets of curly brackets to notify Braze that you'll be using an interpolated value. Inside of these brackets, any user values that you want to substitute must be surrounded by an additional set of brackets with a dollar sign in front of them."
description: "Braze can automatically substitute values from a given user into your messages. Put your expression inside of two sets of curly brackets to notify Braze that you'll be using an interpolated value."

guide_featured_title: "Section Articles"
guide_featured_list:
- name: Using Liquid
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/using_liquid/
  fa_icon: fas fa-flask
- name: Supported Personalization Tags
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/supported_personalization_tags/
  fa_icon: fas fa-tag
- name: Operators
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/operators/
  fa_icon: fas fa-code
- name: Filters
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/filters/
  fa_icon: fas fa-filter
- name: Advanced Filters
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/advanced_filters/
  fa_icon: fas fa-cog
- name: Setting Default Values
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/setting_default_values/
  fa_icon: fas fa-table
- name: Conditional Messaging Logic
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/conditional_logic/
  fa_icon: fas fa-columns
- name: Aborting Messages
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/aborting_messages/
  fa_icon: fas fa-undo
- name: Liquid Use Cases
  link: /docs/user_guide/personalization_and_dynamic_content/liquid/liquid_use_cases/
  fa_icon: fas fa-list-ul
---

## About Liquid

Campaign messages support templated messaging using the Liquid templating language. Detailed documentation of Liquid syntax and usage is available [here][1].

_For more on Dynamic Personalization with Liquid, check out our [detailed, guided LAB course](https://lab.braze.com/dynamic-personalization-with-liquid)!_

Liquid templating language supports the use of objects, tags and filters.

- [Objects]({{site.baseurl}}/user_guide/personalization_and_dynamic_content/liquid/overview/) allow you to insert personalized attributes into your messages.

- [Tags]({{site.baseurl}}/user_guide/personalization_and_dynamic_content/liquid/supported_personalization_tags/) allow you to execute programming logic in your messages. For example, you can use tags to include intelligent logic, such as "if" statements, in your campaigns.

- [Filters]({{site.baseurl}}/user_guide/personalization_and_dynamic_content/liquid/filters/) allow you to reformat personalized attributes and dynamic content. For example, you could convert a timestamp, such as *2016-09-07 08:43:50 UTC* into a date such as *September 7, 2016*.


## Terms to Know

These terms are reinterpreted from [Shopify's documentation](https://shopify.github.io/liquid/basics/introduction/) based on our level of support.

{% alert warning %}

Braze does not currently support 100% of Shopify's Liquid, only certain portions which we have attempted to outline in our documentation. We highly recommend testing all messages using Liquid before sending to reduce the risk of errors or using unsupported Liquid.

{% endalert %}

{% raw %}

| Term | Definition | Example |  
|---|---|---|
| Liquid | An open-source, customer-facing template language created by Shopify and written in Ruby; used to load/pull dynamic content. | `{{${first_name}}}` will insert a user's first name into a message. |
| Object | A denotation of a variable and location of intended variable name that tells Liquid where to show content in the message. | `{{${first_name}}}` will insert a user's first name into a message. |
| Conditional Logic Tag | Tags create logic and control the flow for templates. In Braze's case, Conditional Logic Tags are Liquid used to consider intelligent or programming logic to create exceptions and variations in messages based on certain, predefined criteria. | ```{% if ${language} == 'en' %}``` will trigger your message in a designated way in the event that a user has designated "English" as their language. |
| Filters | Used to change, narrow, or reformat the output of the Liquid Object. It is often used to create mathematical operations. |  ```{{"Big Sale" | upcase}}``` will cause the words "Big Sale" to appear as "BIG SALE" in the message. |
| Operators | Used in messages to create dependencies or criteria that can affect which message your user receives. | Is a user meets the defined criteria in a messaged tagged with `{% custom_attribute.${Total_Revenue} > 0%}`, they will receive the message. If not, they will receive another designated message (or not), depending on what you set. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

{% endraw %}

<br>

[1]: {{site.baseurl}}/user_guide/personalization_and_dynamic_content/liquid/supported_personalization_tags/

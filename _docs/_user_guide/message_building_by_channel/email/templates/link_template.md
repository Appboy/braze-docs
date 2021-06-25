---
nav_title: Link Templates
platform: Message_Building_and_Personalization
subplatform: Email
page_order: 4
description: "Link Templates allow users to append parameters or pre-append URLs to all links in an Email message. This article covers how to create different types of Link Templates."

tool:
  - Dashboard
  - Templates

channel:
  - email

---
# Link Templates

> Link Templates allow users to append parameters or pre-append URLs to all links in an Email message.

This can be ideally used for the following use cases:

1. Appending Google Analytics query parameters to all links in a given Email message easily.
2. Pre-appending a URL to all links in a given Email message

## Creating a Link Template

On the Braze dashboard, Link Templates are found in the Templates & Media section. Dashboard users can create an unlimited number of Link Templates to support their various needs. Note that when using Link Templates and [Liquid][4], Liquid code must only be added within the body tag to ensure consistent rendering. 

There are two types of Link Templates a user can create.

{% tabs %}
{% tab Link Template Before URL %}

__Create a Link Template that inserts before a URL__

Clients who want to pre-append a string or URL before a link in their Email message can create a new Link Template and choose the Template position to be "Before URL".  This will allow you to enter a string that will always get appended before a URL.  A preview section is provided to give you an example of the insertion process.

![pre-append]({% image_buster /assets/img_archive/link_template_preappend.png %})

{% endtab %}
{% tab Link Template After URL %}

__Create a Link Template that appends after a URL__

Clients who want to append query parameters after a URL in their Email message can create a new Link Template and choose the Template position to be "After URL".  This will allow you to enter query parameters (value=something) to the end of each URL.  You can have multiple parameters appended to the end of a URL.

![postappend]({% image_buster /assets/img_archive/link_template_postappend.png %})

{% endtab %}
{% endtabs %}

### Selecting a Template

Once your templates are setup, you can select which template you would like to use from the Email Editor Composer.
Proceed to open the Email composer, click Content Library and select the Link Templating tab.  You will see all the links present in your Email and can add the template from there.

![messagecomposer][1]

### Managing Link Templates
You can also [duplicate]({{site.baseurl}}/user_guide/engagement_tools/templates_and_media/duplicate/) link templates! Learn more about creating and managing templates and creative content in [Templates & Media]({{site.baseurl}}/user_guide/engagement_tools/templates_and_media/).

{% alert important %}
Archiving templates is not currently available for [Link Templates]({{site.baseurl}}/user_guide/message_building_by_channel/email/link_templates/#link-templates).
{% endalert %}


[1]:{% image_buster /assets/img_archive/link_template_messagecomposer2.png %}
[2]:{% image_buster /assets/img_archive/link_template_postappend.png %}
[3]:{% image_buster /assets/img_archive/link_template_preappend.png %}
[4]: {{site.baseurl}}/user_guide/personalization_and_dynamic_content/liquid/

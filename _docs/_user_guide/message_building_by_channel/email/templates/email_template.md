---
nav_title: Create an Email Template
platform: Message_Building_and_Personalization
subplatform: Email
page_order: 2
description: "Email messages are great for delivering content to the user on their terms. This reference article covers how to create, customize and manage email templates."

tool:
  - Dashboard
  - Templates

channel:
  - email

alias: "/dnd/email_template/"

---
# How to Create an Email Template

> Email messages are great for delivering content to the user on their terms. They are also wonderful tools to re-engage users who may have even uninstalled your app. The Braze dashboard has an email template editor that allows you to create custom-tailored, eye-catching emails and save them for later use in campaigns. You can also upload [your own HTML Email Template]({{site.baseurl}}/user_guide/message_building_by_channel/email/html_email_template/).

## Step 1: Navigate to the Email Template Editor

Click on the __Templates & Media__ tab under __Engagement__ in the navigation bar; this will open to the __Email Template Gallery__.

## Step 2: Create or Choose a Template

Now, you can create a new template or edit an existing template (plain or [mobile responsive][8]) using either the drag & drop editing experience or standard HTML experience. If you'd like to create a new template, then you can choose from Braze's predesigned templates, or you can choose to create a new layout.

![New Template][2]

_Note: Any existing custom HTML templates will need to be re-created using the Drag & Drop Editor._

## Step 3: Customize Your Template

You can write your message within the rich-text editor or optionally flip over to our HTML editor or Drag & Drop Editor to customize your content. Once selected, you will be guided to the editor experience you chose. An 'HTML Editor' or 'Drag & Drop Editor' badge will appear, indicating that you are about to use this editing experience for template creation.

![dnd_badge_icon]({% image_buster /assets/img/dnd_badge_icon.png %})

{% tabs %}
{% tab HTML Editor %}

Braze will add a footer with an unsubscribe link at the bottom of your HTML emails by default. You can customize this footer in the **Email Settings** tab of the **Manage Settings** page. For more information, please read our [custom footer documentation]({{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#custom-footer).

If you want to see how your email looks before sending it out to your users, you can send a test email to an address of your choosing through the Settings tab in the top right corner.

![Enter Email Template Editing]({%image_buster/assets/img/email_templates/template3.jpg %})
<br>
<br>
![E-Mail Template Editor Guide]({%image_buster/assets/img/email_templates/template4.jpg %})

Our editor supports **HTML Autocomplete** triggered by the 'Tab' key.  This feature should be used on naked HTML tags. For example, the use of tab on `head` will result in:
{% raw %}
```
<head>
  <meta http-equiv="content-type" content="text/html; charset=utf-8" />

  <title>`substitute(Filename('', 'Page Title'), '^.', '\u&', '')`</title>

</head>
```
{% endraw %}
{% endtab %}
{% tab Drag & Drop Editor %}

The drag & drop editing experience is broken out into 3 sections: __Sending Settings__, __Content__, and __Preview & Test__.

{% subtabs %}
{% subtab Send Settings %}
#### __Sending Settings__
The Sending Settings section allows you to configure your from and reply-to address as well as set the subject line or pre-header. 

_Note: Advanced functionality will appear in the campaign or Canvas step composer. In advanced functionality, you can modify your inline CSS setting, set a BCC email address, and enter in a header or extra key-value pairs (if configured)._

{% endsubtab %}
{% subtab Content %}
#### __Content__
The Content section contains the editor. There are three key components within this section.

- __Content__: This section includes a series of tiles that represent the different kinds of content you can use in your message. More will become available in the future. To use them, just drag one inside an existing row segment; it will auto-adjust to the column width. Every block has its own settings, such as granular control on padding. The right-side panel automatically switches to a property panel for the selected content element.<br><br> For more information see [Editor Block Properties]({{site.baseurl}}/dnd/editor_blocks/)<br><br>
- __Rows__: Rows are structural units that define the horizontal composition of a section of the message by using columns. Using more than one column allows you to put different content elements side by side. You can add all the structural elements you need to your message, regardless of the template you selected when you started.<br><br>
- __Settings__: General settings for the message. They are inherited by Rows and Content sections. For example, the font family set in the message settings is then used everywhere in your message, except where you use a custom setting.

This is very useful to build a coherent message very quickly.
{% endsubtab %}
{% subtab Preview and Test %}
#### __Preview & Test__
The Preview & Test section allows you to preview your email based on different users.

- __Random User__: Braze will randomly select a user from the database and preview the email based on their attributes/event information.
Note: This user may or may not be part of your segmentation criteria. Segmentation is selected afterward, so Braze is unaware of your target audience at this point.<br><br>
- __Select User__: You can select a specific user based on their email address or `external_id`. The email will preview based on that user's attributes and event information<br><br>
- __Custom user__: You can customize a user. Braze will offer inputs for all available attributes and events. You can enter any information you would like to see in the preview email.
{% endsubtab %}
{% endsubtabs %}

_Note: Inbox Vision is currently unavailable during this testing phase and will be made available in the future_

{% alert tip %}
To read more about the different components of the drag & drop editing experiance, visit our drag & drop documentation articles [here]({{site.baseurl}}/user_guide/message_building_by_channel/email/drag_and_drop/). 
{% endalert %}

{% endtab %}
{% endtabs %}

## Step 4: Check for Email Errors {#step-3a-check-for-email-errors}
Email errors are presented on the ‘compose’ tab of the message workflow. Errors prevent you from progressing forward, while “Warnings” indicate reminders to help you follow best practices. Depending on your business, you might choose to ignore them.

![dnd_compose_error][1]{: style="float:right;max-width:40%;margin-left:15px;"}

Here's a list of errors that are accounted for in our editor:

- Incorrect Liquid Syntax
- [Email bodies larger than 400kb; bodies are highly recommended to be less than 102kb][7]
- Templates without an unsubscribe link

## Step 5: Save Your Template {#step-4-save-your-template}

Be sure to save your template by clicking the "Save Template" button in the bottom right corner of the editor. You're now ready to use this template in any campaign or Canvas step you choose.

>  Edits made to an existing template will not be reflected in campaigns that were created using previous versions of that template.

## Using Your Templates in API Campaigns {#api_for_email_templates}
To use your email for an API campaign, you need an `email_template_id`, which can be found at the bottom of any Email Template created in Braze.

![Save Template][6]

## Managing Email Templates

You can [duplicate]({{site.baseurl}}/user_guide/engagement_tools/templates_and_media/duplicate/) and [archive]({{site.baseurl}}/user_guide/engagement_tools/templates_and_media/archive/) email templates! Learn more about creating and managing templates and creative content in [Templates & Media]({{site.baseurl}}/user_guide/engagement_tools/templates_and_media/).


[2]: {% image_buster /assets/img/email_templates/template2.png %}
[1]: {% image_buster /assets/img/dnd_compose_error.png %}
[3]: {% image_buster /assets/img/email_templates/template3.jpg %}
[4]: {% image_buster /assets/img/email_templates/template4.jpg %}
[cf]: {{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#custom-footer
[6]: {% image_buster /assets/img/email_templates/template5.jpg %}
[7]: {{site.baseurl}}/help/best_practices/email/email_styling_tips/#email-size
[8]: {{site.baseurl}}/help/release_notes/2017/august/#mobile-responsive-email-templates

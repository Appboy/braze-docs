---
nav_title: Overview
alias: "/dnd/email_template/"
alias: "/dnd/overview/"
channel: email
page_order: 1
description: "This reference article covers various creative details of Drag & Drop editor blocks."
---

# Drag & Drop Editor Overview

{% include video.html id="4dTrkxe8DLo" align="right" %}

> With Braze Email, you can create completely custom and personalized email messages in either Campaigns or Canvas using a drag & drop editing experience.

## Create a Drag & Drop Email

### Step 1: Select Editing Experience
Navigate to the email wizard and select your editing experience. Two editing options will be shown:
- Select the 'Drag & Drop Editor' to select templates created using the Drag & Drop Editor.
- Select the HTML Code Editor to use the existing editors and see your existing email templates.<br><br>![dnd_editor_workflow][6]{: style="max-width:80%;"}

### Step 2: Build out Email Structure
1. __Assemble Rows__ - Drag & drop different row configurations to design the structure of your email. New configurations must be dragged to the beginning or end of an existing section.
2. __Add Content__ - Add desired content types to the various row components.<br><br>![Drag & Drop Email GIF][1]

## Editing Experience

The drag & drop editing experience is broken out into 3 sections: __Sending Settings__, __Content__, and __Preview & Test__.

{% tabs %}
{% tab Send Settings %}
__Sending Settings__

The Sending Settings section allows you to configure your from and reply-to address and set the subject line or pre-header. 

_Note: Advanced functionality will appear in the campaign or Canvas step composer. In advanced functionality, you can modify your inline CSS setting, set a BCC email address, and enter in a header or extra key-value pairs (if configured)._

{% endtab %}
{% tab Content %}
__Content__

The Content section contains the editor. There are three key components within this section.

- __Content__: This section includes a series of tiles that represent the different kinds of content you can use in your message. More will become available in the future. To use them, just drag one inside an existing row segment; it will auto-adjust to the column width. Every block has its own settings, such as granular control on padding. The right-side panel automatically switches to a property panel for the selected content element.<br><br> For more information see [Editor Block Properties]({{site.baseurl}}/dnd/editor_blocks/)<br><br>
- __Rows__: Rows are structural units that define the horizontal composition of a section of the message by using columns. Using more than one column allows you to put different content elements side by side. You can add all the structural elements you need to your message, regardless of the template you selected when you started.<br><br>
- __Settings__: General settings for the message. They are inherited by Rows and Content sections. For example, the font family set in the message settings is then used everywhere in your message, except where you use a custom setting.

This is very useful to build a coherent message very quickly.

{% endtab %}
{% tab Preview and Test %}
__Preview & Test__

The Preview & Test section allows you to preview your email based on different users.

- __Random User__: Braze will randomly select a user from the database and preview the email based on their attributes/event information.
Note: This user may or may not be part of your segmentation criteria. Segmentation is selected afterward, so Braze is unaware of your target audience at this point.<br><br>
- __Select User__: You can select a specific user based on their email address or `external_id`. The email will preview based on that user's attributes and event information<br><br>
- __Custom user__: You can customize a user. Braze will offer inputs for all available attributes and events. You can enter any information you would like to see in the preview email.
{% endtab %}
{% endtabs %}

_Note: Inbox Vision is currently unavailable during this testing phase and will be made available in the future_

## Creative Details 

### Auto Width Images

Images added to your email will automatically be set to __auto width__. To adjust this setting, toggle off __auto width__ and adjust the width percentage as needed. 

![Drag & Drop Email PNG][2]

### Color Layering

The Drag & Drop Editor allows you to change the color of the email background, content area, and different content components. The color ordering from front to back is as listed: content component color, content area background color, and background color. 

![Drag & Drop Email PNG][3]

### Content Padding

![Drag & Drop Email PNG][4]{: style="float:right;max-width:25%;margin-left:15px;"}

To adjust padding, scroll down to __Block Options__, and toggle __More Options__. This will allow you to fine-tune your padding to get your email looking just right!
<br><br>
### Adding Liquid 

![Drag & Drop Email PNG][5]{: style="float:right;max-width:25%;margin-left:15px;"}

Basic Liquid is supported in our Drag & Drop Editor. To add Liquid into your email, select __Personalization__ under __Design / Build__. 

Here, you can add various personalization types such as default attributes, device attributes, custom attributes, and more! 

Next, take your generated Liquid snippet and add it to your email.

[1]: {% image_buster /assets/img/dnd/dnd.gif %}
[2]: {% image_buster /assets/img/dnd/dnd1.png %}
[3]: {% image_buster /assets/img/dnd/dnd2.png %}
[4]: {% image_buster /assets/img/dnd/dnd3.png %}
[5]: {% image_buster /assets/img/dnd/dnd4.png %}
[6]: {% image_buster /assets/img/dnd_editor_workflow.png %}
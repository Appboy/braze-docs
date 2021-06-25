---
nav_title: Upload an Email Template
platform: Message_Building_and_Personalization
subplatform: Email
page_order: 2.1
description: "This reference article covers how to create, manage, and troubleshoot an HTML email template using the Braze dashboard."

tool:
  - Dashboard
  - Templates

channel:
  - email

---

# How to Upload an HTML Email Template

> Email messages are great for delivering content to the user on their terms. They are also wonderful tools to re-engage users who may have even uninstalled your app. The Braze dashboard has an email template editor that allows you to create custom-tailored, eye-catching emails and save them for later use in campaigns. You can also [create an Email Template]({{site.baseurl}}/user_guide/message_building_by_channel/email/creating_an_email_template/) using our editor.

## Step 1: Navigate to the Email Template Editor
Click on the __Templates & Media__ tab under __Engagement__ in the navigation bar. This will open to the __Email Template Gallery__.

## Step 2: Open the Uploader
Click on the _Upload_ option in the __Basic Email Templates__ box.

## Step 3: Upload your Template
Navigate to and upload your template from your computer.

![HTML Uploader][9]
### Upload Requirements:

Within a single .zip file, you should limit your upload to:
- A single HTML file (the body of your email),
- A folder of images that are referenced in the HTML file, and
- Less than 50 image files.

Generally, the uploaded file must be:
- Under 5MB and
- Zipped in a single file.

### Solve Upload Email Template Errors
There are several email error messages you can receive when uploading an HTML file for email. You will receive a specific error message describing each of these. Here are a few samples with their recommended fixes:

| Error | Fix |
|---|---|
|.zip over 5MB| Reduce your file size and try uploading again.|
|.zip corrupt| Inspect your file and try uploading again. |
|Missing HTML| Add HTML file to your .zip file and try uploading again.|
|Multiple HTML| Remove one of the HTML files and try uploading again.|
|Images over 5MB| Reduce the number of images and try uploading again. |
|Extra Images| You may have additional images in your file that are not referenced in your HTML file. This will not cause a fail error, but those extra images will be discarded. If those images were supposed to be referenced in the HTML file, please check its contents, correct any errors, and then try uploading again.
|Missing Images| If there are images referenced in your HTML file, but those images are not included in the image folder of the .zip file, you will receive a file error. Inspect your file and correct any errors (like misspellings), or add the missing images to your .zip file and try uploading again.|
{: .reset-td-br-1 .reset-td-br-2}

## Step 4: Finish and Save Your Template
Click "Save Template" in the bottom right corner of the editor. You can now use this template in any campaign you choose!

> Edits made to an existing template will not be reflected in campaigns created using previous versions of that template.


[4]: {% image_buster /assets/img_archive/email_templates4-new.png %}
[9]: {% image_buster /assets/img_archive/step1and2htmluploader.gif %}

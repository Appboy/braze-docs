---
nav_title: Email Open Pixel and Click Tracking
page_order: 1

page_type: reference
description: "This reference article covers how Open Pixel and Click Tracking works."
tool: 
- Dashboard
- Campaigns
- Reports
---

# Open Pixel and Click Tracking Overview

[Open Pixel Tracking][open_tracking] and Click tracking can now be disabled per user profile.   This flexibility helps customers support regional privacy laws, where an individual user profile might indicate they no longer want to be tracked.


# Implementation

When either importing or updating a user profile via [API][api_doc] or [CSV][csv_doc], two new fields are now available for you to modify.

- email_open_tracking_disabled
- email_click_tracking_disabled

_For easy reference, this information is reflected on the user profile in the Email Contact Settings._

![open_click_user_profile][1]



[open_tracking]: {{site.baseurl}}/user_guide/administrative/app_settings/manage_app_group/email_settings/#email-open-tracking-pixel
[api_doc]: {{site.baseurl}}/api/objects_filters/user_attributes_object/#braze-user-profile-fields
[csv_doc]: {{site.baseurl}}/user_guide/data_and_analytics/user_data_collection/user_import/#standard-user-data-column-headers
[1]: {% image_buster /assets/img_archive/open_click_user_profile.png %}

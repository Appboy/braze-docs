---
nav_title: "Email Lists & Addresses"
page_order: 0
layout: featured

#Required
description: "This landing page explains and lists the Braze Email Lists and Addresses Endpoints."
page_type: landing
tool:
  - Canvas
  - Campaigns

#Use if applicable
platform:
  - API
channel:
  - Email

guide_top_header: "Email Lists & Addresses Endpoints"
guide_top_text: "Users’ email subscription status can be updated and retrieved via Braze using a RESTful API. You can use the API to set up bi-directional sync between Braze and other email systems or your own database."

guide_featured_title: ""
guide_featured_list:
  - name: "GET: List Hard Bounces"
    link: /docs/api/endpoints/email/get_list_hard_bounces/
    fa_icon: fas fa-reply-all
  - name: "GET: Query Unsubscribed Email Addresses"
    link: /docs/api/endpoints/email/get_query_unsubscribed_email_addresses/
    fa_icon: fas fa-envelope
  - name: "GET: Query by Email Address"
    link: /docs/api/endpoints/email/get_subscription/
    fa_icon: fas fa-reply-all
  - name: "POST: Change Email Subscription Status"
    link: /docs/api/endpoints/email/post_email_subscription_status/
    fa_icon: fas fa-at
  - name: "POST: Remove Hard Bounces"
    link: /docs/api/endpoints/email/post_remove_hard_bounces/
    fa_icon: fas fa-reply-all
  - name: "POST: Remove Spam"
    link: /docs/api/endpoints/email/post_remove_spam/
    fa_icon: fas fa-envelope-open
  - name: "POST: Blacklist Email"
    link: /docs/api/endpoints/email/post_blacklist/
    fa_icon: fas fa-envelope-open
---
{% comment %}
redirect from email_sync.md
{% endcomment %}

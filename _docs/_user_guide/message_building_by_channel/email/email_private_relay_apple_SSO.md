---
nav_title: Apple Private Relay
alias: /email_relay/
page_order: 8
description: "This article covers the process of sending emails to Apple Private Relay. This will allow Sendgrid users to whitelist without haveing to make DNS changes."
tool:
  - Dashboard

channel:
  - email
---

# Sending Emails to Apple Private Relay

Braze customers who use Sendgrid as an email provider can now, essentially, "[whitelist](https://help.apple.com/developer-account/?lang=en#/devf822fb8fc)" with Apple without having to make DNS changes.

Go to your [Apple Certificate](https://help.apple.com/developer-account/?lang=en#/devf822fb8fc) page and whitelist the email address you wish to use for sending via Apple's Email Relay Service (your desired `From` address). 

![Whitelabel the Address]({% image_buster /assets/img/email-relay-whitelabel-address.png %})

To find the proper address, go to your Sendgrid DNS record and copy the UID, Whitelabel Subdomain, and Domain from the Host Value column. 

![HOST Value DNS Records]({% image_buster /assets/img/email-relay-dns-records.png %})

The address should be formatted as:

`bounces+<YOUR_UID>@<YOUR_WHITELABELED_SUBDOMAIN_AND_DOMAIN>`

For example: `bounces+1234567@braze.online.docs.com`.

Once added to your Apple Certificate page, emails from this `From` address domain will be delivered via the Apple Private Relay system.

If you have any further questions, please [submit a support ticket]({{site.baseurl}}/support_contact/).

{% alert important %}
If your desired `From` address is an `abmail` address, please include that in your subdomain. For example, use `abmail.docs.braze.com` instead of `docs.braze.com`.

This might not be the case for your address. Please check your DNS records in Sendgrid. 
{% endalert %}

## From Address Components for Whitelisting with Apple Email Relay

| Value | Description |
|---|---|
| UID | This value is provided by Sendgrid in your DNS records. Do not include the "U" character in your UID in the email address you whitelabel. For example, if your UID is presented in Sendgrid as `u1234567.wl134.sendgrid.net`, then `1234567` is the UID value. <br> <br> _You can also ask your Braze representative to provide your UID, if you do not have access to your DNS records._ |
| Whitelabeled Subdomain and Domain | This is the initial domain and subdomain you entered into Sendgrid. You can also use the HOST value in your DNS Records in Sendgrid. <br> <br> ![HOST Value DNS Records]({% image_buster /assets/img/email-relay-dns-records.png %}) |
{: .reset-td-br-1 .reset-td-br-2}


## What is the Apple Private Relay System?

With the recent iOS 13 release, Apple has introduced new functionality for Apple customers, which impacts how email is sent to them. The new Apple single sign-on (SSO) feature allows Apple customers to share their email address (`example@icloud.com`) or to hide their email address, in which case a "masked" email address (`tq1234snin@privaterelay.appleid.com`) will be provided to brands (as opposed to the user's personal email address).

### Disable forwarding

Users can manage the apps using Sign In With Apple from their Apple ID settings page (see [Apple's Documentation](https://support.apple.com/en-us/HT210426)).

Should a user choose to disable the email forwarding to _your_ app's relay email, Braze will receive email bounce information as usual.

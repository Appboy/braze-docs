---
nav_title: Okta
page_order: 4
---

# Okta
Okta connects any person with any application on any device. It's an enterprise-grade, identity management service, built for the cloud, but compatible with many on-premises applications. With Okta, IT can manage any employee's access to any application or device.

## Requirements

- Okta must be turned on for your account.  Reach out to your Braze Account Manager to have this turned on.
- You must have admin privileges for both Okta and Braze.

## Step 1: Configure Braze

1. Log into your Braze account using an admin account.
2. Click on the drop down from your user name in the upper right corner, select `Company Settings`.
3. Select the `Security Settings` tab.
4. Turn on the `SAML SSO` switch.
5. Enter the `SAML Name` which will appear as the button text on the login screen. This is typically your IdP name.
6. Enter the `Target URL` with the `Embed Link` from the Okta Admin Dashboard.
7. Enter the Certificate with the Certificate you downloaded from the Okta Admin Dashboard (open the file, copy, and paste).
8. Click `Save Changes` at the bottom of the page.

## Step 2: Enable the IdP-initiated Flow

Create your Braze API Key with `sso.saml.login` permission enabled.

![SSO Set Up]({% image_buster /assets/img/sso2.png %})

{% alert important %}
If you do not already have a Braze API Key, go to the `Developer Console` in `App Settings`, then click `Create New API Key`.

Then, scroll down to the SSO section and check the `sso.saml.login` option and then save the API Key.
{% endalert %}

## Step 3: Configure Okta

1. In Okta, select the `Sign On` tab for the Braze SAML app, then click `Edit`.
2. Enter your Braze API Key value from the previous step into the `Default Relay State:` field.
3. Click `Save`.

You should now be able to log into Braze using Okta!

{% alert tip %}
If you want your Braze account users to only sign in with SAML SSO, you can [restrict single sign-on authentication]({{site.baseurl}}/user_guide/administrative/access_braze/single_sign_on/restriction/) from the `Company Settings` page.
{% endalert %}

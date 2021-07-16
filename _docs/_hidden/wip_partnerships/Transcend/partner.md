---
nav_title: Transcend
page_order: 1

description: "Transcend automates fulfillment of data subject requests by orchestrating data across dozens of data systems, including Braze."
alias: /partners/transcend/

page_type: partner
hidden: true
---

# Transcend

Transcend is the data privacy infrastructure that makes it simple for companies to give their users control over their personal data. Personal data is disorganized, hard to spot, and stored across many systems. That makes offering data rights—like deleting your data—very hard for companies. Transcend automatically fulfills data subject requests inside companies, across all of their data systems and vendors. Transcend also provides their end-users with a control panel, or “Privacy Center”, hosted at privacy.\<company\>.com where users can manage their privacy preferences, export their data, or delete it. By making data subject requests painless for companies, Transcend is putting users everywhere in the driver’s seat of their personal data.

Transcend automates privacy requests by orchestrating data across dozens of data systems, including Braze. Ultimately, this helps teams comply with regulations like GDPR and CCPA and puts individuals in the driver's seat when it comes to their data.

## Requirements or Prerequisites

| Requirement | Origin | Access | Description |
|---|---|---|---|
| Transcend Account & Account Information | Transcend | https://app.transcend.io/ | An active Transcend account with admin privileges is required to utilize the Braze integration. An API Key and Subdomain will be required to connect.
| Braze API Key | Braze | You will need to create a new API Key.<br><br>This can be created in the __Developer Console -> API Settings -> Create New API Key__ with __users.track__ permissions. | This description should tell you what to do with the Braze API Key. |

{% alert important %}
The requirements listed below are typical requirements you might need from Braze. We recommend using the attributed titling, origin, links, and phrasing as listed in the chart below. Be sure to adjust the description so that you know what each of these requirements is used to do.
{% endalert %}

## Transcend and Braze Integration

Transcend allows you to programmatically access, erase, and opt users out of communication in the Braze platform in accordance with data privacy regulations like GDPR and CCPA.

### Step 1: Setting up the Braze integration

__To get started__, make sure you're logged in to Transcend. If not, log in to Transcend [here](https://app.transcend.io/login "Transcend Admin Dashboard - Login"). To configure the Braze integration, you'll want to:

1. Navigate to Data Map > Add data silo > Braze.
2. When your account is provisioned you will log in to one of the corresponding URLs: https://dashboard-01.braze.com, https://dashboard-02.braze.com, ...,  https://dashboard-01.braze.eu.
3. Use the table in the following URL: https://www.braze.com/docs/api/basics/?redirected=true#api-definitions to figure out what subdomain you should plug in based on your dashboard URL.
4. To create a new REST API Key, visit the Developer Console: https://dashboard-01.braze.com/sign_in on your Braze Dashboard.
5. Success! Once connected, navigate to the Privacy Center tab. You'll need to map the data in Braze to your Data Practices. To do this, create a new Category as well as a new Data Collection with the appropriate naming convention (e.g. "Mailing Lists or User Profile"). When you're done, hit publish.
6. Navigate back to your Data Map and click into the Braze data silo. Expand "Manage Datapoints" and select the Collection Label (Category) you created in the previous step from the dropdown. You can also choose which data actions (e.g. access or erasure) are enabled for which datapoints.
7. Next, while still in the Braze data silo, expand "Manage Identifiers". Check the respective boxes for which identifiers you'd like enabled. As an example, if you'd like Transcend to search users by email address, you'd check the box to enable the Email Address identifier. Note: if identifiers are not enabled correctly, we may not be able to process requests for certain users.
8. Done!

### Step 2: Testing

We recommend testing requests across your Data Map before you start processing requests from end-users. To do this:

1. Go to Privacy Center and click "View your Privacy Center".
2. From your Privacy Center, click Take Control, then Download my data. You'll need to enter your email or login to authenticate yourself before submitting the request.
3. Check your email for a message from Transcend. You'll be asked to click on a verification link to verify the request. Confirm your download request.
4. Next, back in the Admin Dashboard, navigate to the Incoming Requests tab and select your request. If you don't see the request here, contact us at support@transcend.io.
5. Once you've clicked into your request, navigate to the Data Silos tab and select Braze. Inspect the data returned.
6. Finally, navigate to the Report tab and click Approve and Send. You should receive the report at the email address you submitted with the request.
7. Done!

### Step 3: Removing the Braze integration

To remove the Braze data silo from your Data Map, navigate to your Data Map, and click into Braze. At the bottom of the screen, expand "Remove Braze". Click Remove Silo. You'll be prompted to confirm that you'd like to remove the silo, click Ok. Confirm the silo has been removed by navigating back to your Data Map.

[1]: {{site.baseurl}}/developer_guide/rest_api/basics/#endpoints)

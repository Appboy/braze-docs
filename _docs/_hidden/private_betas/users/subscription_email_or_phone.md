---
nav_title: Subscription Information based on Email Address or Phone Number
permalink: "/users/subscriptions_main/"
hidden: true
---

## Overview:
Email addresses and phone numbers are not unique in the Braze Services. This means your team could have configured Braze to store the same email address or phone number on multiple user profiles. If your team has configured Braze in this way, and has subsequently sent User Delete requests targeting a specific user profiles, the association with a channel identifier, either an email address or phone number, and the subscription state associated with that identifier could still be retained by Braze.  To ensure that both the channel identifiers and their associated subscription states areinformation is no longer kept in Braze, please make the following API calls or reach out to the Support team to remove any identifier no longer associated with a User Profile from the Braze system.

### Removing email addresses which had previously Hard Bounced

Call the users/delete endpoint to delete any and all user profiles associated with the email address from Braze
You can call /users/export/ids by email address or phone number to retrieve all the braze_id(s) to use when making the users/delete API call. (https://www.braze.com/docs/api/endpoints/export/user_data/post_users_identifier/) 
Call /users/subscription using the Delete method and pass both the email address and phone number associated with the user profiles that have been deleted.
Call /users/subscription using the GET method to verify that the email address or phone number associated with the deleted user profile(s) are no longer kept by Braze.
If your GET call returns information suggesting that the email address or phone number is still stored the following can help you troubleshoot
- Delete any user profile that shares the email address or phone number


### Removing the subscription states based on an email address or phone number

Call the users/delete endpoint to delete any and all user profiles associated with the email address or phone numbers from Braze
You can call /users/export/ids by email address or phone number to retrieve the braze_id to use when making the users/delete API call. (https://www.braze.com/docs/api/endpoints/export/user_data/post_users_identifier/) 
Call /users/subscription using the [Delete](https://www.braze.com/docs/users/delete_subscription) method and pass both the email address and phone number associated with the user profiles that have been deleted.
Call /users/subscription using the [GET](https://www.braze.com/docs/users/get_subscription) method to verify that the email address or phone number associated with the deleted user profile(s) are no longer kept by Braze.
If your GET call returns information suggesting that the email address or phone number is still stored the following can help you troubleshoot
- Delete any user profile that shares the email address or phone number

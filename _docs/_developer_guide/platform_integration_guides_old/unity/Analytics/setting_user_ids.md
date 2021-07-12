---
nav_title: Setting User IDs
platform: Unity
page_order: 0
description: "This reference article covers how to set user ids on Unity platform."

---

# Setting User IDs

User IDs should be set for each of your users. These should be unchanging and accessible when a user opens the app. Naming your User IDs correctly from the start is one of the most __crucial__ steps when setting up User IDs. We strongly suggest using the Braze standard of UUIDs/GUIDs (detailed below). We also, strongly recommend providing this identifier as it will allow you to:

- Track your users across devices and platforms, improving the quality of your behavioral and demographic data.
- Import data about your users using our [User Data API][1].
- Target specific users with our [Messaging API][2] for both general and transactional messages.

> If such an identifier is not available, Braze will assign a unique identifier to your users, but you will lack the capabilities above.

## Suggested User ID Naming Convention

At Braze, we __strongly suggest__ naming User IDs also known as `external_user_ids`, in a [UUIDs/GUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier) format. UUIDs/GUIDs are Universally Unique Identifiers that consist of a 128-bit number used to identify information in computer systems. This means that these UUIDs are long, random and well distributed. If you choose a different method in which to name your User IDs, they must also be long, random and well distributed. It is also important to note, that User IDs are __case sensitive__. For example, "Abcdef" is a different user from "abcdef". 

If you find your `external_user_ids` include names, email addresses, timestamps, or incrementors we __strongly suggest__ picking up a new naming method that is more secure. We do not want names, email address, timestamps or incrementors included in your User IDs, because while it might be easy for people within your organization to quickly identify others, __it is not a secure method__. 

Providing this information to others may allow people outside your organization to glean information on how your User IDs are structured, opening up your organization to potentially malicious updates or removal of information. Choosing the correct naming convention from the start is one of the most important steps in setting up User IDs, however a migration is possible using our [External ID Migration API Endpoint]({{site.baseurl}}/api/endpoints/user_data/external_id_migration/).

| User ID Naming |
| Good Example | Bad Example |
| ------------ | ----------- |
| 123e4567-e89b-12d3-a456-836199333115 | JonDoe829525552 |
| 83nmas45-eks1-083m-mk36-426655440000 | Anna@email.com |
| Mbfjla32-937z-09es-sbv6-064026245228 | CompanyName-1-2-19 |
| k6twn923-8234-7354-lzpd-139317000652 | jon-doe-1-2-19 |

### Assigning a User ID

You should make the following call as soon as the user is identified (generally after logging in) in order to set the user id:

```csharp
AppboyBinding.ChangeUser("YOUR_USER_ID_STRING");
```

>  __Do not call `ChangeUser()` when a user logs out. `ChangeUser()` should only be called when the user logs into the application.__ Setting `ChangeUser()` to a static default value will associate ALL user activity with that default "user" until the user logs in again.

Additionally, we recommend against changing the user ID when a user logs out, as it makes you unable to target the previously logged-in user with reengagement campaigns. If you anticipate multiple users on the same device, but only want to target one of them when your app is in a logged out state, we recommend separately keeping track of the user ID you want to target while logged out and switching back to that user ID as part of your app's logout process.

### Automatic Preservation of Anonymous User History

| Identification Context | Preservation Behavior |
| ---------------------- | -------------------------- |
| User __has not__ been previously identified | Anonymous history __is merged__ with user profile upon identification |
| User __has been__ previously identified in-app or via API | Anonymous history __is not merged__ with user profile upon identification |
{: .reset-td-br-1 .reset-td-br-2}

### Additional Notes and Best Practices
Please note the following:

- __If your app is used by multiple people, you can assign each user a unique identifier to track them.__
- __Once a user ID has been set, you cannot revert that user to an anonymous profile__
- __Do Not change the user ID upon a user "log out".__
  - Doing so separates the device from the user profile. You will be unable to target the previously logged out user with re-engagement messages. If you anticipate multiple users on the same device, but only want to target one of them when your app is in a logged out state, we recommend separately keeping track of the user ID you want to target while logged out and switching back to that user ID as part of your app's logout process. By default, only the last user that was logged in will receive push notifications from your app.
- __Switching from one identified user to another is a relatively costly operation.__
  - When you request the user switch, the current session for the previous user is automatically closed and a new session is started. Furthermore, Braze will automatically make a data refresh request for the News Feed, in-app messages, and other Braze resources for the new user.

> If you opt to use a hash of a unique identifier as your userID take care to ensure that you're normalizing the input to your hashing function. For example,if you're going to use a hash of an email address, ensure that you're stripping leading and trailing whitespace from the input, and taking [localization problems][6] into account.

[1]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[2]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[6]: http://developer.android.com/reference/java/util/Locale.html#default_locale "Android Developer Docs - Localization"

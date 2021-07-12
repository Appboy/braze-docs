---
nav_title: Setting User IDs
platform: Roku
page_order: 0

page_type: reference
description: "This page covers methods to identify users, as well as best practices and important considerations."

---

# Setting User IDs

User IDs should be set for each of your users. These should be unchanging and accessible when a user opens the app. Naming your User IDs correctly from the start is one of the most __crucial__ steps when setting up User IDs. We strongly suggest using the Braze standard of UUIDs/GUIDs (detailed below). We also, strongly recommend providing this identifier as it will allow you to:

- Track your users across devices and platforms, improving the quality of your behavioral and demographic data.
- Import data about your users using our [User Data API][1].
- Target specific users with our [Messaging API][2] for both general and transactional messages.

{% alert note %}
If such an identifier is not available, Braze will assign a unique identifier to your users, but you will lack the capabilities above. You should avoid setting User IDs for users for whom you lack a unique identifier that is tied to them as an individual. Passing a device identifier offers no benefit versus the automatic anonymous user tracking Braze offers by default.
{% endalert %}

{% alert warning %}
These User IDs should be private and not easily obtained (e.g. not a plain email address or username).
{% endalert %}

You should make the following call as soon as the user is identified (generally after logging in) in order to set the user ID:

```
m.Braze.setUserId(YOUR_USER_ID_STRING)
```

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

## User ID Integration Best Practices & Notes

### Automatic Preservation of Anonymous User History

| Identification Context | Preservation Behavior |
| ---------------------- | -------------------------- |
| User __has not__ been previously identified | Anonymous history __is merged__ with user profile upon identification |
| User __has been__ previously identified in-app or via API | Anonymous history __is not merged__ with user profile upon identification |
{: .reset-td-br-1 .reset-td-br-2}

### Additional Notes and Best Practices

Please note the following:

- If your app is used by multiple people, you can assign each user a unique identifier to track them.
- Once a user ID has been set, you cannot revert that user to an anonymous profile.
- Do Not change the user ID upon a user "log out".
  - Doing so separates the device from the user profile. If you anticipate multiple users on the same device, but only want to target one of them when your app is in a logged-out state, we recommend separately keeping track of the user ID you want to target while logged out and switching back to that user ID as part of your app's logout process.
- Switching from one identified user to another is a relatively costly operation.
  - When you request the user switch, the current session for the previous user is automatically closed and a new session is started.

> If you opt to use a hash of a unique identifier as your user ID take care to ensure that you're normalizing the input to your hashing function. For example, if you're going to use a hash of an email address, ensure that you're stripping leading and trailing whitespace from the input, and taking localization into account.

[1]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[2]: {{site.baseurl}}/developer_guide/rest_api/messaging/

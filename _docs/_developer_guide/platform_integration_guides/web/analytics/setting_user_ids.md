---
nav_title: Setting User IDs
platform: Web
page_order: 1
search_rank: 5
---
## Setting User IDs

User IDs should be set for each of your users. These should be unchanging and accessible when a user opens the app. A database ID or a hashed email address/username is usually a good reference to use. We __strongly recommend__ providing this identifier as it will allow you to:

- Track your users across devices and platforms, improving the quality of your behavioral and demographic data.
- Import data about your users using our [User Data API][1].
- Target specific users with our [Messaging API][2] for both general and transactional messages.

{% alert note %}
If such an identifier is not available, Braze will assign a unique identifier to your users, but you will lack the capabilities above. You should avoid setting User IDs for users for whom you lack a unique identifier that is tied to them as an individual. Passing a device identifier offers no benefit versus the automatic anonymous user tracking Braze offers by default.
{% endalert %}

{% alert warning %}
These User IDs should be private and not easily obtained (e.g. not a plain email address or username).
{% endalert %}

You should make the following call as soon as the user is identified (generally after logging in) in order to set the user id:

```javascript
appboy.changeUser(YOUR_USER_ID_STRING);
```

>  __Do not call `changeUser()` when a user logs out.__ Setting `changeUser()` to a static default value will associate ALL user activity with that default "user" until the user logs in again.
Additionally, we recommend against changing the user ID when a user logs out, as it makes you unable to target the previously logged-in user with reengagement campaigns. If you anticipate multiple users on the same device, but only want to target one of them when your app is in a logged out state, we recommend separately keeping track of the user ID you want to target while logged out and switching back to that user ID as part of your app's logout process.

Refer to the [changeUser documentation][4] for more information.

### Automatic Preservation of Anonymous User History

| Identification Context | Preservation Behavior |
| ---------------------- | -------------------------- |
| User __has not__ been previously identified | Anonymous history __is merged__ with user profile upon identification |
| User __has been__ previously identified in-app or via API | Anonymous history __is not merged__ with user profile upon identification |

### Additional Notes and Best Practices
Please note the following:

- __If your app is used by multiple people, you can assign each user a unique identifier to track them.__
- __Once a user ID has been set, you cannot revert that user to an anonymous profile__
- __Do Not change the user ID upon a user "log out".__
  - Doing so separates the device from the user profile. You will be unable to target the previously logged out user with re-engagement messages. If you anticipate multiple users on the same device, but only want to target one of them when your app is in a logged out state, we recommend separately keeping track of the user ID you want to target while logged out and switching back to that user ID as part of your app's logout process. By default, only the last user that was logged in will receive push notifications from your app.
  - When you request the user switch, the current session for the previous user is automatically closed and a new session is started. Furthermore, Braze will automatically make a data refresh request for the News Feed, in-app messages, and other Braze resources for the new user.

> If you opt to use a hash of a unique identifier as your userID take care to ensure that you're normalizing the input to your hashing function. For example, if you're going to use a hash of an email address, ensure that you're stripping leading and trailing whitespace from the input.

## Aliasing Users

{% include archive/aliasing.md platform="Web" %}

[1]: {{ site.baseurl }}/developer_guide/rest_api/user_data/#user-data
[2]: {{ site.baseurl }}/developer_guide/rest_api/messaging/
[4]: https://js.appboycdn.com/web-sdk/latest/doc/module-appboy.html#.changeUser "Javadocs"

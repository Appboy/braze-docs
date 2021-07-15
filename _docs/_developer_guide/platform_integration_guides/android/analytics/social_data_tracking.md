---
nav_title: Social Data Tracking
platform: Android
page_order: 5
description: "This reference article shows how to implement social data tracking for your Android application."

---

# Social Data Tracking
Similar to the Braze iOS SDK, the Braze Android SDK does not automatically collect Facebook and Twitter data. However, it's possible to add social media data to a Braze user's profile from the Android SDK as well:

- Obtain social media data within your app via the Facebook SDK and Twitter APIs.
  - [Facebook Documentation][1]
  - [Twitter Documentation][2]
- Initialize Facebook and Twitter User objects with social media data and pass them to Braze.

## Social Network Data Constructors

{% tabs %}
{% tab JAVA %}

```java
FacebookUser(
  String facebookId,
  String firstName,
  String lastName,
  String email,
  String bio,
  String cityName,
  // Gender is a Braze enum.
  Gender gender,
  Integer numberOfFriends,
  // Names of pages the user likes.
  Collection<String> likes,
  // mm/dd/yyyy format.
  String birthday
)
TwitterUser(
  Integer twitterUserId,
  String twitterHandle,
  String name,
  String description,
  Integer followerCount,
  Integer followingCount,
  Integer tweetCount,
  String profileImageUrl
)
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
FacebookUser(
  facebookId: String,
  firstName: String,
  lastName: String,
  email: String,
  bio: String,
  cityName: String,
  // Gender is a Braze enum.
  gender: Gender gender,
  numberOfFriends: Integer,
  // Names of pages the user likes.
  likes: Collection<String>,
  // mm/dd/yyyy format.
  birthday: String
)
TwitterUser(
  twitterUserId: Integer,
  twitterHandle: String,
  name: String,
  description: String,
  followerCount: Integer,
  followingCount: Integer,
  tweetCount: Integer,
  profileImageUrl: String
)
```

{% endtab %}
{% endtabs %}

To pass data retrieved from social networks to Braze, you'll create a new FacebookUser or TwitterUser and then pass them to the method `AppboyUser.setFacebookData()`/`AppboyUser.setTwitterData()`. For example:

{% tabs %}
{% tab JAVA %}

```java
FacebookUser facebookUser = new FacebookUser("100000", "FirstName", "LastName", "email@email.com", "bio", "City", Gender.MALE, 3, Arrays.asList(new String[]{ "like" }), "04/13/1990");
Appboy.getInstance(context).getCurrentUser().setFacebookData(facebookUser);

TwitterUser twitterUser = new TwitterUser(100000, "handle", "Name", "description", 100, 50, 150, "image_url");
Appboy.getInstance(context).getCurrentUser().setTwitterData(twitterUser);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val facebookUser = FacebookUser("100000", "FirstName", "LastName", "email@email.com", "bio", "City", Gender.MALE, 3, listOf("like"),"04/13/1990")
Braze.getInstance(context).currentUser?.setFacebookData(facebookUser)

val twitterUser = TwitterUser(100000, "handle", "Name", "description", 100, 50, 150, "image_url")
Braze.getInstance(context).currentUser?.setTwitterData(twitterUser)
```

{% endtab %}
{% endtabs %}

[1]: https://developers.facebook.com/docs/howtos/androidsdk/3.0/login-with-facebook/#step1
[2]: https://developer.twitter.com/en/docs

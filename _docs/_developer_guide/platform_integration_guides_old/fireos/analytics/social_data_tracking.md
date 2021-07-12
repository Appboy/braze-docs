---
nav_title: Social Data Tracking
platform: FireOS
page_order: 5

page_type: reference
description: "This article covers adding soial media data to a Braze user's profile from the Android SDK."
---

# Social Data Tracking
Similar to the Braze iOS SDK, the Braze Android SDK does not automatically collect Facebook and Twitter data. However, it's possible to add social media data to a Braze user's profile from the Android SDK as well:

- Obtain social media data within your app via the Facebook SDK and Twitter APIs.
  - [Facebook Documentation][1] // [Twitter Documentation][2]
- Initialize Facebook and Twitter User objects with social media data and pass them to Braze.

## Social Network Data Constructors

```java
FacebookUser(
  String facebookId,
  String firstName,
  String lastName,
  String email,
  String bio,
  String cityName,
  // Gender is a Braze enum.
  // Specify either Gender.MALE or Gender.FEMALE.
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

To pass data retrieved from social networks to Braze, you'll create a new FacebookUser or TwitterUser and then pass them to the method `AppboyUser.setFacebookData()`/`AppboyUser.setTwitterData()`. For example:

```java
FacebookUser facebookUser = new FacebookUser("100000", "FirstName", "LastName", "email@email.com", "bio", "City", Gender.MALE, 3, ,"04/13/1990");
AppboyUser.setFacebookData(facebookUser);

String imageUrl = "https://si0.twimg.com/profile_images/000/0000.jpeg";
TwitterUser twitterUser = new TwitterUser(100000, "handle", "Name", "description", 100, 50, 150, imageUrl);
AppboyUser.setTwitterData(twitterUser);

```


[1]: https://developers.facebook.com/docs/howtos/androidsdk/3.0/login-with-facebook/#step1
[2]: https://developer.twitter.com/en/docs

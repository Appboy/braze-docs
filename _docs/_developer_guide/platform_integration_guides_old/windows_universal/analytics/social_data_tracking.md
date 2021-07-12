---
nav_title: Social Data Tracking
platform: Windows_Universal
page_order: 5
description: "This reference article covers how to deal with social data tracking on the Windows Universal platform."

---

# Social Data Tracking

Unlike the Braze iOS SDK, the Braze Windows SDK does not automatically collect Facebook and Twitter data. However, it's possible to add social media data to a Braze user's profile from the Windows SDK as well:

- Obtain social media data within your application via the Facebook SDK and Twitter APIs.
    - [Facebook Documentation][1] // [Twitter Documentation][2]
- Initialize Facebook and Twitter User objects with social media data and pass them to Braze.

## Social Network Data Constructors

```
FacebookUser(
  string Id,
  string FirstName,
  string LastName,
  string Email,
  // mm/dd/yyyy format.
  string Birthday,
  string Bio,
  FacebookLocation LocationObject,
  // "m" or "f".
  string Gender,
  List<FacebookLike> Likes,
  int NumFriends
)

FacebookLocation(
  string CityName
)

FacebookLike(
  // The name of a page the user likes.
  string Like
)

TwitterUser(
  string Description,
  int FollowersCount,
  int FriendsCount,
  int StatusesCount,
  // Twitter's unique id for the user.
  int Id,
  string Name,
  string ProfileImageURL,
  // The user's handle.
  string ScreenName
)
```

To pass data retrieved from social networks to Braze, you'll create a new FacebookUser or TwitterUser and then pass them to the method Appboy.SharedInstance.AppboyUser.SetFacebookData()/Appboy.SharedInstance.AppboyUser.SetTwitterData(). For example:

### Twitter

```
var twitterUser = new TwitterUser {
  Description = "description",
  FollowersCount = 10,
  FriendsCount = 20,
  StatusesCount = 150,
  Id = 1000000,
  Name = "Name",
  ProfileImageURL = "https://si0.twimg.com/profile_images/00000/00000.jpeg",
  ScreenName = "handle"
};

Appboy.SharedInstance.AppboyUser.SetTwitterData(twitterUser);
```

### Facebook

```
// Build a list of pages the user likes.
List likes = new List();
var like = new FacebookLike {
  Like = "Page Name"
};
likes.Add(like);

// Specify the user's city in a FacebookLocation object.
var location = new FacebookLocation {
  CityName = "City"
};

// Populate the FacebookUser object.
var facebookUser = new FacebookUser {
  Id = "100000",
  FirstName = "FirstName",
  LastName = "LastName",
  Email = "email@email.com",
  Birthday = "04/13/1990",
  Bio = "bio",
  LocationObject = location,
  Gender = "m",
  Likes = likes,
  NumFriends = 500
};

Appboy.SharedInstance.AppboyUser.SetFacebookData(facebookUser);
```

[1]: https://developers.facebook.com/docs/facebook-login/manually-build-a-login-flow/ "facebook"
[2]: https://developer.twitter.com/en/docs "twitter"

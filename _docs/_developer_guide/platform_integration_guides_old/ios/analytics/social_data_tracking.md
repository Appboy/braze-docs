---
nav_title: Social Data Tracking
platform: iOS
page_order: 5
description: "This reference article shows how to implement social data tracking for your iOS application."

---

# Social Data Tracking

## Collecting Social Account Data

The Braze iOS SDK does not automatically collect Facebook or Twitter user data. If you want to integrate Facebook user data in Braze user profiles, you need to fetch the user's data and pass it to Braze.

## Passing Facebook Data To Braze

Initialize `ABKFacebookUser` objects with the Facebook data you have collected and pass it to Braze:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
ABKFacebookUser *facebookUser = [[ABKFacebookUser alloc] initWithFacebookUserDictionary:self.facebookUserProfile numberOfFriends:self.numberOfFacebookFriends likes:self.facebookLikes];
[Appboy sharedInstance].user.facebookUser = facebookUser;
```

{% endtab %}
{% tab SWIFT %}

```swift
let facebookUser = ABKFacebookUser(facebookUserDictionary: facebookUserDictionary, numberOfFriends: numberOfFriends, likes: likes)
Appboy.sharedInstance()?.user.facebookUser = facebookUser
```

{% endtab %}
{% endtabs %}

>  In ABKFacebookUser's init method `initWithFacebookUserDictionary:numberOfFriends:likes:`, all the parameters should be dictionaries and arrays returned directly from Facebook:

| Parameter | Definition |
| --------- | ---------- |
|`facebookUserProfile`| The dictionary returned from the endpoint "/me".|
| `numberOfFriends`| The length of the friends array returned from the endpoint "/me/friends".|
| `likes` | The array of user's Facebook likes from the endpoint "/me/likes". |
{: .reset-td-br-1 .reset-td-br-2}

>  For additional information regarding the Facebook Graph API, please refer to [the Facebook Graph API Developer Documentation][10].

Additionally, you can tailor what Facebook data you're sending to Braze, in case you don't want to include the entire basic profile. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
ABKFacebookUser *facebookUser = [[ABKFacebookUser alloc] initWithFacebookUserDictionary:facebookUserPublicProfile numberOfFriends:-1 likes:nil];  
```

{% endtab %}
{% tab SWIFT %}

```swift
let facebookUser = ABKFacebookUser(facebookUserDictionary: facebookUserDictionary, numberOfFriends: -1, likes:nil)
```

{% endtab %}
{% endtabs %}

For more information about integrating the Facebook SDK, follow the steps in [Facebook SDK documentation][2].

## Passing Twitter Data To Braze

Initialize `ABKTwitterUser` objects, set up the Twitter data you have collected and pass it to Braze:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
ABKTwitterUser *twitterUser = [[ABKTwitterUser alloc] init];
twitterUser.userDescription = self.userDescription;
twitterUser.twitterID = self.twitterID;
[Appboy sharedInstance].user.twitterUser = twitterUser;
```

{% endtab %}
{% tab SWIFT %}

```swift
let twitterUser = ABKTwitterUser()
twitterUser.userDescription = twitterDserDescription
twitterUser.twitterID = twitterID
Appboy.sharedInstance()?.user.twitterUser = twitterUser
```

{% endtab %}
{% endtabs %}

[2]: https://developers.facebook.com/docs/ios "facebook iOS sdk docs"
[10]: https://developers.facebook.com/docs/graph-api/reference/v4.0/user "facebook graph api docs"

---
nav_title: News Feed
platform: tvOS
page_order: 2

page_type: reference
description: "This page describes how to fetch and display News Feed data in your tvOS application."

---

# News Feed

The News Feed is a fully customizable in-app content feed for your users. Our targeting and segmentation allows you to create a stream of content that is individually catered to the interests of each user. Depending on their position in the user life cycle and the nature of your app, this could be an on-boarding content server, an advertisement center, an achievement center, or a generic news center.

## tvOS Feed Integration
Our tvOS SDK supports fetching your News Feed data, such that you can display the News Feed in your application with your own custom UI.  To fetch the News Feed, call

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
NSArray *feedCards =  [[Appboy sharedInstance].feedController getNewsFeedCards];
```

{% endtab %}
{% tab swift %}

```swift
let feedCards = Appboy.sharedInstance()?.feedController.newsFeedCards
```

{% endtab %}
{% endtabs %}

and then parse each card by inspecting its class.

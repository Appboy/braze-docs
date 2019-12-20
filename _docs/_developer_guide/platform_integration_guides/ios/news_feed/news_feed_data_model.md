---
nav_title: News Feed Data Model
platform: iOS
page_order: 6
search_rank: 5
---

# News Feed Data Model

## Getting the Data

To access the News Feed data model, subscribe to News Feed update events:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Subscribe to feed updates
// Note: you should remove the observer where appropriate
[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(feedUpdated:)
                                             name:ABKFeedUpdatedNotification
                                           object:nil];
```                                           

```objc
// Called when the feed is refreshed (via `requestFeedRefresh`)
- (void)feedUpdated:(NSNotification *)notification {
  BOOL updateIsSuccessful = [notification.userInfo[ABKFeedUpdatedIsSuccessfulKey] boolValue];
  // check for success
  // get the cards using [[Appboy sharedInstance].feedController getCardsInCategories:ABKCardCategoryAll];
}
```

{% endtab %}
{% tab swift %}

```swift
// Subscribe to feed updates
// Note: you should remove the observer where appropriate
NotificationCenter.default.addObserver(self, selector:
  #selector(feedUpdated),
  name:NSNotification.Name.ABKFeedUpdated, object: nil)
```

```swift
// Called when the feed is refreshed (via `requestFeedRefresh`)
private func feedUpdated(_ notification: Notification) {
  if let updateSuccessful = notification.userInfo?[ABKFeedUpdatedIsSuccessfulKey] as? Bool {
    // check for success
    // get the cards using Appboy.sharedInstance()?.feedController.getCardsInCategories(.all);      
  }
}
```

{% endtab %}
{% endtabs %}

If you want to change the card data after it's been sent by Braze, we recommend storing (deep copy) the card data locally, updating the data and displaying yourself. The cards are accessible via [ABKFeedController][44].

## Base Card Model

Braze has five unique card types that share a base model. Each type of card also has additional properties that are specific to each card which are listed below.

## Base Card Model Properties

- `idString` (read only) - The card's ID set by Braze
- `viewed` - This property reflects if the card is read or unread by the user
- `created` (read only) - The property is the unix timestamp of the card's creation time from Braze dashboard
- `updated` (read only) - The property is the unix timestamp of the card's latest update time from Braze dashboard
- `categories` - The list of categories assigned to the card, cards without a category will be assigned `ABKCardCategoryNoCategory`
- `extras` - An optional NSDictionary of NSString values.

### Categories

- `ABKCardCategoryNoCategory`
- `ABKCardCategoryNews`
- `ABKCardCategoryAdvertising`
- `ABKCardCategoryAnnouncements`
- `ABKCardCategorySocial`
- `ABKCardCategoryAll`

## Banner Properties
In addition to the base card properties:

- `image` (required) - This property is the URL of the card's image
- `url` (optional) - The URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `domain` (optional) - The link text for the property url, like @"blog.braze.com". It can be displayed on the card's UI to indicate the action/direction of clicking on the card, but is hidden in the default Braze News Feed.

## Captioned Image Properties
In addition to the base card properties:

- `image` (required) - This property is the URL of the card's image
- `title` (required) - The title text for the card
- `description` (required) - The body text for the card
- `url` (optional) -The URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `domain` (optional) - The link text for the property url, like @"blog.braze.com". It can be displayed on the card's UI to indicate the action/direction of clicking on the card.

## Text Announcement (Captioned Image without Image) Properties
In addition to the base card properties:

- `title` (required) - The title text for the card
- `description` (required) - The body text for the card
- `url` (optional) -The URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `domain` (optional) - The link text for the property url, like @"blog.braze.com". It can be displayed on the card's UI to indicate the action/direction of clicking on the card.

## Classic Card Properties
In addition to the base card properties:

- `image` (required) - This property is the URL of the card's image
- `title` (optional) - The title text for the card
- `description` (required) - The body text for the card
- `url` (optional) -The URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `domain` (optional) - The link text for the property url, like @"blog.braze.com". It can be displayed on the card's UI to indicate the action/direction of clicking on the card.

## Cross Promotion (Small) Properties
In addition to the base card properties:

- `mediaType` - The type of iTunes media
    - `ItunesAlbum`
    - `ItunesAudiobook`
    - `ItunesCompilation`
    - `ItunesEbook`
    - `ItunesFeatureMovie`
    - `ItunesPodcast`
    - `ItunesSoftware`
    - `ItunesSong`
    - `ItunesTvEpisode`
    - `ItunesTvSeason`
- `title` - The title text for the card. This will be the promoted item's name.
- `subtitle` - The text of the category of the promoted item
- `image` - This property is the URL of the card's image.
- `iTunesId` - The iTunes ID number of the promoted item
- `rating` (required for mediaType ItunesSoftware, optional otherwise) - The rating of the promoted app. This property will be 0.0 unless the promoted item is an app, and the rating will be in the range of [0.0, 5.0];
- `price` - The number of reviews of the promoted app. This property will be 0 unless the promoted item is an app.
- `reviews` - This property is the text that will be displayed in the tag on the top of the small cross promotion card.
- `caption` - The iTunes url of the promoted item which leads to the item's App Store page.
- `url` - The iTunes url of the promoted item which leads to the item's App Store page.
- `universal` (optional) - This property indicates if the promoted item is universal or not.

## Card Methods:

- `logCardImpression` - Manually log an impression to Braze for a particular card.
- `logCardClicked` - Manually log a click to Braze for a particular card. The SDK will only log a card click when the card has the `url` property with a valid value. All subclasses of `ABKCard` have the `url` property.

## Log Feed Display

When displaying the News Feed in your own user interface, you can manually record News Feed impressions via `- (void)logFeedDisplayed;`. For example:

```objc
[[Appboy sharedInstance] logFeedDisplayed];
```

[1]: {{ site.baseurl }}/developer_guide/platform_integration_guides/ios/in-app_messaging/#setting-delegates
[2]: {{ site.baseurl }}/developer_guide/platform_integration_guides/ios/in-app_messaging/#customizing-in-app-message-behavior-on-click
[3]: https://github.com/Appboy/appboy-ios-sdk/tree/master/Example/Stopwatch
[4]: https://github.com/Appboy/appboy-ios-sdk/blob/master/Example/Stopwatch/FeedAndFeedbackUIViewController.m
[10]: {% image_buster /assets/img_archive/UONewsFeed.png %} "Urban Outfitters News Feed"
[11]: https://github.com/Appboy/appboy-ios-sdk/tree/master/Samples/NewsFeed/BrazeNewsFeedSample
[15]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/headers/AppboyKitLibrary/Appboy.h "Appboy.h Header File"
[28]: https://github.com/Appboy/appboy-ios-sdk/blob/master/Example/Stopwatch/AppDelegate.m
[40]: {{ site.baseurl }}/help/best_practices/news_feed/
[42]: {% image_buster /assets/img_archive/badge_example.png %} "Badge Example"
[44]: http://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_feed_controller.html "abk feed controller"
[45]: {% image_buster /assets/img_archive/newsfeed_badges.png %}

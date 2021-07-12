---
nav_title: Multiple Feeds
platform: iOS
page_order: 6
description: "This reference article covers how to implement multiple Content Card feeds in your iOS application."
channel:
  - content cards


---

# Using Multiple Content Card Feeds

Content Cards can be filtered on the app to only display specific cards, which enables you to have multiple Content Card feeds for different use cases (as in having a "Transactional" feed versus a "Marketing" feed).

The following documentation demonstrates an example implementation that can be changed to fit your specific integration.

## Step 1: Setting Key-Value Pairs on Cards

When creating a Content Card campaign, key value pair data can be set on each Card. Our filtering logic will use this key-value pair data to categorize cards.

For the purposes of this example, we'll set a key-value pair with the key `feed_type` that will designate which Content Card feed the card should be displayed in. The value will be whatever your custom feeds will be, as in `Transactional`, `Marketing`, and more.

## Step 2: Set Up a Content Card Listener

Use the following code snippet to add an observer to listen for Content Card updates.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[NSNotificationCenter defaultCenter] addObserver:self
                                           selector:@selector(contentCardsUpdatedNotificationReceived:)
                                               name:ABKContentCardsProcessedNotification
                                             object:nil];
```

{% endtab %}
{% tab SWIFT %}

```swift
NotificationCenter.default.addObserver(self, selector:
  #selector(contentCardsUpdated),
  name:NSNotification.Name.ABKContentCardsProcessed, object: nil)
```

{% endtab %}
{% endtabs %}

Add the following methods to respond to updates from the observer and filter the returned cards by type.

The first method, `contentCardsUpdatedNotificationReceived:`, handles updates from the observer. It calls the second method, `getCardsForFeedType:`, with the desired feed type, in this case `Transactional`.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (void)contentCardsUpdatedNotificationReceived:(NSNotification *)notification {
  BOOL updateIsSuccessful = [notification.userInfo[ABKContentCardsProcessedIsSuccessfulKey] boolValue];
  if (updateIsSuccessful) {
    // Get an array containing only cards that have the "Transactional" feed type set in their extras.
    NSArray<ABKContentCard *> *filteredArray = [self getCardsForFeedType:@"Transactional"];
    NSLog(@"Got filtered array of length: %lu", [filteredArray count]);

    // Pass filteredArray to your UI layer for display.
  }
}

- (NSArray<ABKContentCard *> *)getCardsForFeedType:(NSString *)type {
  NSArray<ABKContentCard *> *cards = [Appboy.sharedInstance.contentCardsController getContentCards];

  NSArray<ABKContentCard *> *filteredArray = [cards filteredArrayUsingPredicate:[NSPredicate predicateWithBlock:^BOOL(ABKContentCard * card, NSDictionary *bindings) {
    NSDictionary *extras = [card extras];
    if (extras != nil && [extras objectForKey:@"feed_type"] != nil && [[extras objectForKey:@"feed_type"] isEqualToString:type]) {
      NSLog(@"Got card: %@ ", card.idString);
      return YES;
    }
    return NO;
  }]];

  return filteredArray;
}
```

{% endtab %}
{% tab SWIFT %}

```swift
@objc private func contentCardsUpdatedNotificationReceived(notification: NSNotification) {
    guard let updateSuccessful = notification.userInfo?[ABKContentCardsProcessedIsSuccessfulKey] as? Bool else { return }
    if updateSuccessful {
        // Get an array containing only cards that have the "Transactional" feed type set in their extras.
        let filteredArray = getCards(forFeedType: "Transactional")
        NSLog("Got filtered array of length: %@",filteredArray?.count ?? 0)

        // Pass filteredArray to your UI layer for display.
    }
}

func getCards(forFeedType type: String) -> [ABKContentCard]? {
    guard let allCards = Appboy.sharedInstance()?.contentCardsController.contentCards as? [ABKContentCard] else { return nil }
    // return filtered cards
    return allCards.filter {
        if $0.extras?["feed_type"] as? String == type {
            NSLog("%@","Got card: \($0.idString)")
            return true
        } else {
            return false
        }
    }
}
```

{% endtab %}
{% endtabs %}

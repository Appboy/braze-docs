---
nav_title: Multiple Feeds
page_order: 5

platform: Web
page_type: reference
description: "This article describes how to set up and use multiple content card feeds."
channel: content cards

---

# Using Multiple Content Card Feeds

Content Cards can be filtered on the app to only display specific cards, which enables you to have multiple Content Card feeds for different use cases (having a `Transactional` feed versus a `Marketing` feed).

The following documentation demonstrates an example implementation that can be changed to fit your specific integration.

## Step 1: Setting Key-Value Pairs on Cards

When creating a Content Card campaign, key value pair data can be set on each Card. Our filtering logic will use this key-value pair data to categorize cards.

For the purposes of this example, we'll set a key value pair with the key `feed_type` that will designate which Content Card feed the card should be displayed in. The value will be whatever your custom feeds will be (`Transactional`, `Marketing`, or other custom feed name).

## Step 2: Set Up Your Custom Feed

The following example will show the Content Cards feed for `Transactional` type cards:

```javascript

/**
 * @param {String} feed_type - value of the "feed_type" KVP to filter
 */
function showCardsByFeedType(feed_type) {
  appboy.display.showContentCards(null, function(cards) {
    return cards.filter(function(card) {
      return card.extras["feed_type"] === feed_type;
    });
  })
}
```

Then, you can set up a toggle for your custom feed, like the example shown below:

```javascript
// show the "Transactional" feed when this button is clicked
document.getElementById("show-transactional-feed").onclick = function() {
  showCardsByFeedType("Transactional"); 
};
```
For more information, see [our JSDocs](https://js.appboycdn.com/web-sdk/latest/doc/module-display.html#.showContentCards).

When you create a Content Card campaign, set a key-value pair as: `feed_type` > `Transactional` or based on the naming convention you choose to implement.

---
nav_title: Content Cards
platform: Cordova
page_order: 3

page_type: reference
description: "This article covers how to get started with Content Cards for Cordova."

---

# Content Cards for Cordova Integration

To get started with Content Cards, the Braze SDKs include a default card feed. To show the card feed you can use the `AppboyPlugin.launchContentCards()` method.

The default card feed included with the Braze SDK will handle all analytics tracking, dismissals, and rendering for a user's Content Cards.

## Customization

You can use these additional methods to build a custom Content Cards Feed within your app:

|Method | Description |
|---|---|
|`AppboyPlugin.requestContentCardsRefresh()`|Requests the latest Content Cards from the Braze SDK server.|
|`AppboyPlugin.getContentCardsFromServer(successCallback, errorCallback)`|Retrieves Content Cards from the Braze SDK. This will return the latest list of cards from the server.|
|`AppboyPlugin.getContentCardsFromCache(successCallback, errorCallback)`|Retrieves Content Cards from the Braze SDK. This will return the latest list of cards from the cache.|
|`AppboyPlugin.logContentCardsDisplayed()`|Logs a Content Content feed displayed event.|
|`AppboyPlugin.logContentCardClicked(cardId)`|Logs a click for the given Content Card ID.|
|`AppboyPlugin.logContentCardImpression(cardId)`|Logs an impression for the given Content Card ID.|
|`AppboyPlugin.logContentCardDismissed(cardId)`|Logs a dismissal for the given Content Card ID.|
{: .reset-td-br-1 .reset-td-br-2}

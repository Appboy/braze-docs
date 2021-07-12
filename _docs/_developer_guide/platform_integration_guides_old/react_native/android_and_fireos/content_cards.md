---
nav_title: Content Cards
platform: React Native
page_order: 1

page_type: reference
description: "This article covers how to get started with Content Cards for React Native apps."

---

# Content Cards for React Native Integration

To get started with Content Cards, the Braze SDKs include a default card feed. To show the card feed you can use the `ReactAppboy.launchContentCards()` method.

The default card feed included with the Braze SDK will handle all analytics tracking, dismissals, and rendering for a user's Content Cards.

## Customization

You can use these additional methods to build a custom Content Cards Feed within your app:

|Method | Description |
|---|---|
|`ReactAppboy.requestContentCardsRefresh()`|Requests the latest Content Cards from the Braze SDK server.|
|`ReactAppboy.getContentCards()`|Retrieves Content Cards from the Braze SDK. This will return the latest list of cards from the server.|
|`ReactAppboy.logContentCardsDisplayed()`|Logs a Content Content feed displayed event.|
|`ReactAppboy.logContentCardClicked(cardId)`|Logs a click for the given Content Card ID.|
|`ReactAppboy.logContentCardImpression(cardId)`|Logs an impression for the given Content Card ID.|
|`ReactAppboy.logContentCardDismissed(cardId)`|Logs a dismissal for the given Content Card ID.|

A sample implementation of this is contained in AppboyProject, within the [React SDK][1].

[1]: https://github.com/Appboy/appboy-react-sdk

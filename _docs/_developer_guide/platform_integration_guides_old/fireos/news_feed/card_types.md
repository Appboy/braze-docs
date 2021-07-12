---
nav_title: Card Types
page_order: 5
platform: FireOS
description: "This article covers different News Feed card types and the different card-specific properties available."
channel:
  - news feed
  
---

# Card Types
Braze has five unique News Feed card types that share a base model. Each card type also has additional card-specific properties which are listed below.

## Base Card

The [Base Card][29] model provides foundational behavior for all cards.  

- `getId()` - returns the card’s ID set by Braze
- `getViewed()` - returns a boolean reflects if the card is read or unread by the user
- `getExtras()` - returns a map of key-value extras for this card
- `setViewed(boolean)` - sets a card's viewed field
- `getCreated()` - returns the unix timestamp of the card’s creation time from Braze dashboard
- `getUpdated()` - returns the unix timestamp of the card’s latest update time from Braze dashboard
- `getCategories()` - returns the list of categories assigned to the card, cards without a category will be assigned ABKCardCategoryNoCategory
- `isInCategorySet(EnumSet)` - returns true if the card belongs to the given category set

## Banner Image Card
[Banner Image Cards][30] are clickable full-sized images. In addition to the base card properties:

- `getImageUrl()` - returns the URL of the card’s image
- `getUrl()` - returns the URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `getDomain()` - returns link text for the property url.

## Captioned Image Card
[Captioned Image Cards][31] are clickable full-sized images with accompanying descriptive text. In addition to the base card properties:

- `getImageUrl()` - returns the URL of the card’s image
- `getTitle()` - returns the title text for the card
- `getDescription()` - returns the body text for the card
- `getUrl()` - returns the URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `getDomain()` - returns the link text for the property url.

## Text Announcement Card (Captioned Image without Image)
[Text Announcement Cards][32] are clickable cards containing descriptive text. In addition to the base card properties:

- `getTitle()` - returns the title text for the card
- `getDescription()` - returns the body text for the card
- `getUrl()` - returns the URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `getDomain()` - returns the link text for the property url.

## Short News Card
[Short News Cards][33] are clickable cards with images and accompanying descriptive text.  In addition to the base card properties:

- `getImageUrl()` - returns the URL of the card’s image
- `getTitle()` - returns the title text for the card
- `getDescription()` - returns the body text for the card
- `getUrl()` - returns the URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL
- `getDomain()` - returns the link text for the property url.

[29]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html
[30]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/BannerImageCard.html
[31]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/CaptionedImageCard.html
[32]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/TextAnnouncementCard.html
[33]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/ShortNewsCard.html

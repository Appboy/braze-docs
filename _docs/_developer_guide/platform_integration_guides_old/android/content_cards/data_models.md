---
nav_title: Integration
page_order: 1
platform: Android
description: "This article covers the Content Card integration and the different data models and card-specific properties available for your Android application."
channel:
  - content cards

---

# Content Cards Integration

In Android, the Content Cards feed is implemented as a [Fragment][2] that are available in the Braze Android UI project. View [Google's documentation on Fragments][3] for information on how to add a Fragment to an Activity.

The [`AppboyContentCardsFragment`][4] class will automatically refresh and display the contents of the Content Cards and log usage analytics. The cards that can appear in a user's ContentCards are created on the Braze dashboard.

## Content Cards Data Model
The Content Cards data model is available in the Android SDK.

## Card Types {#card-types-for-android}
Braze has 3 unique Content Cards card types that share a base model. Each card type also has additional card-specific properties which are listed below.

### Base Card {#base-card-for-android}

The [Base Card][29] model provides foundational behavior for all cards.  

|Property | Description |
|---|---|
|`getId()` | Returns the card’s ID set by Braze.|
|`getViewed()` | Returns a boolean reflects if the card is read or unread by the user.|
|`getExtras()` | Returns a map of key-value extras for this card.|
|`getCreated()`  | Returns the unix timestamp of the card’s creation time from Braze.|
|`getIsPinned` | Returns a boolean that reflects whether the card is pinned.|
|`getOpenUriInWebView()`  | Returns a boolean that reflects whether Uris for this card should be opened <br> in Braze's WebView or not.|
|`getExpiredAt()` | Gets the expiration date of the card.|
|`getIsRemoved()` | Returns a boolean that reflects whether the end user has dismissed this card.|
|`getIsDismissible()`  | Returns a boolean that reflects whether the card is pinned.|
{: .reset-td-br-1 .reset-td-br-2}

### Banner Image Card {#banner-image-card-for-android}
[Banner Image Cards][30] are clickable full-sized images. In addition to the base card properties:

|Property | Description |
|---|---|
|`getImageUrl()` | Returns the URL of the card’s image.|
|`getUrl()` | Returns the URL that will be opened after the card is clicked. It can be a http(s) URL or a protocol URL.|
|`getDomain()` | Returns link text for the property URL.|
{: .reset-td-br-1 .reset-td-br-2}

### Captioned Image Card {#captioned-image-card-for-android}
[Captioned Image Cards][31] are clickable full-sized images with accompanying descriptive text. In addition to the base card properties:

|Property | Description |
|---|---|
|`getImageUrl()` | Returns the URL of the card’s image.|
|`getTitle()` | Returns the title text for the card.
|`getDescription()` | Returns the body text for the card.
|`getUrl()` | Returns the URL that will be opened after the card is clicked. It can be a http(s) URL or a protocol URL.
|`getDomain()` | Returns the link text for the property URL. 
{: .reset-td-br-1 .reset-td-br-2}

### Classic Card {#text-Announcement-card-for-android}
[Text Announcement Cards][32] are clickable cards containing descriptive text. [Short News Cards][41] are clickable cards that include text and images. In addition to the base card properties:

|Property | Description |
|---|---|
|`getTitle()` | Returns the title text for the card.
|`getDescription()` | Returns the body text for the card.
|`getUrl()` | Returns the URL that will be opened after the card is clicked. It can be a http(s) URL or a protocol URL.
|`getDomain()` | Returns the link text for the property URL.
|`getImageUrl()` | Returns the URL of the card's image, applies only to the classic Short News Card.
{: .reset-td-br-1 .reset-td-br-2}

{% alert note %}
Please note that a classic card without an image included will result in a Text Announcement Card. If an image is included, you will recieve a Short News Card.
{% endalert %}

## Card Analytics Methods
All `Card` data model objects offer the following analytics methods for logging user events to Braze servers.

|Method | Description |
|---|---|
|`logImpression()` | Manually log an impression to Braze for a particular card.
|`logClick()` | Manually log a click to Braze for a particular card. 
|`setIsDismissed()` | Manually log a dismissal to Braze for a particular card. If a card is already marked as dismissed, it cannot be marked as dismissed again.
{: .reset-td-br-1 .reset-td-br-2}

[29]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html
[30]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/BannerImageCard.html
[31]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/CaptionedImageCard.html
[32]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/TextAnnouncementCard.html
[41]: https://github.com/Appboy/android-sdk/blob/9a091979b4cbaff7f935c2cae03043a944c3ab53/android-sdk-base/src/main/java/com/appboy/models/cards/ShortNewsCard.java
[2]: http://developer.android.com/guide/components/fragments.html
[3]: http://developer.android.com/guide/components/fragments.html#Adding "Android Documentation: Fragments"
[4]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/AppboyContentCardsFragment.html

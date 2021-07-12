---
nav_title: Customization
page_order: 1

platform: Web
page_type: reference
description: "This article covers how to customize the default Content Cards style within the Braze SDK."
channel: content cards

---

# Content Cards Customization

## Content Card Data Models {#data-models}

The Braze Web SDK supports several unique Content Card card types, [ab.ClassicCard](https://js.appboycdn.com/web-sdk/latest/doc/ab.ClassicCard.html), [ab.Banner](https://js.appboycdn.com/web-sdk/latest/doc/ab.Banner.html), [ab.CaptionedImage](https://js.appboycdn.com/web-sdk/latest/doc/ab.CaptionedImage.html) which share a base model, [ab.Card](https://js.appboycdn.com/web-sdk/latest/doc/ab.Card.html).

## Customizing The Default UI

Braze UI elements come with a default look and feel that matches the composers within the Braze Dashboard and aims for consistency with other Braze mobile platforms. Braze's default styles are defined in CSS within the Braze SDK.

By overriding selected styles in your application, it is possible to customize our standard feed with your own background images, font families, styles, sizes, animations, and more. For instance, the following is an example override that will cause Content Cards to appear 800px wide:

``` css
body .ab-feed {
  width: 800px;
}
```

## Create a Custom UI

### Refreshing The Feed

To refresh and sync a user's feed with Braze servers, use the [`requestContentCardsRefresh`](https://js.appboycdn.com/web-sdk/latest/doc/modules/appboy.html#requestcontentcardsrefresh) method.

```javascript
import braze from "@braze/web-sdk"

function refresh(){
  braze.requestContentCardsRefresh();    
}
```

### Listening for card updates

When cards are refreshed, a callback function can be subscribed to:

```javascript
import braze from "@braze/web-sdk"

braze.subscribeToContentCardsUpdates(function(updates){
  const cards = updates.cards;
  // do something with the latest instance of `cards`
});
```

### Logging Analytics Events

Log impression events when cards are viewed by users.

```javascript
import braze from "@braze/web-sdk"

braze.logCardImpressions(cards, true);
```

Log card click events when users interact with a card.

```javascript
import braze from "@braze/web-sdk"

braze.logCardClick(card, true);
```


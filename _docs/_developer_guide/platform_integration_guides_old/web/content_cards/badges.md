---
nav_title: Badges
page_order: 4

platform: Web
page_type: reference
description: "This reference article describes how to request the number of unread Content Cards."
channel: content cards

---

# Requesting Unviewed Content Card Count for Web

You can request the number of unread cards at any time by calling:

``` javascript
appboy.getCachedContentCards().getUnviewedCardCount();
```

This is often used to power badges signifying how many unread content cards there are. See the [JSDocs](https://js.appboycdn.com/web-sdk/latest/doc/ab.ContentCards.html#toc4) for more information.

{% comment %}
Braze will not refresh Content Cards on new page loads (and so this function will return 0) until you show the feed or call `appboy.requestContentCardsRefresh();`.
{% endcomment %}

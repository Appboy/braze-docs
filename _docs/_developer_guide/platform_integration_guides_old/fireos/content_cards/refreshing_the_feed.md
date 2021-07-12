---
nav_title: Refreshing the Feed
page_order: 3
platform: FireOS
description: "This reference article covers how to implement Content Card refreshing in your Android application."
channel:
  - content cards

---

# Refreshing Content Cards

You can queue a manual refresh of Braze Content Cards at any time by calling:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).requestContentCardsRefresh(false);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).requestContentCardsRefresh(false)
```

{% endtab %}
{% endtabs %}

For more information on this method, please see [our corresponding Javadocs](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#requestContentCardsRefresh-boolean-).

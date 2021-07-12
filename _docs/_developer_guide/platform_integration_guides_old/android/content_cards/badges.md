---
nav_title: Badges
page_order: 5
platform: Android
description: "This article covers how to add badges to your Content Cards in your Android application."
channel:
  - content cards

---

# Adding a Badge

You can [request the number of unread cards][1] at any time by calling:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getContentCardUnviewedCount();
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).contentCardUnviewedCount
```

{% endtab %}
{% endtabs %}

[1]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#getContentCardUnviewedCount--

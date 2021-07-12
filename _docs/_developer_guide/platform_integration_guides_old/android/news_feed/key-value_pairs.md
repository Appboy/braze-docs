---
nav_title: Key-Value Pairs
page_order: 8
platform: Android
description: "This reference article covers how to use key-value pairs in your News Feed for your Android application."
channel:
  - news feed

---

# Key-Value Pairs

`Card` objects may optionally carry key-value pairs as `extras`. These can be used to send data down along with a `Card` for further handling by the application.

Call the following on a `Card` object to retrieve its extras:

{% tabs %}
{% tab JAVA %}

```java
Map<String, String> getExtras()
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
extras: Map<String, String>
```

{% endtab %}
{% endtabs %}

See the [Javadoc][36] for more information.

[36]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#getExtras()

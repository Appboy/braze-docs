---
nav_title: Google Advertising ID (Optional)
page_order: 9
platform: Android
description: "This article covers Google advertising IDs and how to pass this advertising information to Braze."

---

# Optional Google Advertising ID

The Google Advertising ID is a user-specific, unique, resettable ID for advertising, provided by Google Play services. It gives users better controls and provides developers with a simple, standard system to continue to monetize their apps. It is an anonymous identifier for advertising purposes and enables users to reset their identifier or opt-out of interest-based ads within Google Play apps. See [here][2] for more information.

## Passing the Google Advertising ID to Braze

The Google Advertising ID is not automatically collected by the Braze SDK and must be set manually via the [`Appboy.setGoogleAdvertisingId()`][1] method.

{% alert important %}
Google requires the Advertising ID to be collected on a non-UI thread.
{% endalert %}

{% tabs %}
{% tab JAVA %}

```java
new Thread(new Runnable() {
  @Override
  public void run() {
    try {
      AdvertisingIdClient.Info idInfo = AdvertisingIdClient.getAdvertisingIdInfo(getApplicationContext());
      Appboy.getInstance(getApplicationContext()).setGoogleAdvertisingId(idInfo.getId(), idInfo.isLimitAdTrackingEnabled());
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}).start();
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Thread(Runnable {
  try {
    val idInfo = AdvertisingIdClient.getAdvertisingIdInfo(getApplicationContext())
    Appboy.getInstance(getApplicationContext()).setGoogleAdvertisingId(idInfo.id, idInfo.isLimitAdTrackingEnabled)
  } catch (e: Exception) {
    e.printStackTrace()
  }
}).start()
```

{% endtab %}
{% endtabs %}


[1]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#setGoogleAdvertisingId-java.lang.String-boolean-
[2]: https://support.google.com/googleplay/android-developer/answer/6048248/advertising-id?hl=en

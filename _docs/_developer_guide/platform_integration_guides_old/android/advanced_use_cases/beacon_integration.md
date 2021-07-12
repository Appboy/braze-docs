---
nav_title: Beacon Integration
platform: Android
page_order: 2
description: "This article covers how to log custom events using Gimbal Beacons for Android."

---

# Beacon Integration

Here we will walk through how to integrate specific kinds of beacons with Braze to allow for segmentation and messaging.

## Gimbal Beacons

Once you have your Gimbal Beacons set up and integrated into your app, you can log custom events for things like a visit starting or ending, or a beacon being sighted. You can also log properties for these events, like the Place name or the Dwell time.

To log a custom event when a user enters a place, input this code into the `onVisitStart` method:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).logCustomEvent("Entered " + visit.getPlace());
Appboy.getInstance(context).requestImmediateDataFlush();
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).logCustomEvent("Entered " + visit.getPlace())
Appboy.getInstance(context).requestImmediateDataFlush()
```

{% endtab %}
{% endtabs %}

The `requestImmediateDataFlush` ensures that your event will log even if the app is in the background, and the same process can be implemented for leaving a location. Please note that the Activity and Context that you are working in may change exactly how you integrate the `logCustomEvent` and `requestImmediateDataFlush` lines. Also, note that the above will create and increment a unique custom event for each new place that the user enters. As such, if you anticipate creating more than 50 places we recommend you create one generic "Place Entered" custom event and include the place name as an event property.

---
nav_title: Location Tracking
platform: Web
page_order: 5

page_type: reference
description: "This article covers how to enable location tracking via the Braze Web SDK."

---

# Location Tracking

To set a user's current location, use the [`getCurrentPosition()`][0] method of the Geolocation API and log the location data to Braze.

```javascript
function success(position) {
  var coords = position.coords;
  appboy.getUser().setLastKnownLocation(
    coords.latitude,
    coords.longitude,
    coords.accuracy,
    coords.altitude,
    coords.altitudeAccuracy
  );
}

navigator.geolocation.getCurrentPosition(success);
```

Note that calling `navigator.geolocation.getCurrentPosition()` will immediately request permission from the user unless they have already granted or denied permission. See the [JSDocs][1] for more information on setting the user's last known location.


## Logging A Single Location

Additionally, when the Web SDK sends data to Braze servers, the user's country will be automatically detected from their IP Address if it has not been manually set by your application.

### Continuous Tracking

If you'd like to continuously track a user's location during a page load, use the `watchPosition()` method of the Geolocation API. This method will invoke the success callback each time the user's location is updated.

```javascript
function success(position) {
  var coords = position.coords;
  appboy.getUser().setLastKnownLocation(
    coords.latitude,
    coords.longitude,
    coords.accuracy,
    coords.altitude,
    coords.altitudeAccuracy
  );
}

navigator.geolocation.watchPosition(success);
```

Calling `navigator.geolocation.watchPosition()` will immediately request permission from the user unless they have already granted or denied permission. For information about configuring and stopping the location tracking, see the [Mozilla developer docs][2].

[0]: https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/getCurrentPosition
[1]: https://js.appboycdn.com/web-sdk/latest/doc/ab.User.html#setLastKnownLocation
[2]: https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/watchPosition

---
nav_title: Read & Unread Indicators
page_order: 4
platform: FireOS
description: "This reference article covers how to implement read and unread indicators in your News Feed for your Android application."
channel:
  - news feed

---

# Read & Unread Indicators

Braze allows you to optionally toggle on an Unread/Read indicator on News Feed cards as pictured below:

![UnreadvsRead][25]

## Enabling the Indicators

In order to enable this functionality add the following line to your `braze.xml` file:

```xml
<bool name="com_appboy_newsfeed_unread_visual_indicator_on">true</bool>
```

## Customizing the Indicators
These indicators can be customized by altering the values in [android-sdk-ui/src/main/res/drawable-hdpi/icon_unread.png][26] and [android-sdk-ui/src/main/res/drawable-hdpi/icon_read.png][27].

[25]: {% image_buster /assets/img_archive/UnreadvsReadNewsFeedCard.png %}
[26]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/res/drawable-hdpi/icon_unread.png
[27]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/res/drawable-hdpi/icon_read.png

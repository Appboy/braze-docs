---
nav_title: Customization
platform: iOS
page_order: 5
description: "This reference article covers how to customize your News Feed in your iOS  application."
channel:
  - news feed

---

# Customization

{% alert important %}
__Note that integration of `SDWebImage` is required if you plan on using our Braze UI for displaying images__ within iOS In-App Messages, News Feed, or Content Cards.
{% endalert %}

## Overriding Default Images

Braze allows clients to replace existing default images with their own custom images. To accomplish this, create a new `png` file with the custom image and add it to the app’s image bundle. Then, rename the file with the image’s name (see below) to override the default image in our library. Images available for an override in the News Feed include:
* Read icon indicator: `Icons_Read`
* Placeholder image: `img-noimage-lrg`

{% alert note %} Be sure to upload the `@2x` and `@3x` versions of the images as well to accommodate different phone sizes. {% endalert %}

{% alert note %} Note that overriding default images is currently not supported in our Xamarin iOS integration. {% endalert %}

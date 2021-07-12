---
nav_title: Rich Notifications
platform: iOS
page_order: 3
description: "This article covers how to implement rich push notifications in your iOS application."
channel:
  - push

---

# iOS 10 Rich Notifications

iOS 10 introduces the ability to send push notifications with images, gifs, and video. To enable this functionality, clients must create a `Service Extension`, a new type of extension that enables modification of a push payload before it is displayed.

## Creating A Service Extension
To create a [Notification Service Extension][23], navigate to `File > New > Target` and select `Notification Service Extension`.

![Adding a Service Extension][26]

Ensure that `Embed In Application` is set to embed the extension in your application.

## Setting Up The Service Extension
A `Notification Service Extension` is its own binary that is bundled with your app. As such, it must be set up in the [Apple Developer Portal][27] with its own App ID and Provisioning Profile.

### Configuring The Service Extension To Work With Braze
Braze sends down an attachment payload in the APNs payload under the `ab` key that we use to configure, download and display rich content:

For example:

```json
{
  "ab" :
    {
    ...

    "att" :
      {
       "url" : "http://mysite.com/myimage.jpg",
       "type" : "jpg"
       }
    },
  "aps" :
    {
    ...
    }
}
```

The relevant payload values are:

```objc
// The Braze dictionary key
static NSString *const AppboyAPNSDictionaryKey = @"ab";

// The attachment dictionary
static NSString *const AppboyAPNSDictionaryAttachmentKey = @"att";

// The attachment URL
static NSString *const AppboyAPNSDictionaryAttachmentURLKey = @"url";

// The type of the attachment - a suffix for the file you save
static NSString *const AppboyAPNSDictionaryAttachmentTypeKey = @"type";
```

To manually display push with a Braze payload, download the content from the value under `AppboyAPNSDictionaryAttachmentURLKey`, save it as a file with the file type stored under the `AppboyAPNSDictionaryAttachmentTypeKey` key, and add it to the notification attachments.
We provide sample code that you can copy into your `Notification Service Extension`. Simply changing the class name to the one you picked will automatically provide this functionality.

You can write the Service Extension in either Objective-C or Swift.

## Creating A Rich Notification In Your Dashboard

To create a rich notification in your Braze dashboard, simple create an iOS push and attach an image or gif, or provide a url that hosts an image, gif, or video.  Note that assets are downloaded on the receipt of push notifications, so that if you are hosting your own content you should plan for large, synchronous spikes in requests.

Also note the supported file types and sizes, listed [here][28].


[23]: https://developer.apple.com/reference/usernotifications/unnotificationserviceextension
[26]: {% image_buster /assets/img_archive/ios10_se_at.png %}
[27]: https://developer.apple.com
[28]: https://developer.apple.com/reference/usernotifications/unnotificationattachment

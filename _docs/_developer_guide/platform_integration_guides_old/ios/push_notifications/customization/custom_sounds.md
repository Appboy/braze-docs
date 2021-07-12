---
nav_title: Custom Sounds
platform: iOS
page_order: 2
description: "This article covers how to implement custom sounds in your iOS push notifications."
channel:
  - push

---

# Custom Sounds

## Step 1: Hosting the Sound in the App

Custom push notification sounds must be hosted locally within the main bundle of the client application. The following audio data formats are accepted:

- Linear PCM
- MA4
- µLaw
- aLaw

You can package the audio data in an aiff, wav, or caf file. Then, in Xcode, add the sound file to your project as a nonlocalized resource of the application bundle.

You may use the afconvert tool to convert sounds. For example, to convert the 16-bit linear PCM system sound Submarine.aiff to IMA4 audio in a CAF file, use the following command in the Terminal application:

```bash
afconvert /System/Library/Sounds/Submarine.aiff ~/Desktop/sub.caf -d ima4 -f caff -v
```

You can inspect a sound to determine its data format by opening it in QuickTime Player and choosing Show Movie Inspector from the Movie menu.

Custom sounds must be under 30 seconds when played. If a custom sound is over that limit, the default system sound is played instead.

## Step 2: Providing the Dashboard with a Protocol URL for the Sound

Your sound must be hosted locally within the app. You must specify a Protocol URL that directs to the location of the sound file in the app within the "Sound" field in the push composer. Specifying “default” in this field will play the default notification sound on the device. This can be specified via our [Messaging API][25] or our dashboard under “Advanced Settings” in the push composer wizard as pictured below:

![Push Notification Sound][8]

If the specified sound file doesn’t exist or the keyword ‘default’ is entered, Braze will use the default device alert sound. Aside from our dashboard, sound can also be configured via our our [Messaging API][12]. For additional information see the Apple Developer Documentation regarding ["Preparing Custom Alert Sounds"][9].

[8]: {% image_buster /assets/img_archive/sound_push_ios.png %}
[9]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SupportingNotificationsinYourApp.html
[12]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[25]: {{site.baseurl}}/developer_guide/rest_api/messaging/#messaging

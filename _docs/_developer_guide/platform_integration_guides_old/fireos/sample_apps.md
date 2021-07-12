---
nav_title: Sample Apps
platform: FireOS
page_order: 7

page_type: reference
description: "This page describes the sample apps provided within Braze's SDK repository."

---

# Sample Apps

Braze's SDKs each come with a sample application within the repository for your convenience. Each of these apps is fully buildable so you can test Braze features alongside implementing them within your own applications. Testing behavior within your own application versus expected behavior and codepaths within the sample applications is an excellent way to debug any problems you may run into.

## Building the Droidboy Test Application
Braze's test application within the [Android SDK github repository][3] is called Droidboy. Follow the instructions below to build a fully functional copy of it alongside your project.

- Create a new ["App Group"][25] and note the Braze API key.
- Copy your FCM Sender ID and Braze API key into the appropriate places within `/droidboy/res/values/braze.xml` (in between the tags for the strings named "com_appboy_push_fcm_sender_id" and "com_appboy_api_key", respectively).
- Copy your FCM API Key into your **Settings** page.
- To assemble the Droidboy APK run the following command within the SDK directory:

```
./gradlew assemble
```
> Use `gradlew.bat` on Windows

- To automatically install the Droidboy APK on a test device run the following command within the SDK directory:

```
./gradlew installDebug
```

## Building the Hello Appboy Test Application
The Hello Appboy test application shows a minimal use case of the Braze SDK, and additionally shows how to easily integrate the Braze SDK into a gradle project.

1. Copy your API key from your **Settings** page into your `braze.xml` file in the `res/values` folder.
![HelloBraze][34]

2. To install the sample app to a device or emulator, run the following command within the SDK directory:

```
./gradlew installDebug
```

> If you don't have your `ANDROID_HOME` variable properly set or don't have a `local.properties` folder with a valid `sdk.dir` folder, this plugin will also install the base SDK for you. See the [plugin repo][27] for more information.

For more information on the Android SDK build system see the [Github Repository readme][26].

[25]: {{site.baseurl}}/developer_guide/platform_wide/app_group_configuration/#app-group-configuration
[26]: https://github.com/Appboy/appboy-android-sdk/blob/master/README.md
[27]: https://github.com/JakeWharton/sdk-manager-plugin
[3]: https://github.com/appboy/appboy-android-sdk "Appboy Android Github Repository"
[34]: {% image_buster /assets/img_archive/hello_appboy.png %}

---
nav_title: Manual Integration Options
platform: iOS
page_order: 2
description: "This reference article shows how to manually integrate the Braze SDK for iOS."

---

# Manual Integration

{% alert tip %}
We strongly recommend that you implement the SDK via a package manager such as [Swift Package Manager](../swift_package_manager/), [CocoaPods](../cocoapods/), or [Carthage](../carthage_integration/). It will save you a lot of time and automate much of the process for you. However, if you are unable to do so you may complete the integration manually by following the instructions below.
{% endalert %}

## Step 1: Downloading the Braze SDK

1. Download `Appboy_iOS_SDK.zip` from the [release page](https://github.com/appboy/appboy-ios-sdk/releases).
If integrating an SDK version before 3.24.0, instead clone the Braze iOS SDK Github project:

```bash
# This command will clone both versions of the Braze SDK
$ git clone git@github.com:Appboy/appboy-ios-sdk.git
```

2. In Xcode, from the project navigator, select the destination project or group for Braze
3. Navigate to File > Add Files to “Project_Name”
4. Add the `AppboyKit` and `AppboyUI` folders to your project as a group.
	- Make sure that the "Copy items into destination group’s folder" option is checked if you are integrating for the first time. Expand "Options" in the file picker to select "Copy items if needed" and "Create groups."
	- Delete the `AppboyKit/include` and `AppboyUI/include` directories
5. (Optional) If you are one of the following:
  - You only want the core analytics features of the SDK and do not use any UI features (e.g, In-App Messages or Content Cards)
  - You have custom UI for Braze's UI features and handle the image downloading yourself

	You can use the core version of the SDK by removing the file `ABKSDWebImageProxy.m` and `Appboy.bundle`. This will remove the SDWebImage framework dependency and all the UI related resources (e.g. Nib files, images, localization files) from the SDK.

{% alert warning %}
If you try to use the core version of the SDK without Braze's UI features, in-app messages will not display. Trying to display Braze's Content Cards UI with the core version will lead to unpredictable behavior.
{% endalert %}

## Step 2: Adding Required iOS Libraries

1. Click on the target for your project (using the left-side navigation), and select the “Build Phases” tab
2. Click the <i class="fas fa-plus"></i> button under “Link Binary With Libraries”
3. In the menu, select `SystemConfiguration.framework`
4. Mark this library as required using the pull-down menu next to `SystemConfiguration.framework`
5. Repeat to add each of the following required frameworks to your project, marking each as “required”
	- `QuartzCore.framework`
	- `libz.tbd`
	- `CoreImage.framework`
	- `CoreText.framework`
	- `WebKit.framework`
6. Add the following frameworks and mark them as optional:
	- `CoreTelephony.framework`
7. Select the "Build Settings" tab. In the "Linking" section, locate the "Other Linker Flags" setting and add the `-ObjC` flag.
8. The SDWebImage framework is required for the Braze News Feed, Content Cards and In-App Messaging to function properly. SDWebImage is used for image downloading and displaying, including GIFs. If you intend to use the News Feed, Content Cards or In-App Messages, please follow the steps below.

### SDWebImage Integration

1. Follow the SDWebImage Installation Guide's [instructions](https://github.com/SDWebImage/SDWebImage/wiki/Installation-Guide#using-sdwebimage-as-sub-xcode-project) for manually using SDWebImage as a Sub-Project in Xcode.
2. In the `SDWebImage` project settings, open the "Build Settings" tab. In the "Linking" section, locate the "Other Linker Flags" setting and add the `-ObjC` flag if it isn't already present.

### Optional Location Tracking

1. Add the `CoreLocation.framework` to enable location tracking
2. You must authorize location for your users using `CLLocationManager` in your app

## Step 3: Updating your App Delegate

{% tabs %}
{% tab OBJECTIVE-C %}

Add the following line of code to your `AppDelegate.m` file:

```objc
#import "AppboyKit.h"
```

Within your `AppDelegate.m` file, add the following snippet within your `application:didFinishLaunchingWithOptions` method:

{% alert important %}
Be sure to update `YOUR-API-KEY` with the correct value from your **Settings** page.
{% endalert %}


```objc
[Appboy startWithApiKey:@"YOUR-API-KEY"
         inApplication:application
     withLaunchOptions:launchOptions];
```

{% alert important %}
Be sure to initialize Braze in your application's main thread.
{% endalert %}

{% endtab %}
{% tab swift %}

If you do not have a bridging header file, create one and name it `your-product-module-name-Bridging-Header.h` by choosing File > New > File > (iOS or OS X) > Source > Header File. Then add the following line of code to the top of your bridging header file:
```
#import "AppboyKit.h"
```

In your project's Build Settings, add the relative path of your header file to the `Objective-C Bridging Header` build setting under `Swift Compiler - Code Generation`.

For more information about using Objective-C code in Swift projects, please refer to the [Apple Developer Docs](https://developer.apple.com/library/ios/documentation/swift/conceptual/buildingcocoaapps/MixandMatch.html).

In `AppDelegate.swift`, add following snippet within function `application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool`:

```
Appboy.start(withApiKey: "YOUR-API-KEY",
                 in:application,
                 withLaunchOptions:launchOptions,
                 withAppboyOptions:appboyOptions)
```

{% endtab %}
{% endtabs %}

## SDK Integration Complete {#manual-sdk-integ}

Braze should now be collecting data from your application and your basic integration should be complete. Please see the following sections in order to enable custom event tracking, push messaging, the news-feed and the complete suite of Braze features.

[Full iOS class documentation][7] is available to provide additional guidance on any of the aforementioned methods.

[7]: http://appboy.github.io/appboy-ios-sdk/docs/annotated.html "full iOS class documentation"

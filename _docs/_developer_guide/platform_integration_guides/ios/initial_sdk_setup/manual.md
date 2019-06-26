---
nav_title: Add Braze Manually
platform: iOS
page_order: 2
search_rank: 5
---

# Manual Integration Options

{% alert tip %}
We strongly recommend that you implement the SDK via a [CocoaPod](http://cocoapods.org/). It will save you a lot of time and automate much of the process for you. However, if you are unable to do so you may complete integration manually without CocoaPods by using our manual integration instructions below.
{% endalert %}

## Manual SDK Integration

### Step 1: Cloning the Braze SDK

1. Clone the Braze iOS SDK Github project:

```bash
# This command will clone both versions of the Braze SDK
$ git clone git@github.com:Appboy/appboy-ios-sdk.git
```

2. In Xcode, from the project navigator, select the destination project or group for Braze
3. Navigate to File > Add Files to “Project_Name”
4. Add the `AppboyKit` and `AppboyUI` folders to your project as a group.
	- Make sure that the "Copy items into destination group’s folder" option is checked if you are integrating for the first time. In Xcode 7+, expand "Options" in the file picker to select "Copy items if needed" and "Create groups."
5. (Optional) If you are one of the following:
  - You only want the core analytics features of the SDK and do not use any UI features (e.g, In-App Messages, News Feed, or Feedback)
  - You have custom UI for Braze's UI features and handle the image downloading yourself

	You can use the core version of the SDK by removing the file `ABKSDWebImageProxy.m` and `Appboy.bundle`. This will remove the SDWebImage framework dependency and all the UI related resources (e.g. Nib files, images, localization files) from the SDK.

{% alert warning %}
If you try to use the core version of the SDK without Braze's UI features, in-app messages will not display. Trying to display Braze's News Feed and Feedback UI with core version will lead to unpredictable behavior.
{% endalert %}

### Step 2: Adding Required iOS Libraries

1. Click on the target for your project (using the left-side navigation), and select the “Build Phases” tab
2. Click the <i class="fas fa-plus"></i> button under “Link Binary With Libraries”
3. In the menu, select `SystemConfiguration.framework`
4. Mark this library as required using the pull-down menu next to `SystemConfiguration.framework`
5. Repeat to add each of the following required frameworks to your project, marking each as “required”
	- `QuartzCore.framework`
	- `libz.dylib`, or `libz.tbd` in Xcode 7+
	- `CoreImage.framework`
	- `CoreText.framework`
	- `WebKit.framework`
6. Add the following frameworks and mark them as optional:
	- `CoreTelephony.framework`
	- `Social.framework`
	- `Accounts.framework`
	- `AdSupport.framework`
	- `StoreKit.framework`
7. The SDWebImage framework is required for the Braze News Feed, Content Cards and In-App Messaging to function properly. SDWebImage is used for image downloading and displaying, including GIFs. If you intend to use the News Feed, Content Cards or In-App Messages, please follow the steps below.

{% alert warning %}  
From version 2.26.0, Braze iOS SDK only supports 4.x version of SDWebImage. If you have to use SDWebImage version 3.x, please use Braze SDK version 2.25.0 or below.
{% endalert %}

#### SDWebImage Integration

1. Inside of your project folder, clone SDWebImage repository recursively:
```
git clone --recursive https://github.com/rs/SDWebImage.git
```
2. Drag-n-drop `SDWebImage/SDWebImage.xcodeproj` into your application Xcode project.
3. In your project application’s target settings, open the "General" tab, click the "+" button under the "Link Frameworks and Libraries" block and add `ImageIO.framework`.
4. In your project application’s target settings, open the "General" tab, click the "+" button under the "Embedded Binaries" block and add `SDWebImage.framework`.
5. In the `SDWebImage` project settings, open the "Build Settings" tab, in the "Linking" section, locate the "Other Linker Flags" setting and add the `-ObjC` flag.
6. In your project application's target settings, open the "Build Settings", in the "Search Paths" section, locate "Header Search Paths" and add `$(SRCROOT)/SDWebImage` with "recursive" turned on.

#### Optional Location Tracking

1. Add the `CoreLocation.framework` to enable location tracking
2. You must authorize location for your users using `CLLocationManager` in your app

### Step 3: Updating your App Delegate

{% tabs %}
{% tab OBJECTIVE-C %}

Add the following line of code to your `AppDelegate.m` file:

```objc
#import "AppboyKit.h"
```

Within your `AppDelegate.m` file, add the following snippet within your `application:didFinishLaunchingWithOptions` method:

{% alert important %}
Be sure to update `YOUR-API-KEY` with the correct value from your App Settings page.
{% endalert %}


```objc
[Appboy startWithApiKey:@"YOUR-API-KEY"
         inApplication:application
     withLaunchOptions:launchOptions];
```

{% alert important %}
Be sure to initialize Braze in your application's main thread.
{% endalert %}

See the [`AppDelegate.m` file](https://github.com/Appboy/appboy-ios-sdk/blob/master/Example/Stopwatch/AppDelegate.m) in the Stopwatch sample app.

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
Appboy.startWithApiKey("YOUR-API-KEY", inApplication:application, withLaunchOptions:launchOptions)
```

{% endtab %}
{% endtabs %}

### SDK Integration Complete {#manual-sdk-integ}

Braze should now be collecting data from your application and your basic integration should be complete. Please see the following sections in order to enable custom event tracking, push messaging, the news-feed and the complete suite of Braze features.

[Full iOS class documentation][7] is available to provide additional guidance on any of the aforementioned methods.

[1]: #clone-sdk
[2]: #add-libs
[3]: #configure
[4]: #update-delegate
[7]: http://appboy.github.io/appboy-ios-sdk/docs/annotated.html "full ios class documentation"
[10]: http://cocoapods.org/

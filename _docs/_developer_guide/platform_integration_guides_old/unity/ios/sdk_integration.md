---
nav_title: SDK Integration
platform: Unity
subplatform: iOS
page_order: 0
description: "This reference article covers the iOS SDK integration for the Unity platform."

---

# SDK Integration

Follow the below instructions to get Braze running in your Unity application. If you are transitioning from a manual integration, please read the instructions on [Transitioning From a Manual to an Automated Integration][5].

## Step 1: Choose your Braze Unity Package

The Braze [`.unitypackage`][41] bundles native bindings for the Android and iOS platforms, along with a C# interface.

There are several Braze Unity packages available for download at [Braze Unity Releases Page][42]:
 
{% include archive/unity/unitypackage_descriptions.md%}

## Step 2: Import the Package

1. In the Unity Editor, import the package into your Unity project by navigating to `Assets > Import Package > Custom Package`.
2. Click "Import".

Alternatively, follow the Unity instructions for [Importing Asset packages][41] for a more detailed guide on importing custom Unity packages. 

{% alert note %}
If you only wish to import the iOS plugin, deselect the `Plugins/Android` subdirectory when importing the Braze `.unitypackage`.
{% endalert %}

## Step 3: Set Your API Key

Braze provides a native Unity solution for automating the Unity iOS integration. This solution modifies the built Xcode project using Unity's [`PostProcessBuildAttribute`][6] and subclasses the UnityAppController using the `IMPL_APP_CONTROLLER_SUBCLASS` macro.

1. In the Unity Editor, open the Braze Configuration Settings by navigating to Braze > Braze Configuration.
2. Check the "Automate Unity iOS Integration" box.
3. In the "Braze API Key" field, input your application's API key from the [Braze Dashboard][19]. Your Braze Configuration settings should look like this:

![Braze Config Editor][18]

>  If your application is already using another `UnityAppController` subclass, you will need to merge your subclass implementation with `AppboyAppDelegate.mm`.

## Step 4: Basic SDK Integration Complete

Braze should now be collecting data from your application and your basic integration should be complete.

## Push

See the [Push documentation][50] for information on integrating push.

## In-App Messages

See the [In-App Message documentation][34] for information on integrating in-app messages.

## Content Cards Integration

See the [Content Cards documentation][40] for information on integrating Content Cards.

## News Feed Integration

See the [News Feed documentation][35] for information on integrating the News Feed.

## Extending the SDK

To extend the SDK's behaviors, fork our [Braze Unity SDK Github project][1] and make your required changes.

To publish your modified code as a Unity package, see [Advanced Use Cases][51].

## Transitioning from Manual to Automated Integration

To take advantage of the automated iOS integration offered in the Braze Unity SDK, follow these steps on transitioning from a manual to an automated integration.

1. Remove all Braze-related code from your Xcode project's `UnityAppController` subclass.
2. Remove Braze's iOS libraries from your Unity or Xcode project (i.e., `Appboy_iOS_SDK.framework` and `SDWebImage.framework`) and [import the Braze Unity package][7] into your Unity project.
3. Follow the integration instructions on [setting your API key through Unity][8].

[1]: https://github.com/appboy/appboy-unity-sdk
[5]: #transitioning-from-manual-to-automated-integration
[6]: http://docs.unity3d.com/ScriptReference/Callbacks.PostProcessBuildAttribute.html
[7]: #step-1-importing-the-braze-unity-package
[8]: #step-2-setting-your-api-key
[18]: {% image_buster /assets/img_archive/unity-ios-appboyconfig.png %}
[19]: https://dashboard-01.braze.com/app_settings/app_settings
[34]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/in-app_messaging/
[35]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/news_feed/
[40]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/content_cards/
[41]: https://docs.unity3d.com/Manual/AssetPackages.html
[42]: https://github.com/Appboy/appboy-unity-sdk/releases
[50]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/ios/push_notifications/
[51]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/Advanced_Use_Cases/advanced_use_cases

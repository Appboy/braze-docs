---
nav_title: Initial SDK Setup
platform: Xamarin
subplatform: iOS
page_order: 0
description: "This article covers the inital iOS SDK setup for the Xamarin platform."

---

# Initial SDK Setup

Installing the Braze SDK will provide you with basic analytics functionality as well as working in-app messages with which you can engage your users.

## Step 1: Get the Xamarin binding

A Xamarin binding is a way to use native libraries in Xamarin apps.  The implementation of a binding consists of building a C# interface to the library and then using that interface in your application.

There are two ways to include the Braze SDK binding.

### Option 1: Nuget

The simplest integration method involves getting the Braze SDK Bindings from the [Nuget.org][9] central repository. In the Visual Studio sidebar, right-click `Packages` folder and click `Add Packages...`.  Search for 'Braze' and install the [`AppboyPlatformXamariniOSBinding`][11] package into your project.

### Option 2: Source

The second integration method is to include the binding source found [here][3].  In [our github repo][7] you will find our binding source code; adding a project reference to the ```AppboyPlatformXamariniOSBinding.csproj``` in your Xamarin application will cause the binding to be built with your project and provide you access to the Braze iOS SDK. Please make sure `AppboyPlatformXamariniOSBinding` is showing in your project's "Reference" folder.

## Step 2: Update your App Delegate and Declare Xamarin Usage

Within your `AppDelegate.cs` file, add the following snippet within your `FinishedLaunching` method:

>  Be sure to update `YOUR-API-KEY` with the correct value from your [Settings][5] page.

```csharp
// C#
 Appboy.StartWithApiKey ("YOUR-API-KEY", UIApplication.SharedApplication, options);
 Appboy.SharedInstance.SdkFlavor = ABKSDKFlavor.Xamarin;
```

**Implementation Example**

See the `AppDelegate.cs` file in the [TestApp.XamariniOS][10] sample app.

## Step 3: Add your SDK endpoint to your Info.plist file

Within your `Info.plist` file, add the following snippet:

>  Be sure to update `YOUR-SDK-ENDPOINT` with the correct value from your [Settings][5] page.

```csharp
// C#
<key>Braze</key>
<dict>
 <key>Endpoint</key>
 <string>YOUR-SDK-ENDPOINT</string>
</dict>
```

You can optionally include verbose logging by including the following snippet:

```csharp
// C#
<key>Braze</key>
<dict>
 <key>LogLevel</key>
 <string>0</string>
 <key>Endpoint</key>
 <string>YOUR-SDK-ENDPOINT</string>
</dict>
```

Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

## SDK Integration Complete

Braze should now be collecting data from your application and your basic integration should be complete. Please see the following sections in order to enable custom event tracking, push messaging, the news-feed and the complete suite of Braze features.

>  Our current public Xamarin binding for the iOS SDK does not connect to the iOS Facebook SDK (linking social data) and does not include sending the IDFA to Braze.

[3]: https://github.com/Appboy/appboy-xamarin-bindings
[5]: https://dashboard-01.braze.com/app_settings/app_settings/ "Settings"
[7]: https://github.com/Appboy/appboy-xamarin-bindings/tree/master/appboy-component/src/ios-unified
[9]: https://www.nuget.org/
[10]: https://github.com/Appboy/appboy-xamarin-bindings/tree/master/appboy-component/samples/ios-unified/TestApp.XamariniOS
[11]: https://www.nuget.org/packages/AppboyPlatformXamariniOSBinding/

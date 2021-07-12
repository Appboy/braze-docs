---
nav_title: Initial SDK Setup
platform: Xamarin
subplatform: Android and FireOS
page_order: 0
description: "This article covers the inital Android and FireOS SDK setup for the Xamarin platform."

---

# Initial SDK Setup

Installing the Braze SDK will provide you with basic analytics functionality as well as working in-app messages with which you can engage your users.

## Step 1: Get the Xamarin binding

A Xamarin binding is a way to use native libraries in Xamarin apps. The implementation of a binding consists of building a C# interface to the library, and then using that interface in your application.  See [the Xamarin documentation][2].

There are two ways to include the Braze SDK binding.

### Option 1: Nuget

The simplest integration method involves getting the Braze SDK Bindings from the [Nuget.org][9] central repository. In the Visual Studio sidebar, right click `Packages` folder and click `Add Packages...`.  Search for 'Braze' and install the [`AppboyPlatform.AndroidBinding`][13] package into your project.

### Option 2: Source

The second integration method is to include the binding source found [here][3].  Under `appboy-component\src\android` you will find our binding source code; adding a project reference to the ```AppboyPlatform.XamarinAndroidBinding.csproj``` in your Xamarin application will cause the binding to be built with your project and provide you access to the Braze Android SDK.

>  The Braze Nuget package depends on the [`Xamarin.Android.Support.v4`][12] Nuget package.

## Step 2: Configure the Braze SDK in braze.xml
Now that the libraries have been integrated, you have to create an `braze.xml` file in your project's `Resources/values` folder. The contents of that file should resemble the following code snippet:

>  Be sure to substitute `REPLACE_WITH_YOUR_API_KEY` with the API key located the [Settings][4] page of the Braze dashboard.

```java
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
    <string name="com_appboy_api_key">REPLACE_WITH_YOUR_API_KEY</string>
    <string translatable="false" name="com_appboy_custom_endpoint">YOUR_CUSTOM_ENDPOINT_OR_CLUSTER</string>
    </resources>
```

## Step 3: Add Required Permissions to Android Manifest
Now that you've added your API key, you need to add the following permissions to your `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

## Step 4: Tracking User Sessions and Registering for In-App Messages
To enable user session tracking and register your app for in-app messages, add the following call to the `OnCreate()` lifecycle method of the `Application` class in your app:

```csharp
RegisterActivityLifecycleCallbacks(new AppboyLifecycleCallbackListener());
```

## SDK Integration Complete

You should now be able to launch your application and see sessions being logged to the Braze dashboard (along with device information and other analytics).  

> Consult the [Android integration instructions][8] for a more in-depth discussion of best practices for the basic SDK integration.

[2]: http://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/binding_a_java_library_%28.jar%29/
[3]: https://github.com/Appboy/appboy-xamarin-bindings
[4]: https://dashboard-01.braze.com/app_settings/app_settings/ "Settings"
[8]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/
[9]: https://www.nuget.org/
[12]: https://www.nuget.org/packages/Xamarin.Android.Support.v4/
[13]: https://www.nuget.org/packages/AppboyPlatform.AndroidBinding/

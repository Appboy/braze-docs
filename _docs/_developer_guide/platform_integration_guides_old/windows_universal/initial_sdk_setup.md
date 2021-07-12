---
nav_title: Initial SDK Setup
platform: Windows_Universal
page_order: 0
description: "This reference article covers the inital SDK integration steps to integrate the Braze SDK on your Windows Universal platform."

---

# Initial SDK Integration

The Braze SDK will provide you with an API to report information to be used in analytics, segmentation, and engagement, as well as the ability to register users for push and receive notifications.

>  The Windows Universal SDK is also compatible with Xamarin Windows Apps.

## Step 1: Install the SDK via the Nuget Package Manager

The Windows Universal SDK is installed via the [Nuget Package Manager][14]. To install the Braze Windows SDK via Nuget:

1. Right-click on the project file
2. Click on "Manage Nuget Packages"
3. Click "Online" in the dropdown menu on the left
4. Search in "Nuget.org" for "Appboy"
5. Click on the "AppboyPlatform.Universal.Release" Nuget Package and click Install

>  The Windows Universal Library should be used for all Windows 8.1, Windows Phone 8.1, and UWP applications.

## Step 2: Creation and Configuration of AppboyConfiguration.xml

Create a file called `AppboyConfiguration.xml` in the root directory of your project and add the following code snippet into that file:

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <AppboyConfig>
        <ApiKey>YOUR_API_KEY_HERE</ApiKey>
    </AppboyConfig>
```
>  Be sure to update `YOUR_API_KEY_HERE` with your API key which can found on the [Settings][1] page within the Braze Dashboard.

Once you've added that snippet, be sure to modify the following file properties for `AppboyConfiguration.xml`

1. Set the `Build Action` to `Content`
2. Set `Copy to Output Directory` to `Copy Always`

## Step 3: Configuring Package.appxmanifest

Within the "Capabilities tab, ensure `Internet (Client)` is checked.
![Internet_Client][18]

## Step 4: Editing your App class

- Add the following to the `usings` of your `App.xaml.cs` file:

```csharp
using AppboyPlatform.PCL.Managers;
using AppboyPlatform.Universal;
using AppboyPlatform.Universal.Managers.PushArgs;
```

- Call the following within your `OnLaunched` lifecycle method:

```csharp
Appboy.SharedInstance.OpenSession();
```

- Call the following within your `OnSuspending` lifecycle method:

```csharp
Appboy.SharedInstance.CloseSession();
```

## Basic SDK Integration Complete

Braze should now be collecting data from your application. Please see the following sections on how to log attributes, events, and purchases to our SDK and how to instrument push messaging.

>  If you are using the Braze Unity project in the same app, you may have to fully qualify calls to Braze as “AppboyPlatform.Universal.Appboy”

[1]: https://dashboard-01.braze.com/app_settings/app_settings "Settings"
[14]: http://www.nuget.org/
[18]: {% image_buster /assets/img_archive/internet_client.png %}

---
nav_title: Troubleshooting
platform: Xamarin
subplatform: Android and FireOS
page_order: 6
description: "This article covers iOS troubleshooting for the Xamarin platform."

---

# Troubleshooting

### Push Doesn't Appear After App is Closed from Task Switcher

If you observe that push notifications no longer appear after the app is closed from the task switcher, your app is likely in Debug mode. Xamarin adds scaffolding in Debug mode that prevents apps from receiving push after their process is killed. If you run your app in Release Mode, you should see push even after the app is closed from the task switcher.

### Custom Notification Factory Not Being Set Correctly

Custom notification factories (and all delegates) must extend [`Java.Lang.Object`][2] to work properly across the C#/Java divide. See Xamarin's [documentation on implementing Java interfaces][1] for more information.

[1]: https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/working_with_jni/#Implementing_Interfaces
[2]: https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/

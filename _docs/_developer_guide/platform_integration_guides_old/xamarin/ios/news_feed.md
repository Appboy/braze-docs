---
nav_title: News Feed
platform: Xamarin
subplatform: iOS
page_order: 3
description: "This article covers iOS News Feed integration for the Xamarin platform."

---

# News Feed

See [the iOS integration instructions][1] for information on how to integrate the News Feed into your Xamarin iOS app.  Furthermore, you can look at the [sample application][2] implementation samples.

Of all the implementation options, the quickest to implement is the Modal, which can be added by doing

```csharp
// C#
ABKFeedViewControllerModalContext m = new ABKFeedViewControllerModalContext ();
this.PresentViewController (m, true, null);
```

in your ViewController.

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/news_feed/
[2]: https://github.com/Appboy/appboy-xamarin-bindings/tree/master/appboy-component/samples

---
nav_title: In-App Message Templates
page_order: 4
description: ""
---

# In-App Message Templates

## Custom App Store Review Prompt

{% tabs %}
{% tab Android & FireOS %}

Due to the limitations and restrictions set by Google, custom Google Play review prompts are not currently supported by Braze. While some users have been able to integrate these prompts successfully, others have shown low success rates due to [Google Play quotas](https://developer.android.com/guide/playcore/in-app-review#quotas). Please integrate at your own risk. Documentation on Google Play in-app review prompts can be found [here](https://developer.android.com/guide/playcore/in-app-review).

{% endtab %}
{% tab iOS %}

{% alert note %}
Once you implement this prompt, Braze stops automatically tracking impressions and you must log analytics with the methods found [here]({{site.baseurl}}/developer_guide/platform_integration_guides/ios/in-app_messaging/customization/#logging-impressions-and-clicks).

{% endalert %}

Creating a campaign to ask users for an App Store review is a popular usage of in-app messages.

Start by [setting the In-App Message delegate](#in-app-message-controller-delegate) in your app. Next, implement the following delegate method to disable the default App Store review message:

{% subtabs global %}
{% subtab OBJECTIVE-C %}

```objc
- (ABKInAppMessageDisplayChoice)beforeInAppMessageDisplayed:(ABKInAppMessage *)inAppMessage {
  if (inAppMessage.extras != nil && inAppMessage.extras[@"Appstore Review"] != nil) {
    [[UIApplication sharedApplication] openURL:inAppMessage.uri options:@{} completionHandler:nil];
    return ABKDiscardInAppMessage;
  } else {
    return ABKDisplayInAppMessageNow;
  }
}
```

{% endsubtab %}
{% subtab swift %}

```swift
func before(inAppMessageDisplayed inAppMessage: ABKInAppMessage) -> ABKInAppMessageDisplayChoice {
  if inAppMessage.extras?["Appstore Review"] != nil && inAppMessage.uri != nil {
    UIApplication.shared.open(inAppMessage.uri!, options: [:], completionHandler: nil)
    return ABKInAppMessageDisplayChoice.discardInAppMessage
  } else {
    return ABKInAppMessageDisplayChoice.displayInAppMessageNow
  }
}
```

{% endsubtab %}
{% endsubtabs %}

In your deep link handling code, you can then add the following code to process the `{YOUR-APP-SCHEME}:appstore-review` deep link. Note that you will need to import `StoreKit` to use `SKStoreReviewController`:

{% subtabs global %}
{% subtab OBJECTIVE-C %}

```objc
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey, id> *)options {
  NSString *urlString = url.absoluteString.stringByRemovingPercentEncoding;
  if ([urlString isEqualToString:@"{YOUR-APP-SCHEME}:appstore-review"]) {
    [SKStoreReviewController requestReview];
    return YES;
  }
  // Other deep link handling code…
}
```

{% endsubtab %}
{% subtab swift %}

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  let urlString = url.absoluteString.removingPercentEncoding
  if (urlString == "{YOUR-APP-SCHEME}:appstore-review") {
    SKStoreReviewController.requestReview()
    return true;
  }
  // Other deep link handling code…
}
```

{% endsubtab %}
{% endsubtabs %}

{% raw %}

Next, create an In-App Messaging campaign with the following:

- add the key-value pair `“Appstore Review” : “true”`
- set the "On Click Behavior" to "Deep Link Into App," using the above deep link (e.g., `{YOUR-APP-SCHEME}:appstore-review`)

{% endraw %}

{% alert tip %}
  Apple limits App Store review prompts to a maximum of three (3) times per year for each user, so your campaign should be [rate-limited]({{site.baseurl}}/user_guide/engagement_tools/campaigns/testing_and_more/rate-limiting/#user-centric-rate-limiting) to three times per year per user.

  Users may turn off App Store review prompts. As a result, your custom review prompt should not promise that a native App Store review prompt will appear, or directly ask for a review.
{% endalert %}

{% endtab %}
{% endtabs %}


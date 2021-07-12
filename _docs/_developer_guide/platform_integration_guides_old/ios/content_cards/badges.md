---
nav_title: Badges
platform: iOS
page_order: 5
description: "This article covers how to add badges to your Content Cards in your iOS application."
channel:
  - content cards

---

# Badges

## Requesting Unread Content Card Counts

If you would like to display the number of unread Content Cards your user has, we suggest you request a card count and represent it with a Badge. Badges are a great way to call attention to new content awaiting your users in the Content Cards. If you'd like to add a badge to your Content Cards, the Braze SDK provides methods to query the following:

- Unviewed Content Cards for the current user
- Total Viewable Content Cards for the current user

The method declarations in [ABKContentCardsController](https://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_content_cards_controller.html) below describe this in detail:

```
/*!
 * This method returns the number of currently active content cards that have not been viewed.
 * A "view" happens when a card becomes visible in the Content Cards view.  This differentiates
 * between cards which are off-screen in the scrolling view, and those which
 * are on-screen; when a card scrolls onto the screen, it's counted as viewed.
 *
 * Cards are counted as viewed only once -- if a card scrolls off the screen and
 * back on, it's not re-counted.
 *
 * Cards are counted only once even if they appear in multiple Content Cards views or across multiple devices.
 */
- (NSInteger)unviewedContentCardCount;
/*!
 * This method returns the total number of currently active content cards. Cards are
 * counted only once even if they appear in multiple Content Cards views.
 */
- (NSInteger)contentCardCount;
```


## Displaying Number of Unviewed Content Cards on App Badge Count

In addition to serving as push notification reminders for an app, Badges can also be utilized to denote unviewed items in the user's Content Cards feed. Updating the badge count based on unviewed Content Cards updates can be a valuable tool in attracting users back to your app and increasing sessions.

This method records the badge count once the app is closed and the user's session ends:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
(void)applicationDidEnterBackground:(UIApplication *)application
```

Within the above method, implement the following code which actively updates the badge count while the user views cards during a given session:

```objc
[UIApplication sharedApplication].applicationIconBadgeNumber = [[Appboy sharedInstance].contentCardsController unviewedContentCardCount];
```

{% endtab %}
{% tab swift %}

```swift
func applicationDidEnterBackground(_ application: UIApplication)
```

Within the above method, implement the following code which actively updates the badge count while the user views cards during a given session:

```swift
UIApplication.shared.applicationIconBadgeNumber =
  Appboy.sharedInstance()?.contentCardsController.unviewedContentCardCount() ?? 0
```

{% endtab %}
{% endtabs %}

For more information see the [`Appboy.h` header file](https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/Appboy.h).

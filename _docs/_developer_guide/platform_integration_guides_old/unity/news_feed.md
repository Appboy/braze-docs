---
nav_title: News Feed
platform: Unity
page_order: 5
description: "This reference article covers News Feed integration for the Unity platform."

---

# News Feed

## Receiving News Feed Data in Unity

You may register Unity Game Objects to be notified of incoming News Feed cards. 

On iOS, we recommend setting game object listeners from the Braze configuration editor.

On Android, set `com_appboy_feed_listener_callback_method_name` and `com_appboy_feed_listener_game_object_name` in your Unity project's `braze.xml`.

- To configure your game object listener at runtime on either platform, use `AppboyBinding.ConfigureListener()` and specify `BrazeUnityMessageType.CONTENT_CARDS_UPDATED`.

## Parsing Content Cards

Incoming `string` messages received in your Content Cards game object callback can be parsed into our pre-supplied [Feed][11] object, which has a list of [Card][12] objects for convenience.

See the following example for details:

### Example Content Cards Callback

```csharp
void FeedReceivedCallback(string message) {
  Feed feed = new Feed(message);
  Debug.Log("Feed received: " + feed);
  foreach (Card card in feed.Cards) {
    Debug.Log("Card: " + card);
  }
}
```

## Refreshing the News Feed

To refresh the News Feed from Braze, call either of the following methods:

```csharp
// results in a network request to Braze
AppboyBinding.RequestFeedRefresh()

AppboyBinding.RequestFeedRefreshFromCache()
```

Both methods will notify your News Feed listener and pass the News Feed along to your callback method.

## Analytics

Clicks and impressions must be manually logged for cards not displayed directly by Braze.

Use `LogClick()` and `LogImpression()` on [Card][12] to log clicks and impressions for specific cards.

To log that the user viewed the feed as a whole, call `AppboyBinding.LogFeedDisplayed()`.

[11]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/models/Feed.cs
[12]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/models/Cards/Card.cs

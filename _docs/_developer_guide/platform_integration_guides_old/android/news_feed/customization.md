---
nav_title: Customization
page_order: 2
platform: Android
description: "This reference article covers how to customize your News Feed in your Android application."
channel:
  - news feed
  
---

# News Feed Customization

## Default Styling

Braze UI elements come with a default look and feel that matches the Android standard UI guidelines and provides a seamless experience. You can see these default styles in the `res/values/style.xml` file in the Braze SDK distribution.

```xml
  <style name="Appboy"/>
  <!-- Feed -->
  <style name="Appboy.Feed"/>
  <style name="Appboy.Feed.List">
    <item name="android:background">@android:color/transparent</item>
    <item name="android:divider">@android:color/transparent</item>
    <item name="android:dividerHeight">16.0dp</item>
    <item name="android:paddingLeft">12.5dp</item>
    <item name="android:paddingRight">5.0dp</item>
    <item name="android:scrollbarStyle">outsideInset</item>
  </style>
  ...
  </style>
```

## Overriding Styles

If you would prefer, you can override these styles to create a look and feel that better suits your app. To override a style, copy it in its entirety to the `styles.xml` file in your own project and make modifications. The whole style must be copied over to your local `styles.xml` file in order for all of the attributes to be correctly set.

#### Correct Style Override

```xml
<style name="Appboy.Feed.List">
  <item name="android:background">@color/mint</item>
  <item name="android:cacheColorHint">@color/mint</item>
  <item name="android:divider">@android:color/transparent</item>
  <item name="android:dividerHeight">16.0dp</item>
  <item name="android:paddingLeft">12.5dp</item>
  <item name="android:paddingRight">5.0dp</item>
  <item name="android:scrollbarStyle">outsideInset</item>
</style>
```

#### Incorrect Style Override

```xml
<style name="Appboy.Feed.List">
  <item name="android:background">@color/mint</item>
  <item name="android:cacheColorHint">@color/mint</item>
</style>
```

## Feed Style Elements

Below is a description of the themable Braze UI elements and their names for styling purposes:

{% gallery %}{% image_buster /assets/img_archive/Image27Theming.png %}
{% image_buster /assets/img_archive/Image28Theming.png %}
{% image_buster /assets/img_archive/Image29Theming.png %}
{% image_buster /assets/img_archive/Image30Theming.png %}{% endgallery %}

## Setting A Custom Font

Braze allows for setting a custom font using the [font family guide][40]. To use it, override a style for cards and use the `fontFamily` attribute to instruct Braze to use your custom font family.

For example, to update the font on all titles for Short News Cards, override the `Appboy.Cards.ShortNews.Title` style and reference your custom font family. The attribute value should point to a font family in your `res/font` directory.

Here is a truncated example with a custom font family, `my_custom_font_family`, referenced on the last line:

```
<style name="Appboy.Cards.ShortNews.Title">
  <item name="android:layout_height">wrap_content</item>
  ...
  <item name="android:fontFamily">@font/my_custom_font_family</item>
  <item name="fontFamily">@font/my_custom_font_family</item>
</style>
```

## Setting a Custom News Feed Click Listener

You can handle News Feed clicks manually by setting a custom News Feed click listener. This enables use cases such as selectively using the native web browser to open web links.

### Step 1: Implement a News Feed Click Listener

Create a class that implements [IFeedClickActionListener][37]. Implement the `onFeedCardClicked()` method, which will be called when the user clicks a News Feed card.

### Step 2: Instruct Braze to Use Your News Feed Click Listener

Once your `IFeedClickActionListener` is created, call `AppboyFeedManager.getInstance().setFeedCardClickActionListener()` to instruct `AppboyFeedManager` to use your custom `IFeedClickActionListener`.

## Fully Custom Feed Display

If you would like to display the feed in a completely custom manner, it is possible to do so by using your own views populated with data from our [models][9]. To obtain Braze's News Feed models, you will need to subscribe for News Feed updates and use the resulting model data to populate your views. You will also need to log analytics on the model objects as users interact with your views.

### Part 1: Subscribing to Feed Updates

First, declare a private variable in your custom feed class to hold your subscriber:

```java
// subscriber variable
private IEventSubscriber<FeedUpdatedEvent> mFeedUpdatedSubscriber;
```

Next, add the following code to subscribe to feed updates from Braze, typically inside of your custom feed activity's `Activity.onCreate()`:

```java
// Remove the old subscription first
Appboy.getInstance(context).removeSingleSubscription(mFeedUpdatedSubscriber, FeedUpdatedEvent.class);
mFeedUpdatedSubscriber = new IEventSubscriber<FeedUpdatedEvent>() {
  @Override
  public void trigger(final FeedUpdatedEvent event) {
    // This list of Card objects included in the FeedUpdatedEvent should be used to populate your News Feed views.
    List<Card> cards = event.getFeedCards();
    // your logic here
  }
};
Appboy.getInstance(context).subscribeToFeedUpdates(mFeedUpdatedSubscriber);

// Request a refresh of feed data
Appboy.getInstance(context).requestFeedRefresh();
```

We also recommend unsubscribing when your custom feed activity moves out of view. Add the following code to your activity's `onDestroy()` lifecycle method:

```
Appboy.getInstance(context).removeSingleSubscription(mFeedUpdatedSubscriber, FeedUpdatedEvent.class);
```

### Part 2: Logging Analytics

When using custom views, you will need to log analytics manually as well, since analytics are only handled automatically when using Braze views.

To log a display of the feed, call [`Appboy.logFeedDisplayed()`][6].

To log an impression or click on a Card, call [`Card.logClick()`][7] and [`Card.logImpression()`][8] respectively.


[6]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logFeedDisplayed--
[7]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#logClick--
[8]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#logImpression--
[9]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/news_feed/card_types/#card-types
[18]: {% image_buster /assets/img_archive/Image27Theming.png %} "Android Feed"
[19]: {% image_buster /assets/img_archive/Image28Theming.png %} "Android Cards"
[20]: {% image_buster /assets/img_archive/Image29Theming.png %} "Android Empty"
[21]: {% image_buster /assets/img_archive/Image30Theming.png %} "Android Network Error"
[37]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/feed/listeners/IFeedClickActionListener.java
[40]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/font_customization/#font-customization

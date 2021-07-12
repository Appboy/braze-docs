---
nav_title: Customization
page_order: 1
platform: FireOS
description: "This article covers customization options for your Content Cards in your Android application."
channel:
  - content cards

---

# Customization

## Default Styling {#default-styling-for-android}

Braze In-App Messages and Content Cards come with a default look and feel that matches the Android standard UI guidelines and provide a seamless experience. You can see these default styles in the [`res/values/styles.xml`][42] file in the Braze SDK distribution.

```xml
  <!-- Content Cards Example -->
  <style name="Appboy.ContentCards.CaptionedImage.Description">
    <item name="android:textColor">@color/com_appboy_description</item>
    <item name="android:textSize">15.0sp</item>
    <item name="android:includeFontPadding">false</item>
    <item name="android:paddingBottom">8.0dp</item>
    <item name="android:layout_marginLeft">10.0dp</item>
    <item name="android:layout_marginRight">10.0dp</item>
    <item name="android:layout_marginTop">8.0dp</item>
    <item name="android:layout_width">match_parent</item>
    <item name="android:layout_below">@id/com_appboy_content_cards_captioned_image_card_title_container</item>
  </style>
```

## Overriding Styles {#overriding-styles-for-android}

If you would prefer, you can override these styles to create a look and feel that better suits your app. To override a style, copy it in its entirety to the `styles.xml` file in your project and make modifications. The whole style must be copied over to your local `styles.xml` file in order for all of the attributes to be correctly set.

### Correct Style Override {#correct-style-override-for-android}

```xml
<style name="Appboy.ContentCardsDisplay">
  <item name="android:background">@color/mint</item>
  <item name="android:cacheColorHint">@color/mint</item>
  <item name="android:divider">@android:color/transparent</item>
  <item name="android:dividerHeight">16.0dp</item>
  <item name="android:paddingLeft">12.5dp</item>
  <item name="android:paddingRight">5.0dp</item>
  <item name="android:scrollbarStyle">outsideInset</item>
</style>
```

### Incorrect Style Override {#incorrect-style-override-for-android}

```xml
<style name="Appboy.ContentCardsDisplay">
  <item name="android:background">@color/mint</item>
  <item name="android:cacheColorHint">@color/mint</item>
</style>
```

## Customizing The Default Content Card Feed {#content-cards-fragment-customization}

This section covers customization of the [AppboyContentCardsFragment][49] whose source can be found [here][54].

### Customizing The Network Connection Error Message

If the [AppboyContentCardsFragment][49] determines that a Content Card refresh has failed, it will display a network connection error message.

A special adapter, the [AppboyEmptyContentCardsAdapter][50] replaces the standard [AppboyCardAdapter][53] to display the error message. To set the custom message itself, override the string resource `com_appboy_feed_empty`.

The style used to display this message can be found via [`Appboy.ContentCardsDisplay.Empty`][52] and is reproduced below.

```xml
<style name="Appboy.ContentCardsDisplay.Empty">
  <item name="android:lineSpacingExtra">1.5dp</item>
  <item name="android:text">@string/com_appboy_feed_empty</item>
  <item name="android:textColor">@color/com_appboy_title</item>
  <item name="android:textSize">18.0sp</item>
  <item name="android:gravity">center</item>
  <item name="android:layout_height">match_parent</item>
  <item name="android:layout_width">match_parent</item>
</style>
```

To fully customize the network error behavior, you can extend the [AppboyContentCardsFragment][54] and set the `mShowNetworkUnavailableRunnable` variable to perform some other action.

## Content Cards Style Elements {#content-cards-style-elements-for-android}

### Custom Font {#setting-a-custom-font-for-android}

Braze allows for setting a custom font using the [font family guide][40]. To use it, override a style for cards and use the `fontFamily` attribute to instruct Braze to use your custom font family.

For example, to update the font on all titles for Captioned Image Cards, override the `Appboy.ContentCards.CaptionedImage.Title` style and reference your custom font family. The attribute value should point to a font family in your `res/font` directory.

Here is a truncated example with a custom font family, `my_custom_font_family`, referenced on the last line:

```xml
  <style name="Appboy.ContentCards.CaptionedImage.Title">
    <item name="android:layout_width">wrap_content</item>
    ...
    <item name="android:fontFamily">@font/my_custom_font_family</item>
    <item name="fontFamily">@font/my_custom_font_family</item>
  </style>
```

### Custom Pinned Icon {#setting-a-custom-pinned-icon-for-android}

To set a custom pinned icon, override the `Appboy.ContentCards.PinnedIcon` style. Your custom image asset should be declared in the `android:src` element.

### Customizing Displayed Card Order {#customizing-displayed-card-order-for-android}

The `AppboyContentCardsFragment` relies on a [`IContentCardsUpdateHandler`][44] to handle any sorting or modifications of Content Cards before they are displayed in the feed. A custom update handler can be set via [`setContentCardUpdateHandler`][45] on your [`AppboyContentCardsFragment`][47].

Filtering out Content Cards before they reach the user's feed is a common use-case and could be achieved by reading the key-value pairs set on the dashboard via [`Card.getExtras()`][36] and performing any logic you'd like in the update handler.

The following is the default `IContentCardsUpdateHandler` and can be used as a starting point for customizations.

{% tabs %}
{% tab JAVA %}

```java
public class DefaultContentCardsUpdateHandler implements IContentCardsUpdateHandler {

  // Interface that must be implemented and provided as a public CREATOR
  // field that generates instances of your Parcelable class from a Parcel.
  public static final Parcelable.Creator<DefaultContentCardsUpdateHandler> CREATOR = new Parcelable.Creator<DefaultContentCardsUpdateHandler>() {
    public DefaultContentCardsUpdateHandler createFromParcel(Parcel in) {
      return new DefaultContentCardsUpdateHandler();
    }

    public DefaultContentCardsUpdateHandler[] newArray(int size) {
      return new DefaultContentCardsUpdateHandler[size];
    }
  };

  @Override
  public List<Card> handleCardUpdate(ContentCardsUpdatedEvent event) {
    List<Card> sortedCards = event.getAllCards();
    // Sort by pinned, then by the 'updated' timestamp descending
    // Pinned before non-pinned
    Collections.sort(sortedCards, new Comparator<Card>() {
      @Override
      public int compare(Card cardA, Card cardB) {
        // A displays above B
        if (cardA.getIsPinned() && !cardB.getIsPinned()) {
          return -1;
        }

        // B displays above A
        if (!cardA.getIsPinned() && cardB.getIsPinned()) {
          return 1;
        }

        // At this point, both A & B are pinned or both A & B are non-pinned
        // A displays above B since A is newer
        if (cardA.getUpdated() > cardB.getUpdated()) {
          return -1;
        }

        // B displays above A since A is newer
        if (cardA.getUpdated() < cardB.getUpdated()) {
          return 1;
        }

        // At this point, every sortable field matches so keep the natural ordering
        return 0;
      }
    });

    return sortedCards;
  }

  // Parcelable interface method
  @Override
  public int describeContents() {
    return 0;
  }

  // Parcelable interface method
  @Override
  public void writeToParcel(Parcel dest, int flags) {
    // No state is kept in this class so the parcel is left unmodified
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class DefaultContentCardsUpdateHandler : IContentCardsUpdateHandler {
  override fun handleCardUpdate(event: ContentCardsUpdatedEvent): List<Card> {
    val sortedCards = event.allCards
    // Sort by pinned, then by the 'updated' timestamp descending
    // Pinned before non-pinned
    sortedCards.sortWith(Comparator sort@{ cardA: Card, cardB: Card ->
      // A displays above B
      if (cardA.isPinned && !cardB.isPinned) {
        return@sort -1
      }

      // B displays above A
      if (!cardA.isPinned && cardB.isPinned) {
        return@sort 1
      }

      // At this point, both A & B are pinned or both A & B are non-pinned
      // A displays above B since A is newer
      if (cardA.updated > cardB.updated) {
        return@sort -1
      }

      // B displays above A since A is newer
      if (cardA.updated < cardB.updated) {
        return@sort 1
      }
      0
    })
    return sortedCards
  }

  // Parcelable interface method
  override fun describeContents(): Int {
    return 0
  }

  // Parcelable interface method
  override fun writeToParcel(dest: Parcel, flags: Int) {
    // No state is kept in this class so the parcel is left unmodified
  }

  companion object {
    // Interface that must be implemented and provided as a public CREATOR
    // field that generates instances of your Parcelable class from a Parcel.
    val CREATOR: Parcelable.Creator<DefaultContentCardsUpdateHandler?> = object : Parcelable.Creator<DefaultContentCardsUpdateHandler?> {
      override fun createFromParcel(`in`: Parcel): DefaultContentCardsUpdateHandler? {
        return DefaultContentCardsUpdateHandler()
      }

      override fun newArray(size: Int): Array<DefaultContentCardsUpdateHandler?> {
        return arrayOfNulls(size)
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

> This code can also be found here, [DefaultContentCardsUpdateHandler][46].

And here's how to use the above class:

{% tabs %}
{% tab JAVA %}

```java
IContentCardsUpdateHandler cardUpdateHandler = new DefaultContentCardsUpdateHandler();

AppboyContentCardsFragment fragment = getMyCustomFragment();
fragment.setContentCardUpdateHandler(cardUpdateHandler);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val cardUpdateHandler = DefaultContentCardsUpdateHandler()

val fragment = getMyCustomFragment()
fragment.setContentCardUpdateHandler(cardUpdateHandler)
```

{% endtab %}
{% endtabs %}

### Customizing Card Rendering {#customizing-card-rendering-for-android}

Here's information on how to change how any card is rendered in the recyclerView. The `IContentCardsViewBindingHandler` interface defines how all Content Cards get rendered. You can customize this to change anything you want.

{% tabs %}
{% tab JAVA %}

```java
public class DefaultContentCardsViewBindingHandler implements IContentCardsViewBindingHandler {
  // Interface that must be implemented and provided as a public CREATOR
  // field that generates instances of your Parcelable class from a Parcel.
  public static final Parcelable.Creator<DefaultContentCardsViewBindingHandler> CREATOR = new Parcelable.Creator<DefaultContentCardsViewBindingHandler>() {
    public DefaultContentCardsViewBindingHandler createFromParcel(Parcel in) {
      return new DefaultContentCardsViewBindingHandler();
    }

    public DefaultContentCardsViewBindingHandler[] newArray(int size) {
      return new DefaultContentCardsViewBindingHandler[size];
    }
  };

  /**
   * A cache for the views used in binding the items in the {@link android.support.v7.widget.RecyclerView}.
   */
  private final Map<CardType, BaseContentCardView> mContentCardViewCache = new HashMap<CardType, BaseContentCardView>();

  @Override
  public ContentCardViewHolder onCreateViewHolder(Context context, List<Card> cards, ViewGroup viewGroup, int viewType) {
    CardType cardType = CardType.fromValue(viewType);
    return getContentCardsViewFromCache(context, cardType).createViewHolder(viewGroup);
  }

  @Override
  public void onBindViewHolder(Context context, List<Card> cards, ContentCardViewHolder viewHolder, int adapterPosition) {
    Card cardAtPosition = cards.get(adapterPosition);
    BaseContentCardView contentCardView = getContentCardsViewFromCache(context, cardAtPosition.getCardType());
    contentCardView.bindViewHolder(viewHolder, cardAtPosition);
  }

  @Override
  public int getItemViewType(Context context, List<Card> cards, int adapterPosition) {
    Card card = cards.get(adapterPosition);
    return card.getCardType().getValue();
  }

  /**
   * Gets a cached instance of a {@link BaseContentCardView} for view creation/binding for a given {@link CardType}.
   * If the {@link CardType} is not found in the cache, then a view binding implementation for that {@link CardType}
   * is created and added to the cache.
   */
  @VisibleForTesting
  BaseContentCardView getContentCardsViewFromCache(Context context, CardType cardType) {
    if (!mContentCardViewCache.containsKey(cardType)) {
      // Create the view here
      BaseContentCardView contentCardView;
      switch (cardType) {
        case BANNER:
          contentCardView = new BannerImageContentCardView(context);
          break;
        case CAPTIONED_IMAGE:
          contentCardView = new CaptionedImageContentCardView(context);
          break;
        case SHORT_NEWS:
          contentCardView = new ShortNewsContentCardView(context);
          break;
        case TEXT_ANNOUNCEMENT:
          contentCardView = new TextAnnouncementContentCardView(context);
          break;
        default:
          contentCardView = new DefaultContentCardView(context);
          break;
      }
      mContentCardViewCache.put(cardType, contentCardView);
    }
    return mContentCardViewCache.get(cardType);
  }

  // Parcelable interface method
  @Override
  public int describeContents() {
    return 0;
  }

  // Parcelable interface method
  @Override
  public void writeToParcel(Parcel dest, int flags) {
    // Retaining views across a transition could lead to a
    // resource leak so the parcel is left unmodified
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class DefaultContentCardsViewBindingHandler : IContentCardsViewBindingHandler {
  // Interface that must be implemented and provided as a public CREATOR
  // field that generates instances of your Parcelable class from a Parcel.
  val CREATOR: Parcelable.Creator<DefaultContentCardsViewBindingHandler?> = object : Parcelable.Creator<DefaultContentCardsViewBindingHandler?> {
    override fun createFromParcel(`in`: Parcel): DefaultContentCardsViewBindingHandler? {
      return DefaultContentCardsViewBindingHandler()
    }

    override fun newArray(size: Int): Array<DefaultContentCardsViewBindingHandler?> {
      return arrayOfNulls(size)
    }
  }

  /**
    * A cache for the views used in binding the items in the [RecyclerView].
    */
  private val mContentCardViewCache: MutableMap<CardType, BaseContentCardView<*>?> = HashMap()

  override fun onCreateViewHolder(context: Context?, cards: List<Card?>?, viewGroup: ViewGroup?, viewType: Int): ContentCardViewHolder? {
    val cardType = CardType.fromValue(viewType)
    return getContentCardsViewFromCache(context, cardType)!!.createViewHolder(viewGroup)
  }

  override fun onBindViewHolder(context: Context?, cards: List<Card>, viewHolder: ContentCardViewHolder?, adapterPosition: Int) {
    if (adapterPosition < 0 || adapterPosition >= cards.size) {
      return
    }
    val cardAtPosition = cards[adapterPosition]
    val contentCardView = getContentCardsViewFromCache(context, cardAtPosition.cardType)
    if (viewHolder != null) {
      contentCardView!!.bindViewHolder(viewHolder, cardAtPosition)
    }
  }

  override fun getItemViewType(context: Context?, cards: List<Card>, adapterPosition: Int): Int {
    if (adapterPosition < 0 || adapterPosition >= cards.size) {
      return -1
    }
    val card = cards[adapterPosition]
    return card.cardType.value
  }

  /**
    * Gets a cached instance of a [BaseContentCardView] for view creation/binding for a given [CardType].
    * If the [CardType] is not found in the cache, then a view binding implementation for that [CardType]
    * is created and added to the cache.
    */
  @VisibleForTesting
  fun getContentCardsViewFromCache(context: Context?, cardType: CardType): BaseContentCardView<Card>? {
    if (!mContentCardViewCache.containsKey(cardType)) {
      // Create the view here
      val contentCardView: BaseContentCardView<*> = when (cardType) {
        CardType.BANNER -> BannerImageContentCardView(context)
        CardType.CAPTIONED_IMAGE -> CaptionedImageContentCardView(context)
        CardType.SHORT_NEWS -> ShortNewsContentCardView(context)
        CardType.TEXT_ANNOUNCEMENT -> TextAnnouncementContentCardView(context)
        else -> DefaultContentCardView(context)
      }
      mContentCardViewCache[cardType] = contentCardView
    }
    return mContentCardViewCache[cardType] as BaseContentCardView<Card>?
  }

  // Parcelable interface method
  override fun describeContents(): Int {
    return 0
  }

  // Parcelable interface method
  override fun writeToParcel(dest: Parcel?, flags: Int) {
    // Retaining views across a transition could lead to a
    // resource leak so the parcel is left unmodified
  }
}
```

{% endtab %}
{% endtabs %}

> This code can also be found here, [DefaultContentCardsViewBindingHandler][56].

And here's how to use the above class:

{% tabs %}
{% tab JAVA %}

```java
IContentCardsViewBindingHandler viewBindingHandler = new DefaultContentCardsViewBindingHandler();

AppboyContentCardsFragment fragment = getMyCustomFragment();
fragment.setContentCardsViewBindingHandler(viewBindingHandler);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val viewBindingHandler = DefaultContentCardsViewBindingHandler()

val fragment = getMyCustomFragment()
fragment.setContentCardsViewBindingHandler(viewBindingHandler)
```

{% endtab %}
{% endtabs %}

There are additional relevant resources on this topic available [here](https://medium.com/google-developers/android-data-binding-recyclerview-db7c40d9f0e4).

### Custom Content Cards Click Listener

You can handle Content Cards clicks manually by setting a custom click listener. This enables use cases such as selectively using the native web browser to open web links.

#### Step 1: Implement a Content Cards Click Listener

Create a class that implements [IContentCardsActionListener][43] and register it with `AppboyContentCardsManager`. Implement the `onContentCardClicked()` method, which will be called when the user clicks a content card.

#### Step 2: Instruct Braze to Use Your Content Card Click Listener

You can see an example of steps 1 and 2 here:

{% tabs %}
{% tab JAVA %}

```java
AppboyContentCardsManager.getInstance().setContentCardsActionListener(new IContentCardsActionListener() {
  @Override
  public boolean onContentCardClicked(Context context, Card card, IAction cardAction) {
    return false;
  }

  @Override
  public void onContentCardDismissed(Context context, Card card) {

  }
});
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
AppboyContentCardsManager.getInstance().contentCardsActionListener = object : IContentCardsActionListener {
  override fun onContentCardClicked(context: Context, card: Card, cardAction: IAction): Boolean {
    return false
  }

  override fun onContentCardDismissed(context: Context, card: Card) {

  }
}
```

{% endtab %}
{% endtabs %}

### Dark Theme Customization

By default, Content Cards views will automatically respond to Dark Theme changes on the device with a set of themed colors and layout changes. 

To override this behavior, override the `values-night` values in `android-sdk-ui/src/main/res/values-night/colors.xml` and `android-sdk-ui/src/main/res/values-night/dimens.xml`.

### Fully Custom Content Card Display {#fully-custom-content-card-display-for-android}

If you would like to display the Content Cards in a completely custom manner, it is possible to do so by using your own views populated with data from our models. To obtain Braze’s Content Cards models, you will need to subscribe to content card updates and use the resulting model data to populate your views. You will also need to log analytics on the model objects as users interact with your views.

#### Part 1: Subscribing to Content Card Updates

First, declare a private variable in your custom class to hold your subscriber:

{% tabs %}
{% tab JAVA %}

```java
// subscriber variable
private IEventSubscriber<ContentCardsUpdatedEvent> mContentCardsUpdatedSubscriber;
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
private var mContentCardsUpdatedSubscriber: IEventSubscriber<ContentCardsUpdatedEvent>? = null
```

{% endtab %}
{% endtabs %}

Next, add the following code to subscribe to Content Card updates from Braze, typically inside of your custom Content Cards activity's `Activity.onCreate()`:

{% tabs %}
{% tab JAVA %}

```java
// Remove the previous subscriber before rebuilding a new one with our new activity.
Appboy.getInstance(context).removeSingleSubscription(mContentCardsUpdatedSubscriber, ContentCardsUpdatedEvent.class);
mContentCardsUpdatedSubscriber = new IEventSubscriber<ContentCardsUpdatedEvent>() {
    @Override
    public void trigger(ContentCardsUpdatedEvent event) {
        // List of all content cards
        List<Card> allCards = event.getAllCards();

        // Your logic below
    }
};
Appboy.getInstance(context).subscribeToContentCardsUpdates(mContentCardsUpdatedSubscriber);
Appboy.getInstance(context).requestContentCardsRefresh(true);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
// Remove the previous subscriber before rebuilding a new one with our new activity.
Appboy.getInstance(context).removeSingleSubscription(mContentCardsUpdatedSubscriber, ContentCardsUpdatedEvent::class.java)
mContentCardsUpdatedSubscriber = IEventSubscriber { event ->
  // List of all content cards
  val allCards = event.allCards

  // Your logic below
}
Appboy.getInstance(context).subscribeToContentCardsUpdates(mContentCardsUpdatedSubscriber)
Appboy.getInstance(context).requestContentCardsRefresh(true)
```

{% endtab %}
{% endtabs %}

We also recommend unsubscribing when your custom activity moves out of view. Add the following code to your activity's `onDestroy()` lifecycle method:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).removeSingleSubscription(mContentCardsUpdatedSubscriber, ContentCardsUpdatedEvent.class);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).removeSingleSubscription(mContentCardsUpdatedSubscriber, ContentCardsUpdatedEvent::class.java)
```

{% endtab %}
{% endtabs %}

#### Part 2: Logging Analytics

When using custom views, you will need to log analytics manually as well, since analytics are only handled automatically when using Braze views.

To log a display of the Content Cards, call [`Appboy.logContentCardsDisplayed()`][41].

To log an impression or click on a Card, call [`Card.logClick()`][7] or [`Card.logImpression()`][8] respectively.

For campaigns using Control Cards for A/B testing, you can use [`Card.isControl()`][55] to determine if a card will be blank, and used only for tracking purposes.

#### Manually Dismissing a Content Card

You can manually log or set a Content Card as "dismissed" to Braze [for a particular card with `setIsDismissed`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#setIsDismissed-boolean-).

If a card is already marked as dismissed, it cannot be marked as dismissed again.

## Disabling Swipe To Dismiss

Disabling swipe-to-dismiss functionality is done on a per-card basis via the [`card.setIsDismissibleByUser()`][48] method. Cards can be intercepted before display using the [`AppboyContentCardsFragment.setContentCardUpdateHandler()`][45] method.

## Key-Value Pairs

`Card` objects may optionally carry key-value pairs as `extras`. These can be used to send data down along with a `Card` for further handling by the application.

See the [Javadoc][36] for more information.

{% alert note %}
Content Cards have a maximum size of **2kb** (including images, links, and all content) - exceeding that amount will prevent the card from sending.
{% endalert %}

## GIFs {#gifs-news-content-cards}

{% alert note %}
This section applies to integrations which use the Braze SDK's default Content Cards Fragment or Views to display Content Cards.
{% endalert %}

{% include archive/android/gifs.md channel="Content Cards" %}


[7]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#logClick--
[8]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#logImpression--
[36]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#getExtras--
[40]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/font_customization/#font-customization
[41]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#logContentCardsDisplayed--
[42]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/res/values/styles.xml
[43]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/contentcards/listeners/IContentCardsActionListener.html
[44]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/contentcards/handlers/IContentCardsUpdateHandler.html
[45]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/AppboyContentCardsFragment.html#setContentCardUpdateHandler-com.appboy.ui.contentcards.handlers.IContentCardsUpdateHandler-
[46]: https://github.com/Appboy/appboy-android-sdk/blob/v11.0.0/android-sdk-ui/src/main/java/com/appboy/ui/contentcards/handlers/DefaultContentCardsUpdateHandler.java
[47]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/AppboyContentCardsFragment.html
[48]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#setIsDismissibleByUser-boolean-
[49]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/AppboyContentCardsFragment.html
[50]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/contentcards/AppboyEmptyContentCardsAdapter.html
[52]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/res/values/styles.xml#L552-L560
[53]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/contentcards/AppboyCardAdapter.html
[54]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/AppboyContentCardsFragment.java
[55]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/cards/Card.html#isControl--
[56]: https://github.com/Appboy/appboy-android-sdk/blob/v11.0.0/android-sdk-ui/src/main/java/com/appboy/ui/contentcards/handlers/DefaultContentCardsViewBindingHandler.java

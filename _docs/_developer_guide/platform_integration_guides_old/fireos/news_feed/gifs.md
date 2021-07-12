---
nav_title: GIFs
page_order: 9
platform: FireOS
description: "This reference article covers how to implement gifs in your News Feed for your Android applciation."
channel:
  - news feed

---

# GIFs {#gifs-news-feed}

Braze requires an external image library to display animated GIFs with News Feed

### Custom Image Library Integration {#gifs-delegate-integration}

Braze offers the ability to use a custom image library to display animated GIFs with the News Feed

__Note:__ Although the example below uses [Glide][gifs-67], any image library that supports GIFs is compatible.

#### Step 1: Creating the Image Loader Delegate

The Image Loader delegate must implement the following methods:

* [`getInAppMessageBitmapFromUrl()`][gifs-71]
* [`getPushBitmapFromUrl()`][gifs-72]
* [`renderUrlIntoCardView()`][gifs-73]
* [`renderUrlIntoInAppMessageView()`][gifs-74]
* [`setOffline()`][gifs-70]

The integration example below is taken from the [Glide Integration Sample App][gifs-65] included with the Braze Android SDK.

{% tabs %}
{% tab JAVA %}

```java
public class GlideAppboyImageLoader implements IAppboyImageLoader {
  private static final String TAG = GlideAppboyImageLoader.class.getName();

  private RequestOptions mRequestOptions = new RequestOptions();

  @Override
  public void renderUrlIntoCardView(Context context, Card card, String imageUrl, ImageView imageView, AppboyViewBounds viewBounds) {
    renderUrlIntoView(context, imageUrl, imageView, viewBounds);
  }

  @Override
  public void renderUrlIntoInAppMessageView(Context context, IInAppMessage inAppMessage, String imageUrl, ImageView imageView, AppboyViewBounds viewBounds) {
    renderUrlIntoView(context, imageUrl, imageView, viewBounds);
  }

  @Override
  public Bitmap getPushBitmapFromUrl(Context context, Bundle extras, String imageUrl, AppboyViewBounds viewBounds) {
    return getBitmapFromUrl(context, imageUrl, viewBounds);
  }

  @Override
  public Bitmap getInAppMessageBitmapFromUrl(Context context, IInAppMessage inAppMessage, String imageUrl, AppboyViewBounds viewBounds) {
    return getBitmapFromUrl(context, imageUrl, viewBounds);
  }

  private void renderUrlIntoView(Context context, String imageUrl, ImageView imageView, AppboyViewBounds viewBounds) {
    Glide.with(context)
        .load(imageUrl)
        .apply(mRequestOptions)
        .into(imageView);
  }

  private Bitmap getBitmapFromUrl(Context context, String imageUrl, AppboyViewBounds viewBounds) {
    try {
      return Glide.with(context)
          .asBitmap()
          .apply(mRequestOptions)
          .load(imageUrl).submit().get();
    } catch (Exception e) {
      Log.e(TAG, "Failed to retrieve bitmap at url: " + imageUrl, e);
    }
    return null;
  }

  @Override
  public void setOffline(boolean isOffline) {
    // If the loader is offline, then we should only be retrieving from the cache
    mRequestOptions = mRequestOptions.onlyRetrieveFromCache(isOffline);
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class GlideAppboyImageLoader : IAppboyImageLoader {
  companion object {
    private val TAG = GlideAppboyImageLoader::class.qualifiedName
  }

  private var mRequestOptions = RequestOptions()

  override fun renderUrlIntoCardView(context: Context, card: Card, imageUrl: String, imageView: ImageView, viewBounds: AppboyViewBounds) {
    renderUrlIntoView(context, imageUrl, imageView, viewBounds)
  }

  override fun renderUrlIntoInAppMessageView(context: Context, inAppMessage: IInAppMessage, imageUrl: String, imageView: ImageView, viewBounds: AppboyViewBounds) {
    renderUrlIntoView(context, imageUrl, imageView, viewBounds)
  }

  override fun getPushBitmapFromUrl(context: Context, extras: Bundle, imageUrl: String, viewBounds: AppboyViewBounds): Bitmap? {
    return getBitmapFromUrl(context, imageUrl, viewBounds)
  }

  override fun getInAppMessageBitmapFromUrl(context: Context, inAppMessage: IInAppMessage, imageUrl: String, viewBounds: AppboyViewBounds): Bitmap? {
    return getBitmapFromUrl(context, imageUrl, viewBounds)
  }

  private fun renderUrlIntoView(context: Context, imageUrl: String, imageView: ImageView, viewBounds: AppboyViewBounds) {
    Glide.with(context)
        .load(imageUrl)
        .apply(mRequestOptions)
        .into(imageView)
  }

  private fun getBitmapFromUrl(context: Context, imageUrl: String, viewBounds: AppboyViewBounds): Bitmap? {
    try {
      return Glide.with(context)
          .asBitmap()
          .apply(mRequestOptions)
          .load(imageUrl).submit().get()
    } catch (e: Exception) {
      Log.e(TAG, "Failed to retrieve bitmap at url: $imageUrl", e)
    }

    return null
  }

  override fun setOffline(isOffline: Boolean) {
    // If the loader is offline, then we should only be retrieving from the cache
    mRequestOptions = mRequestOptions.onlyRetrieveFromCache(isOffline)
  }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Setting the Image Loader Delegate

The Braze SDK will use any custom image loader set with [setAppboyImageLoader][gifs-66]. Note that we recommend setting the custom image loader in a custom application subclass.

{% tabs %}
{% tab JAVA %}

```java
public class GlideIntegrationApplication extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    Appboy.getInstance(context).setAppboyImageLoader(new GlideAppboyImageLoader());
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class GlideIntegrationApplication : Application() {
  override fun onCreate() {
    super.onCreate()
    Appboy.getInstance(context).appboyImageLoader = GlideAppboyImageLoader()
  }
}
```

{% endtab %}
{% endtabs %}

[gifs-56]: http://developer.android.com/reference/android/app/Application.html
[gifs-59]: https://github.com/Appboy/appboy-android-sdk#version-support
[gifs-60]: http://developer.android.com/guide/topics/manifest/application-element.html#nm
[gifs-61]: https://github.com/Appboy/appboy-android-sdk/tree/master/droidboy
[gifs-64]: https://github.com/Appboy/appboy-android-sdk/tree/master/droidboy
[gifs-65]: https://github.com/Appboy/appboy-android-sdk/tree/master/samples/glide-image-integration
[gifs-66]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#setAppboyImageLoader-com.appboy.IAppboyImageLoader-
[gifs-67]: https://bumptech.github.io/glide/
[gifs-70]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyImageLoader.html#setOffline-boolean-
[gifs-71]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyImageLoader.html#getInAppMessageBitmapFromUrl-android.content.Context-com.appboy.models.IInAppMessage-java.lang.String-com.appboy.enums.AppboyViewBounds-
[gifs-72]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyImageLoader.html#getPushBitmapFromUrl-android.content.Context-android.os.Bundle-java.lang.String-com.appboy.enums.AppboyViewBounds-
[gifs-73]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyImageLoader.html#renderUrlIntoCardView-android.content.Context-com.appboy.models.cards.Card-java.lang.String-android.widget.ImageView-com.appboy.enums.AppboyViewBounds-
[gifs-74]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/IAppboyImageLoader.html#renderUrlIntoInAppMessageView-android.content.Context-com.appboy.models.IInAppMessage-java.lang.String-android.widget.ImageView-com.appboy.enums.AppboyViewBounds-


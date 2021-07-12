---
nav_title: Customization
page_order: 2
platform: Android
description: "This reference article covers in-app messaging customization options for your Android application."
channel:
  - in-app messages

---

# Customization {#in-app-message-customization}

All of Braze’s in-app message types are highly customizable across messages, images, [Font Awesome][15]  icons, click actions, analytics, editable styling, custom display options, and custom delivery options. Multiple options can be configured on a per in-app message basis from [within the dashboard]({{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/). Braze additionally provides multiple levels of advanced customization to satisfy a variety of use cases and needs.

## Key Value Pair Extras

In-app message objects may carry key-value pairs as `extras`. They are specified on the dashboard under "Advanced Settings" when creating an in-app message campaign. These can be used to send data down along with an in-app message for further handling by the application.

Call the following when you get an in-app message object to retrieve its extras:

{% tabs %}
{% tab JAVA %}

```java
Map<String, String> getExtras()
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
extras: Map<String, String>
```

{% endtab %}
{% endtabs %}

See the [Javadoc][44] for more information.

## Custom Styling

Braze UI elements come with a default look and feel that matches the Android standard UI guidelines and provides a seamless experience. You can see these default styles in the Braze SDK's [`styles.xml`][6] file.

```xml
  <style name="Appboy"/>
    <!-- In-app Message -->
  <style name="Appboy.InAppMessage">
  </style>
  <style name="Appboy.InAppMessage.Header">
    <item name="android:layout_height">wrap_content</item>
    <item name="android:layout_width">wrap_content</item>
    <item name="android:padding">0.0dp</item>
    <item name="android:background">@android:color/transparent</item>
    <item name="android:textColor">@color/com_appboy_inappmessage_header_text_light</item>
    <item name="android:textSize">19.0sp</item>
    <item name="android:layout_gravity">center</item>
    <item name="android:singleLine">true</item>
    <item name="android:textStyle">bold</item>
  </style>
```

If you would prefer, you can override these styles to create a look and feel that better suits your app.

To override a style, copy it in its entirety to the `styles.xml` file in your project and make modifications. The whole style must be copied over to your local `styles.xml` file for all attributes to be correctly set. Please note that these custom styles are for changes to individual UI elements, not wholesale changes to layouts. Layout-level changes need to be handled with custom views.

### Using Custom Styling to Set a Custom Font

Braze allows for setting a custom font using the [font family guide][79]. To use it, override the style for message text, headers, and/or button text and use the `fontFamily` attribute to instruct Braze to use your custom font family.

For example, to update the font on your in-app message button text, override the `Appboy.InAppMessage.Button` style and reference your custom font family. The attribute value should point to a font family in your `res/font` directory.

Here is a truncated example with a custom font family, `my_custom_font_family`, referenced on the last line:

```xml
  <style name="Appboy.InAppMessage.Button">
    <item name="android:layout_height">wrap_content</item>
    ...
    <item name="android:paddingBottom">15.0dp</item>
    <item name="android:fontFamily">@font/my_custom_font_family</item>
    <item name="fontFamily">@font/my_custom_font_family</item>
  </style>
```

Aside from the `Appboy.InAppMessage.Button` style for button text, the style for message text is `Appboy.InAppMessage.Message` and the style for message headers is `Appboy.InAppMessage.Header`. If you want to use your custom font family across all possible in-app message text, you can set your font family on the `Appboy.InAppMessage` style, which is the parent style for all in-app messages.

{% alert important %}
As with other custom styles, the entire style must be copied over to your local `styles.xml` file for all attributes to be correctly set.
{% endalert %}

## Setting Custom Listeners

Before customizing in-app messages with custom listeners, it's important to understand the [`AppboyInAppMessageManager`][34], which handles the majority of in-app message handling. As described in [Step 1][5], it must be registered for in-app messages to function appropriately.

`AppboyInAppMessageManager` manages in-app message display on Android.  It contains helper class instances that help it manage the lifecycle and display of in-app messages. All of these classes have standard implementations and defining custom classes is completely optional. However, doing so can add another level of control over the display and behavior of in-app messages.  These customizable classes include:

- [`IInAppMessageManagerListener`][21] - Implement to [custom manage in-app message display and behavior](#setting-a-custom-manager-listener).
- [`IInAppMessageViewFactory`][42] - Implement to [build custom in-app message views](#setting-a-custom-view-factory).
- [`IInAppMessageAnimationFactory`][20] - Implement to [define custom in-app message animations](#setting-a-custom-animation-factory).
- [`IHtmlInAppMessageActionListener`][86] - Implement to [custom manage HTML in-app message display and behavior](#setting-a-custom-html-in-app-message-action-listener).
- [`IInAppMessageViewWrapperFactory`][88] - Implement to [custom manage in-app message view hierarchy interaction](#setting-a-custom-view-wrapper-factory).

### Custom Manager Listener

The `AppboyInAppMessageManager` automatically handles the display and lifecycle of in-app messages.  If you require more control over the lifecycle of a message, setting a custom manager listener will enable you to receive the in-app message object at various points in the in-app message lifecycle, allowing you to handle its display yourself, perform further processing, react to user behavior, process the object's [Extras][14], and much more.

#### Step 1: Implement an In-App Message Manager Listener

Create a class that implements [`IInAppMessageManagerListener`][21].

The callbacks in your `IInAppMessageManagerListener` will be called at various points in the in-app message lifecycle.

For example, if you set a custom manager listener when an in-app message is received from Braze, the `beforeInAppMessageDisplayed()` method will be called. If your implementation of this method returns [`InAppMessageOperation.DISCARD`][83], that signals to Braze that the in-app message will be handled by the host app and should not be displayed by Braze. If `InAppMessageOperation.DISPLAY_NOW` is returned, Braze will attempt to display the in-app message. This method should be used if you choose to display the in-app message in a customized manner.

`IInAppMessageManagerListener` also includes delegate methods for clicks on the message itself or one of the buttons.  A common use case would be intercepting a message when a button or message is clicked for further processing.

#### Step 2: Hook into In-App Message View Lifecycle Methods (Optional)

The [`IInAppMessageManagerListener`][21] interface has in-app message view methods that are called at distinct points in the in-app message view lifecycle. These methods are called in the following order:

- [beforeInAppMessageViewOpened][92] - Called just before the in-app message is added to the Activity's view. The in-app message is not yet visible to the user at this time.
- [afterInAppMessageViewOpened][93] - Called just after the in-app message is added to the Activity's view. The in-app message is now visible to the user at this time.
- [beforeInAppMessageViewClosed][94] - Called just before the in-app message is removed from the Activity's view. The in-app message is still visible to the user at this time.
- [afterInAppMessageViewClosed][95] - Called just after the in-app message is removed from the Activity's view. The in-app message is no longer visible to the user at this time.

For further context, the time between [afterInAppMessageViewOpened][93] and [beforeInAppMessageViewClosed][94] is when the in-app message view is on screen, visible to the user.

{% alert note %}
  No implementation of these methods is required. They are merely provided to track/inform of the in-app message view lifecycle. It is functionally acceptable to leave these method implementations empty.
{% endalert %}

#### Step 3: Instruct Braze to use your In-App Message Manager Listener

Once your `IInAppMessageManagerListener` is created, call `AppboyInAppMessageManager.getInstance().setCustomInAppMessageManagerListener()` to instruct `AppboyInAppMessageManager`
to use your custom `IInAppMessageManagerListener` instead of the default listener.

We recommend setting your `IInAppMessageManagerListener` in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom listener is set before any in-app message is displayed.

##### In-Depth: Altering In-App Messages Before Display

When a new in-app message is received, and there is already an in-app message being displayed, the new message will be put onto the top of the stack and can be displayed at a later time.

However, if there is no in-app message being displayed, the following delegate method in `IInAppMessageManagerListener` will be called:

{% tabs %}
{% tab JAVA %}

```java
@Override
public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessageBase) {
  return InAppMessageOperation.DISPLAY_NOW;
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
override fun beforeInAppMessageDisplayed(inAppMessageBase: IInAppMessage): InAppMessageOperation {
  return InAppMessageOperation.DISPLAY_NOW
}
```

{% endtab %}
{% endtabs %}

The `InAppMessageOperation()` return value can be used to control when the message should be displayed. The suggested usage of this method would be to delay messages in certain parts of the app by returning `DISPLAY_LATER` when in-app messages would be distracting to the user's app experience.

| `InAppMessageOperation` return value | Behavior |
| -------------------------- | -------- |
| `DISPLAY_NOW` | The message will be displayed |
| `DISPLAY_LATER` | The message will be returned to the stack and displayed at the next available opportunity |
| `DISCARD` | The message will be discarded |
| `null` | The message will be ignored. This method should __NOT__ return `null` |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

See [`InAppMessageOperation.java`][45] for more details.

{% alert tip %}
If you choose to `DISCARD` the in-app message and replace it with your own in-app message view, you will need to manually log in-app message clicks and impressions.
{% endalert %}

On Android, this is done by calling `logClick` and `logImpression` on in-app messages, and `logButtonClick` on immersive in-app messages.

{% alert tip %}
Once an in-app message has been placed on the stack, you can request for it to be retrieved and displayed at any time by calling [`AppboyInAppMessageManager.getInstance().requestDisplayInAppMessage()`](https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/AppboyInAppMessageManager.html#requestDisplayInAppMessage--). Calling this method requests Braze to display the next available in-app message from the stack.
{% endalert %}

#### Step 4: Customizing Dark Theme Behavior (Optional) {#android-in-app-message-dark-theme-customization}

In the default `IInAppMessageManagerListener` logic, in `beforeInAppMessageDisplayed()`, the system settings are checked and conditionally enable Dark Theme styling on the message with the following code:

{% tabs %}
{% tab JAVA %}

```java
@Override
public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessage) {
  if (inAppMessage instanceof IInAppMessageThemeable && ViewUtils.isDeviceInNightMode(AppboyInAppMessageManager.getInstance().getApplicationContext())) {
    ((IInAppMessageThemeable) inAppMessage).enableDarkTheme();
  }
  return InAppMessageOperation.DISPLAY_NOW;
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
override fun beforeInAppMessageDisplayed(inAppMessage: IInAppMessage): InAppMessageOperation {
  if (inAppMessage is IInAppMessageThemeable && ViewUtils.isDeviceInNightMode(AppboyInAppMessageManager.getInstance().applicationContext!!)) {
    (inAppMessage as IInAppMessageThemeable).enableDarkTheme()
  }
  return InAppMessageOperation.DISPLAY_NOW
}
```

{% endtab %}
{% endtabs %}

If you would like to use your own conditional logic, then you can call [`enableDarkTheme`][97] at any step in the pre-display process.

### Custom View Factory

Braze's suite of in-app message types is versatile enough to cover the vast majority of custom use cases. However, if you would like to fully define the visual appearance of your in-app messages instead of using a default type, Braze makes this possible by setting a custom view factory.

#### Step 1: Implement an In-App Message View Factory

Create a class that implements [`IInAppMessageViewFactory`][87].

{% tabs %}
{% tab JAVA %}

```java
public class CustomInAppMessageViewFactory implements IInAppMessageViewFactory {
  @Override
  public View createInAppMessageView(Activity activity, IInAppMessage inAppMessage) {
    // Uses a custom view for slideups, modals, and full in-app messages.
    // HTML in-app messages and any other types will use the Braze default in-app message view factories
    switch (inAppMessage.getMessageType()) {
      case SLIDEUP:
      case MODAL:
      case FULL:
        // Use a custom view of your choosing
        return createMyCustomInAppMessageView();
      default:
        // Use the default in-app message factories
        final IInAppMessageViewFactory defaultInAppMessageViewFactory = AppboyInAppMessageManager.getInstance().getDefaultInAppMessageViewFactory(inAppMessage);
        return defaultInAppMessageViewFactory.createInAppMessageView(activity, inAppMessage);
    }
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomInAppMessageViewFactory : IInAppMessageViewFactory {
  override fun createInAppMessageView(activity: Activity, inAppMessage: IInAppMessage): View {
    // Uses a custom view for slideups, modals, and full in-app messages.
    // HTML in-app messages and any other types will use the Braze default in-app message view factories
    when (inAppMessage.messageType) {
      MessageType.SLIDEUP, MessageType.MODAL, MessageType.FULL ->
        // Use a custom view of your choosing
        return createMyCustomInAppMessageView()
      else -> {
        // Use the default in-app message factories
        val defaultInAppMessageViewFactory = AppboyInAppMessageManager.getInstance().getDefaultInAppMessageViewFactory(inAppMessage)
        return defaultInAppMessageViewFactory!!.createInAppMessageView(activity, inAppMessage)
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your In-App Message View Factory

Once your `IInAppMessageViewFactory` is created, call `AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewFactory()` to instruct `AppboyInAppMessageManager`
to use your custom `IInAppMessageViewFactory` instead of the default view factory.

{% alert tip %}
We recommend setting your `IInAppMessageViewFactory` in your `Application.onCreate()` before any other calls to Braze. This will ensure that the custom view factory is set before any in-app message is displayed.
{% endalert %}

##### In-Depth: Implementing a Braze View Interface

Braze's `slideup` in-app message view implements [`IInAppMessageView`][25].  Braze's `full` and `modal` type message views implement [`IInAppMessageImmersiveView`][24].  Implementing one of these classes will allow Braze to add click listeners to your custom view where appropriate.  All Braze view classes extend Android's [View][18] class.

Implementing `IInAppMessageView` allows you to define a certain portion of your custom view as clickable. Implementing [`IInAppMessageImmersiveView`][24] allows you to define message button views and a close button view.

### Custom Animation Factory

In-app messages have preset animation behavior. `Slideup` type messages slide into the screen; `full` and `modal` messages fade in and out.  If you would like to define custom animation behaviors for your in-app messages, Braze makes this possible by setting a custom animation factory.

#### Step 1: Implement an In-App Message Animation Factory

Create a class that implements [`IInAppMessageAnimationFactory`][20]

{% tabs %}
{% tab JAVA %}

```java
public class CustomInAppMessageAnimationFactory implements IInAppMessageAnimationFactory {

  @Override
  public Animation getOpeningAnimation(IInAppMessage inAppMessage) {
    Animation animation = new AlphaAnimation(0, 1);
    animation.setInterpolator(new AccelerateInterpolator());
    animation.setDuration(2000L);
    return animation;
  }

  @Override
  public Animation getClosingAnimation(IInAppMessage inAppMessage) {
    Animation animation = new AlphaAnimation(1, 0);
    animation.setInterpolator(new DecelerateInterpolator());
    animation.setDuration(2000L);
    return animation;
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomInAppMessageAnimationFactory : IInAppMessageAnimationFactory {
  override fun getOpeningAnimation(inAppMessage: IInAppMessage): Animation {
    val animation: Animation = AlphaAnimation(0, 1)
    animation.interpolator = AccelerateInterpolator()
    animation.duration = 2000L
    return animation
  }

  override fun getClosingAnimation(inAppMessage: IInAppMessage): Animation {
    val animation: Animation = AlphaAnimation(1, 0)
    animation.interpolator = DecelerateInterpolator()
    animation.duration = 2000L
    return animation
  }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your In-App Message View Factory

Once your `IInAppMessageAnimationFactory` is created, call `AppboyInAppMessageManager.getInstance().setCustomInAppMessageAnimationFactory()` to instruct `AppboyInAppMessageManager`
to use your custom `IInAppMessageAnimationFactory` instead of the default animation factory.

We recommend setting your `IInAppMessageAnimationFactory` in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom animation factory is set before any in-app message is displayed.

### Custom HTML In-App Message Action Listener

The Braze SDK has a default `AppboyDefaultHtmlInAppMessageActionListener` class that is used if no custom listener is defined and takes appropriate action automatically. If you require more control over how a user interacts with different buttons inside a custom HTML in-app message, implement a custom `IHtmlInAppMessageActionListener` class.

#### Step 1: Implement a Custom HTML In-App Message Action Listener

Create a class that implements [`IHtmlInAppMessageActionListener`][86].

The callbacks in your `IHtmlInAppMessageActionListener` will be called whenever the user initiates any of the following actions inside the HTML in-app message:
- Clicks on close button.
- Clicks on News Feed button.
- Fires a custom event.
- Clicks on a URL inside HTML in-app message.

{% tabs %}
{% tab JAVA %}

```java
public class CustomHtmlInAppMessageActionListener implements IHtmlInAppMessageActionListener {
  private final Context mContext;

  public CustomHtmlInAppMessageActionListener(Context context) {
    mContext = context;
  }

  @Override
  public void onCloseClicked(IInAppMessage inAppMessage, String url, Bundle queryBundle) {
    Toast.makeText(mContext, "HTML In App Message closed", Toast.LENGTH_LONG).show();
    AppboyInAppMessageManager.getInstance().hideCurrentlyDisplayingInAppMessage(false);
  }

  @Override
  public boolean onCustomEventFired(IInAppMessage inAppMessage, String url, Bundle queryBundle) {
    Toast.makeText(mContext, "Custom event fired. Ignoring.", Toast.LENGTH_LONG).show();
    return true;
  }

  @Override
  public boolean onNewsfeedClicked(IInAppMessage inAppMessage, String url, Bundle queryBundle) {
    Toast.makeText(mContext, "Newsfeed button pressed. Ignoring.", Toast.LENGTH_LONG).show();
    AppboyInAppMessageManager.getInstance().hideCurrentlyDisplayingInAppMessage(false);
    return true;
  }

  @Override
  public boolean onOtherUrlAction(IInAppMessage inAppMessage, String url, Bundle queryBundle) {
    Toast.makeText(mContext, "Custom url pressed: " + url + " . Ignoring", Toast.LENGTH_LONG).show();
    AppboyInAppMessageManager.getInstance().hideCurrentlyDisplayingInAppMessage(false);
    return true;
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomHtmlInAppMessageActionListener(private val mContext: Context) : IHtmlInAppMessageActionListener {

    override fun onCloseClicked(inAppMessage: IInAppMessage, url: String, queryBundle: Bundle) {
        Toast.makeText(mContext, "HTML In App Message closed", Toast.LENGTH_LONG).show()
        AppboyInAppMessageManager.getInstance().hideCurrentlyDisplayingInAppMessage(false)
    }

    override fun onCustomEventFired(inAppMessage: IInAppMessage, url: String, queryBundle: Bundle): Boolean {
        Toast.makeText(mContext, "Custom event fired. Ignoring.", Toast.LENGTH_LONG).show()
        return true
    }

    override fun onNewsfeedClicked(inAppMessage: IInAppMessage, url: String, queryBundle: Bundle): Boolean {
        Toast.makeText(mContext, "Newsfeed button pressed. Ignoring.", Toast.LENGTH_LONG).show()
        AppboyInAppMessageManager.getInstance().hideCurrentlyDisplayingInAppMessage(false)
        return true
    }

    override fun onOtherUrlAction(inAppMessage: IInAppMessage, url: String, queryBundle: Bundle): Boolean {
        Toast.makeText(mContext, "Custom url pressed: $url . Ignoring", Toast.LENGTH_LONG).show()
        AppboyInAppMessageManager.getInstance().hideCurrentlyDisplayingInAppMessage(false)
        return true
    }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your HTML In-App Message Action Listener

Once your `IHtmlInAppMessageActionListener` is created, call `AppboyInAppMessageManager.getInstance().setCustomHtmlInAppMessageActionListener()` to instruct `AppboyInAppMessageManager` to use your custom `IHtmlInAppMessageActionListener` instead of the default action listener.

We recommend setting your `IHtmlInAppMessageActionListener` in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom action listener is set before any in-app message is displayed.

{% tabs %}
{% tab JAVA %}

```java
AppboyInAppMessageManager.getInstance().setCustomHtmlInAppMessageActionListener(new CustomHtmlInAppMessageActionListener(context));
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
AppboyInAppMessageManager.getInstance().setCustomHtmlInAppMessageActionListener(CustomHtmlInAppMessageActionListener(context))
```

{% endtab %}
{% endtabs %}

### Custom View Wrapper Factory

The `AppboyInAppMessageManager` automatically handles placing the in-app message model into the existing Activity view hierarchy by default using [`DefaultInAppMessageViewWrapper`][89]. If you need to customize how in-app messages are placed into the view hierarchy, you should use a custom [`IInAppMessageViewWrapperFactory`][88].

#### Step 1: Implement an In-App Message View Wrapper Factory

Create a class that implements [`IInAppMessageViewWrapperFactory`][88] and returns an [IInAppMessageViewWrapper][90].

This factory is called immediately after the in-app message view is created. The easiest way to implement a custom [IInAppMessageViewWrapper][90] is just to extend the default [`DefaultInAppMessageViewWrapper`][89].

{% tabs %}
{% tab JAVA %}

```java
public class CustomInAppMessageViewWrapper extends DefaultInAppMessageViewWrapper {
  public CustomInAppMessageViewWrapper(View inAppMessageView,
                                       IInAppMessage inAppMessage,
                                       IInAppMessageViewLifecycleListener inAppMessageViewLifecycleListener,
                                       BrazeConfigurationProvider brazeConfigurationProvider,
                                       Animation openingAnimation,
                                       Animation closingAnimation, View clickableInAppMessageView) {
    super(inAppMessageView,
        inAppMessage,
        inAppMessageViewLifecycleListener,
        brazeConfigurationProvider,
        openingAnimation,
        closingAnimation,
        clickableInAppMessageView);
  }

  @Override
  public void open(@NonNull Activity activity) {
    super.open(activity);
    Toast.makeText(activity.getApplicationContext(), "Opened in-app message", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void close() {
    super.close();
    Toast.makeText(mInAppMessageView.getContext().getApplicationContext(), "Closed in-app message", Toast.LENGTH_SHORT).show();
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomInAppMessageViewWrapper(inAppMessageView: View,
                                    inAppMessage: IInAppMessage,
                                    inAppMessageViewLifecycleListener: IInAppMessageViewLifecycleListener,
                                    brazeConfigurationProvider: BrazeConfigurationProvider,
                                    openingAnimation: Animation,
                                    closingAnimation: Animation, clickableInAppMessageView: View) : 
    DefaultInAppMessageViewWrapper(inAppMessageView, 
        inAppMessage, 
        inAppMessageViewLifecycleListener, 
        brazeConfigurationProvider, 
        openingAnimation, 
        closingAnimation, 
        clickableInAppMessageView) {

  override fun open(activity: Activity) {
    super.open(activity)
    Toast.makeText(activity.applicationContext, "Opened in-app message", Toast.LENGTH_SHORT).show()
  }

  override fun close() {
    super.close()
    Toast.makeText(mInAppMessageView.context.applicationContext, "Closed in-app message", Toast.LENGTH_SHORT).show()
  }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your Custom View Wrapper Factory

Once your [IInAppMessageViewWrapper][90] is created, call [`AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory()`][91] to instruct `AppboyInAppMessageManager` to use your custom [`IInAppMessageViewWrapperFactory`][88] instead of the default view wrapper factory.

We recommend setting your [`IInAppMessageViewWrapperFactory`][88] in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom view wrapper factory is set before any in-app message is displayed.

{% tabs %}
{% tab JAVA %}

```java
AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory(new CustomInAppMessageViewWrapper());
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory(CustomInAppMessageViewWrapper())
```

{% endtab %}
{% endtabs %}

## Setting Fixed Orientation

To set a fixed orientation for an in-app message, first [set a custom in-app message manager listener][19]. Then, call `setOrientation()` on the `IInAppMessage` object in the `beforeInAppMessageDisplayed()` delegate method.

{% tabs %}
{% tab JAVA %}

```java
public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessage) {
  // Set the orientation to portrait
  inAppMessage.setOrientation(Orientation.PORTRAIT);
  return InAppMessageOperation.DISPLAY_NOW;
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
override fun beforeInAppMessageDisplayed(inAppMessage: IInAppMessage): InAppMessageOperation {
  // Set the orientation to portrait
  inAppMessage.orientation = Orientation.PORTRAIT
  return InAppMessageOperation.DISPLAY_NOW
}
```

{% endtab %}
{% endtabs %}


For *tablet* devices, in-app messages will appear in the style of the user's preferred orientation regardless of actual screen orientation.

## Disabling Back Button Dismissal

By default, the hardware back button dismisses Braze In-App Messages. This behavior can be disabled on a per-message basis via [`AppboyInAppMessageManager.setBackButtonDismissesInAppMessageView()`][96]. 

In the following example, `disable_back_button` is a custom key-value pair set on the In-App Message that signifies whether the message should allow for the back button to dismiss the message.

{% tabs %}
{% tab JAVA %}

```java
AppboyInAppMessageManager.getInstance().setCustomInAppMessageManagerListener(new AppboyDefaultInAppMessageManagerListener() {
  @Override
  public void beforeInAppMessageViewOpened(View inAppMessageView, IInAppMessage inAppMessage) {
    super.beforeInAppMessageViewOpened(inAppMessageView, inAppMessage);
    final Map<String, String> extras = inAppMessage.getExtras();
    if (extras != null && extras.containsKey("disable_back_button")) {
      AppboyInAppMessageManager.getInstance().setBackButtonDismissesInAppMessageView(false);
    }
  }

  @Override
  public void afterInAppMessageViewClosed(IInAppMessage inAppMessage) {
    super.afterInAppMessageViewClosed(inAppMessage);
    AppboyInAppMessageManager.getInstance().setBackButtonDismissesInAppMessageView(true);
  }
});
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
AppboyInAppMessageManager.getInstance().setCustomInAppMessageManagerListener(object : AppboyDefaultInAppMessageManagerListener() {
  override fun beforeInAppMessageViewOpened(inAppMessageView: View, inAppMessage: IInAppMessage) {
    super.beforeInAppMessageViewOpened(inAppMessageView, inAppMessage)
    val extras = inAppMessage.extras
    if (extras != null && extras.containsKey("disable_back_button")) {
      AppboyInAppMessageManager.getInstance().setBackButtonDismissesInAppMessageView(false)
    }
  }

  override fun afterInAppMessageViewClosed(inAppMessage: IInAppMessage) {
    super.afterInAppMessageViewClosed(inAppMessage)
    AppboyInAppMessageManager.getInstance().setBackButtonDismissesInAppMessageView(true)
  }
})
```

{% endtab %}
{% endtabs %}

{% alert note %}
Note that if this functionality is disabled, the host Activity's hardware back button default behavior will be used instead. This may lead to the back button instead of closing the application instead of the displayed in-app message.
{% endalert %}


## GIFs {#gifs-IAMs}

Braze requires an external image library to display animated GIFs with {{ include.channel }}.

### Custom Image Library Integration {#gifs-delegate-integration}

Braze offers the ability to use a custom image library to display animated GIFs with {{ include.channel }}.

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

## Android Dialogs

Braze doesn't support displaying in-app messages in [Android Dialogs][85] at this time.

## Google Play In-App Review Prompt

Due to the limitations and restrictions set by Google, custom Google Play review prompts are not currently supported by Braze. While some users have been able to integrate these prompts successfully, others have shown low success rates due to [Google Play quotas](https://developer.android.com/guide/playcore/in-app-review#quotas). Please integrate at your own risk. Documentation on Google Play in-app review prompts can be found [here](https://developer.android.com/guide/playcore/in-app-review).

## Youtube in HTML In-App messages

Youtube and other HTML5 content can play in HTML in-app messages. This requires hardware acceleration to be enabled in the Activity where the in-app message is being displayed, please see the [Android developer guide][84] for more details. Hardware acceleration is only available on Android API versions 11 and above.

The following is an example of an embedded Youtube video in an HTML snippet:

```html
<body>
    <div class="box">
        <div class="relativeTopRight">
            <a href="appboy://close">X</a>
        </div>
        <iframe width="60%" height="50%" src="https://www.youtube.com/embed/_x45EB3BWqI">
        </iframe>
    </div>
</body>
```



[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/integration/#step-1-braze-in-app-message-manager-registration
[6]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/res/values/styles.xml
[14]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/news_feed/key-value_pairs/
[15]: http://fortawesome.github.io/Font-Awesome/
[18]: http://developer.android.com/reference/android/view/View.html
[19]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/customization/#setting-custom-listeners
[20]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/IInAppMessageAnimationFactory.html
[21]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/listeners/IInAppMessageManagerListener.html
[24]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/IInAppMessageImmersiveView.html
[25]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/inappmessage/IInAppMessageView.java
[34]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/inappmessage/AppboyInAppMessageManager.java
[42]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/IInAppMessageViewFactory.html
[44]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessage.html#getExtras--
[45]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/inappmessage/InAppMessageOperation.java
[79]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/font_customization/#font-customization
[82]: https://developer.android.com/reference/android/app/Application.html#onCreate()
[83]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/inappmessage/InAppMessageOperation.java
[84]: https://developer.android.com/guide/topics/graphics/hardware-accel.html#controlling
[85]: https://developer.android.com/guide/topics/ui/dialogs.html
[86]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/listeners/IHtmlInAppMessageActionListener.html
[87]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/IInAppMessageViewFactory.html
[88]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/IInAppMessageViewWrapperFactory.html
[89]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/main/java/com/appboy/ui/inappmessage/DefaultInAppMessageViewWrapper.java
[90]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/IInAppMessageViewWrapper.html
[91]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/AppboyInAppMessageManagerBase.html#setCustomInAppMessageViewWrapperFactory-com.appboy.ui.inappmessage.IInAppMessageViewWrapperFactory-
[92]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/listeners/IInAppMessageManagerListener.html#beforeInAppMessageViewOpened-android.view.View-com.appboy.models.IInAppMessage-
[93]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/listeners/IInAppMessageManagerListener.html#afterInAppMessageViewOpened-android.view.View-com.appboy.models.IInAppMessage-
[94]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/listeners/IInAppMessageManagerListener.html#beforeInAppMessageViewClosed-android.view.View-com.appboy.models.IInAppMessage-
[95]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/listeners/IInAppMessageManagerListener.html#afterInAppMessageViewClosed-com.appboy.models.IInAppMessage-
[96]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/AppboyInAppMessageManagerBase.html#setBackButtonDismissesInAppMessageView-boolean-
[97]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessageThemeable.html#enableDarkTheme--
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

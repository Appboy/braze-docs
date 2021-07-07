---
nav_title: Advanced Implementation (Optional)
platform: Android
page_order: 6
description: "This advanced implementation guide covers Android in-app message code considerations, three use cases built by our team, and accompanying code snippets."
channel:
  - in-app messages
---

{% alert important %}
Looking for the out-of-the-box in-app message developer integration guide? Find it [here]({{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/customization/).
{% endalert %}

# In-App Messaging Implementation Guide

> This optional and advanced implementation guide covers in-app message code considerations, three custom use cases built by our team, and accompanying code snippets. Visit our Braze Demo repository [here](https://github.com/braze-inc/braze-growth-shares-android-demo-app)! Please note that this implementation guide is centered around a Kotlin implementation, but Java snippets are provided for those interested.  Looking for HTML implementations? Take a look at our [HTML template repository](https://github.com/braze-inc/in-app-message-templates)!

## Code Considerations

The following guide offers an optional custom developer integration to use in addition to out-of-the-box in-app messages. Custom view components and factories are included as needed below with each use case, offering examples to extend functionality and natively customize the look and feel of your in-app messages. There are, in some instances, multiple ways to achieve similar results. The optimal implementation will depend on the specific use case.

### Custom Factories

The Braze SDK allows developers to override a number of defaults through the use of custom factory objects. These can be registered with the Braze SDK as needed to achieve the desired results. In most cases, however, if you decide to override a factory, you will need to either explicitly defer to the default or reimplement the functionality provided by the Braze default. The code snippet below illustrates how to supply custom implementations of the `IInAppMessageViewFactory` and the `IInAppMessageViewWrapperFactory` interfaces. Once you have a solid understanding of the concepts behind overriding Braze's default factories, check out our [use cases](#sample-use-cases) below to get started implementing custom in-app messaging functionality.

{% tabs %}
{% tab Kotlin %}
__In App Message Types__<br>

```kotlin
class BrazeDemoApplication : Application(){
 override fun onCreate() {
    super.onCreate()
    registerActivityLifecycleCallbacks(AppboyLifecycleCallbackListener(true, true))
    AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory(CustomInAppMessageViewWrapperFactory())
    AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewFactory(CustomInAppMessageViewFactory())
  }
}
```
{% endtab %}
{% tab Java %}
__In App Message Types__<br> 

```java
public class BrazeDemoApplication extends Application {
  @Override
  public void onCreate{
    super.onCreate();
    registerActivityLifecycleCallbacks(new AppboyLifecycleCallbackListener(true, true));
    AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory(new CustomInAppMessageViewWrapperFactory());
    AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewFactory(new CustomInAppMessageViewFactory());
  }
}
```
{% endtab %}
{% endtabs %}

## Sample Use Cases

There are three sample customer use cases provided. Each sample has code snippets and a look into how in-app messages may look and be used in the Braze dashboard:
- [Custom Slideup In-App Message](#custom-slideup-in-app-message)
- [Custom Modal In-App Message](#custom-modal-in-app-message)
- [Custom Full In-App Message](#custom-full-in-app-message)

### Custom Slideup In-App Message

While building out your slide-up in-app message, you may notice you aren't able to modify the placement of the message. While this option is not explicitly offered out-of-the-box, modification like this is made possible by subclassing the `DefaultInAppMessageViewWrapper` class to adjust the layout parameters. You can adjust the final position on the screen by overriding the `getLayoutParams` method, returning the modified `LayoutParams` with your own custom positioning values. Visit the [CustomSlideUpInAppMessageViewWrapper](TODO) to get started.

#### Custom View Wrapper<br><br>

{% tabs %}
{% tab Java %}
__Override and Return Custom Layout Parameters__<br>
Within the `getLayoutParams` method, you can use the superclass method to access the original `LayoutParameters` for the in-app message. Then, you can adjust the position by adding or subtracting as desired.

```java
TODO
```
{% endtab %}
{% tab Kotlin %}
__Override and Return Custom Layout Parameters__<br>
Within the `getLayoutParams` method, you can use the superclass method to access the original `LayoutParameters` for the in-app message. Then, you can adjust the position by adding or subtracting as desired.

```kotlin
class CustomSlideUpInAppMessageViewWrapper(inAppMessageView: View?,
                                           inAppMessage: IInAppMessage?,
                                           inAppMessageViewLifecycleListener: IInAppMessageViewLifecycleListener?,
                                           configurationProvider: BrazeConfigurationProvider?,
                                           openingAnimation: Animation?,
                                           closingAnimation: Animation?,
                                           clickableInAppMessageView: View?) : DefaultInAppMessageViewWrapper(inAppMessageView,
    inAppMessage,
    inAppMessageViewLifecycleListener,
    configurationProvider,
    openingAnimation,
    closingAnimation,
    clickableInAppMessageView) {

    override fun getLayoutParams(inAppMessage: IInAppMessage?): ViewGroup.LayoutParams {
        val params = super.getLayoutParams(inAppMessage) as FrameLayout.LayoutParams
        params.bottomMargin = params.bottomMargin + 500 //move the view up by 500 pixels
        return params
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Kotlin %} 
__Supply a Custom Factory to Return Your Custom Wrapper__<br>
In order to ensure that the Braze SDK uses your custom wrapper, you also need to supply a custom `IInAppMessageViewWrapperFactory` implementation that returns your custom wrapper. You can either implement the `IInAppMessageViewWrapperFactory` directly, or subclass `AppboyInAppMessageViewWrapperFactory` and only override the `createInAppMessageViewWrapper` method:

```kotlin
class CustomInAppMessageViewWrapperFactory : AppboyInAppMessageViewWrapperFactory() {

    override fun createInAppMessageViewWrapper(
        inAppMessageView: View?,
        inAppMessage: IInAppMessage?,
        inAppMessageViewLifecycleListener: IInAppMessageViewLifecycleListener?,
        configurationProvider: BrazeConfigurationProvider?,
        openingAnimation: Animation?,
        closingAnimation: Animation?,
        clickableInAppMessageView: View?
    ): IInAppMessageViewWrapper {
        return if (inAppMessage is InAppMessageSlideup) {
            CustomSlideUpInAppMessageViewWrapper( //return our custom view wrapper only for slideups
                inAppMessageView,
                inAppMessage,
                inAppMessageViewLifecycleListener,
                configurationProvider,
                openingAnimation,
                closingAnimation,
                clickableInAppMessageView
            )
        } else {
            super.createInAppMessageViewWrapper( //defer to the default implementation for all other IAM types
                inAppMessageView,
                inAppMessage,
                inAppMessageViewLifecycleListener,
                configurationProvider,
                openingAnimation,
                closingAnimation,
                clickableInAppMessageView
            )
        }
    }
}
```
{% endtab %}
{% tab Java %}
__Supply a Custom Factory to Return Your Custom Wrapper__<br>
In order to ensure that the Braze SDK uses your custom wrapper, you also need to supply a custom `IInAppMessageViewWrapperFactory` implementation that returns your custom wrapper. You can either implement the `IInAppMessageViewWrapperFactory` directly, or subclass `AppboyInAppMessageViewWrapperFactory` and only override the `createInAppMessageViewWrapper` method:

```java
TODO
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Kotlin %}
__Register Your Factory with Braze__<br>
Once you've created your custom wrapper factory, register it with the Braze SDK via the `AppboyInAppMessageManager`:

```kotlin
AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory(CustomInAppMessageViewWrapperFactory())
```
{% endtab %}
{% tab Java %}
__Register Your Factory with Braze__<br>
Once you've created your custom wrapper factory, register it with the Braze SDK via the `AppboyInAppMessageManager`:

```java
AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewWrapperFactory(new CustomInAppMessageViewWrapperFactory());
```
{% endtab %}
{% endtabs %}

### Custom Modal In-App Message

An `AppboyInAppMessageModalView` can be subclassed to leverage a `Spinner` offering engaging ways to collect valuable user attributes. The example below shows how you can use Connected Content to capture custom attributes from a dynamic list of items. Visit the [TeamPickerView](TODO) to get started.

{% tabs %}
{% tab Kotlin %}
__Using `view_type` for UI Display Behavior__<br>
The `IInAppMessage` object has an `extras` dictionary that we can query to find the `view_type` key (if any) and display the correct type of view. It’s important to note that in-app messages are configured on a per-message basis, so custom and out-of-the-box modal views can work harmoniously.

```kotlin
override fun createInAppMessageView(activity: Activity, inAppMessage: IInAppMessage): View {
  return when {
      inAppMessage.extras?.get("view_type") == "picker" -> {
          getCustomPickerView(activity, inAppMessage)
      }
      //...
      else -> {
          //Defer to default
          AppboyInAppMessageManager
              .getInstance()
              .getDefaultInAppMessageViewFactory(inAppMessage).createInAppMessageView(activity, inAppMessage)
      }
  }
}
```
{% endtab %}
{% tab Java %}
__Using `view_type` for UI Display Behavior__<br>
The `IInAppMessage` object has an `extras` dictionary that we can query to find the `view_type` key (if any) and display the correct type of view. It’s important to note that in-app messages are configured on a per-message basis, so custom and out-of-the-box modal views can work harmoniously.

```java
TODO
```
{% endtab %}
{% endtabs %}

__Override and Provide Custom View__<br>
Provide a layout that mimics the standard modal in-app message, but supply your view as the root elment, and then inflate that layout 
```xml
<com.braze.advancedsamples.inapp.modal.TeamPickerView xmlns:android="http://schemas.android.com/apk/res/android"
                                                      xmlns:tools="http://schemas.android.com/tools"
                                                      android:layout_width="match_parent"
                                                      android:layout_height="match_parent"
                                                      android:padding="0.0dp"
                                                      android:id="@+id/team_picker_view">
    <!-- ... -->
    <Spinner android:layout_width="match_parent" android:layout_height="wrap_content"
                     android:id="@+id/team_spinner"/>
    <!-- ... -->                                                      
</com.braze.advancedsamples.inapp.modal.TeamPickerView>
```

{% tabs %}
{% tab Kotlin %}
__Inflate and Customize the View__<br>
Before reloading the `Spinner` components, the `inAppMessage` message variable is output as a _String_. This message must be formatted as an array of items to be displayed correctly. As an example, this can be achieved using `String.split(",")`.

```kotlin
private fun getCustomView(activity: Activity, inAppMessage: IInAppMessage): TeamPickerView {
        val view = activity.layoutInflater.inflate(R.layout.team_picker_dialog, null) as TeamPickerView
        val teams = inAppMessage.message.split(",")
        view.setTeams(teams)
        return view
    }
```
{% endtab %}
{% tab Java %}
__Inflate and Customize the View__<br>
Before reloading the `Spinner` components, the `inAppMessage` message variable is output as a _String_. This message must be formatted as an array of items to be displayed correctly. As an example, this can be achieved using `String.split(",")`.

```java
private TeamPickerView getCustomView(Activity activity, IInAppMessage inAppMessage) {
        TeamPickerView view = (TeamPickerView) activity.getLayoutInflater().inflate(R.layout.team_picker_dialog, null);
        String[] teams = inAppMessage.getMessage().split(",");
        view.setTeams(teams);
        return view
    }
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Kotlin %}
__Assign Custom Attribute__<br>
Using the view subclass, after a user presses submit, pass the attribute with its corresponding selected value to Braze and dismiss the in-app message by calling `messageClickableView.performClick()`.

```kotlin
    override fun onClick(v: View?) {
        val selectedTeam = spinner.selectedItem as String;
        Appboy.getInstance(ctx).getCurrentUser<AppboyUser>()?.setCustomUserAttribute("FavoriteTeam", selectedTeam)
        messageClickableView.performClick()
    }
```
{% endtab %}
{% tab Java %}
__Assign Custom Attribute__<br>
Using the view subclass, after a user presses submit, pass the attribute with its corresponding selected value to Braze and dismiss the in-app message by calling `messageClickableView.performClick()`.

```java
TODO
```
{% endtab %}
{% endtabs %}

### Custom Full In-App Message
Implementing a fully custom immersive (full screen) in-app message involves a similar approach outlined above for implementing a customized modal in-app message. In this instance, however, simply extend `AppboyInAppMessageFullView` and customize as needed. Just remember that the view will be displayed over the application UI, and views in Android by default are transparent. This means you will need to define a background such that the in-app message obscures the content behind it. By extending `AppboyInAppMessageFullView`, the Braze SDK will handle intercepting touch events on the view and take the appropriate action. Like with the modal example, you can override this behavior for certain controls (like `Switch` controls) to collect feedback from the user.

{% tabs %}
{% tab Kotlin %}
__Using `view_type` for UI Display Behavior__<br>
We will add another `view_type` extra for our new immersive customization. Revisiting the `createInAppMessageView` method, add a option for the "switches" UI:

```kotlin
override fun createInAppMessageView(activity: Activity, inAppMessage: IInAppMessage): View {
    return when {
        inAppMessage.extras?.get("view_type") == "picker" -> {
            getCustomPickerView(activity, inAppMessage)
        }
        inAppMessage.extras?.get("view_type") == "switches" -> {
            getCustomImmersiveView(activity, inAppMessage) // new customization
        }
        else -> {
            //Defer to default
            AppboyInAppMessageManager
                .getInstance()
                .getDefaultInAppMessageViewFactory(inAppMessage).createInAppMessageView(activity, inAppMessage)
        }
    }
}
```
{% endtab %}
{% tab Java %}
__Using `view_type` for UI Display Behavior__<br>
We will add another `view_type` extra for our new immersive customization. Revisiting the `createInAppMessageView` method, add a option for the "switches" UI:

```java
TODO
```
{% endtab %}
{% endtabs %}

__Override and Provide Custom View__<br>
Provide a layout that mimics the standard modal in-app message, but supply your view as the root elment, and then inflate that layout 
```xml
<?xml version="1.0" encoding="utf-8"?>
<com.braze.advancedsamples.immersive.CustomImmersiveInAppMessage
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="wrap_content">
    <!-- giving the parent layout a white backround color will obscure the app behind the IAM. You could also do this within your custom view -->
    <LinearLayout android:background="@color/white" android:layout_width="match_parent" android:layout_height="match_parent" android:gravity="center"> 
        <!-- ... -->
        <androidx.recyclerview.widget.RecyclerView android:layout_width="match_parent"
                                                       android:layout_height="wrap_content"
                                                       android:id="@+id/option_list"/>
        <!-- ... -->
    </LinearLayout>
</com.braze.advancedsamples.immersive.CustomImmersiveInAppMessage>
```

{% tabs %}
{% tab Kotlin %}
__Inflate and Customize the View__<br>
Before setting the options for the `RecyclerView` component, the `inAppMessage` message variable is output as a _String_. This message must be formatted as an array of items to be displayed correctly. As an example, this can be achieved using `String.split(",")`. The `title` and `subtitle` are also extracted from the `extras` bundle.

```kotlin
private fun getCustomImmersiveView(activity: Activity, inAppMessage: IInAppMessage): CustomImmersiveInAppMessage{
    val view = activity.layoutInflater.inflate(R.layout.full_screen_iam, null) as CustomImmersiveInAppMessage
    val options = inAppMessage.message.split(",")
    view.setOptions(options)
    inAppMessage.extras?.get("title").let { view.setTitle(it) }
    inAppMessage.extras?.get("subtitle").let {view.setSubtitle(it) }
    return view
}
```
{% endtab %}
{% tab Java %}
__Inflate and Customize the View__<br>
Before setting the options for the `RecyclerView` component, the `inAppMessage` message variable is output as a _String_. This message must be formatted as an array of items to be displayed correctly. As an example, this can be achieved using `String.split(",")`. The `title` and `subtitle` are also extracted from the `extras` bundle.

```java
//TODO
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Kotlin %}
__Assign Custom Attribute__<br>
Using the view subclass, after a user toggles one of the switches, pass the associated attribute and the toggle status to Braze.

```kotlin
fun logClick(value:String, checked:Boolean){
    Appboy.getInstance(ctx).logCustomEvent("SwitchChanged", BrazeProperties())
}

inner class OptionViewHolder(item: View): RecyclerView.ViewHolder(item), View.OnClickListener{

    var value: String = ""

    override fun onClick(p0: View?) {
        if (p0 is Switch){
            val checked = p0.isChecked
            p0.isChecked = !p0.isChecked
            logClick(value, checked)
        }
    }
}
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): OptionViewHolder {
    return OptionViewHolder(mInflater.inflate(R.layout.switch_cell, null))
}

override fun onBindViewHolder(holder: OptionViewHolder, position: Int) {
    holder.itemView.findViewById<TextView>(R.id.label).text = options[position]
    holder.value = options[position]
}
```
{% endtab %}
{% tab Java %}
__Assign Custom Attribute__<br>
Using the view subclass, after a user toggles one of the switches, pass the associated attribute and the toggle status to Braze.

```java
TODO
```
{% endtab %}
{% endtabs %}

#### Intercepting In-App Message Touches
![Touches][1]{: style="float:right;max-width:30%;margin-left:10px;border:0"}
Intercepting in-app message touches is crucial in making the custom full in-app message buttons function correctly. By default, all in-app message views add `onClick` listeners onto the message, so users can dismiss messages without buttons. When you add custom controls that should respond to user input (like custom buttons), you can register an `onClick` listener with the view as normal. Any touches outside of the custom controls will dismiss the in-app message as usual, while touches received by the custom controls will invoke your `onClick` listener. 

[1]: {% image_buster /assets/img/iam_implementation_guide.png %}

---
nav_title: Customer Feedback
platform: FireOS
page_order: 5
hidden: true
page_type: reference

---

{% alert Update %}
Customer Feedback is no longer supported. [Learn more about this and other deprecated features here]({{site.baseurl}}/help/release_notes/deprecations/#feedback).
{% endalert %}

# Customer Feedback

The Braze feedback form allows users to submit feedback about your app that is immediately sent to your company's dashboard.

![Android ActivityFeed+FeedBack][1]

In Android, the News Feed and feedback form are implemented as [Fragments][2] that are available in the Braze Android UI project. View [Google's documentation on Fragments][3] for information on how to add a Fragment to an Activity.

>  The Android UI Fragments do not automatically track session analytics. To ensure that sessions are tracked correctly, you should call `IAppboy.openSession()` when your app is opened (learn more about [tracking user sessions][6]).

## Braze Feedback Form

The `AppboyFeedbackFragment` class creates a Feedback form and makes calls to the core Braze library to send feedback to Braze. The `AppboyFeedbackFragment` contains two buttons, "cancel" and "send", which when pressed will call a `FeedbackFinishedListener`. You must set a `FeedbackFinishedListener` on your fragment to handle navigation when a user interacts with the form:

```java
AppboyFeedbackFragment appboyFeedbackFragment = new AppboyFeedbackFragment();
appboyFeedbackFragment.setFeedbackFinishedListener(new AppboyFeedbackFragment.FeedbackFinishedListener() {

    @Override
    public void onFeedbackFinished(AppboyFeedbackFragment.FeedbackResult feedbackResult) {
        // Here you can take action on the feedbackResult
    }

    @Override
    public String beforeFeedbackSubmitted(String feedbackString) {
        return feedbackString;
    }
});
```

## Manual Feedback Collection

Braze has a [method][5] allows you to pass Feedback to Braze from a form or field within your app. This is perfect for passing feedback from an existing UI element to Braze. The method will return a boolean which indicates whether or not the feedback was queued for delivery.

## Third-Party Provider Integrations

Braze has easy integrations with both [Desk.com][13] and [Zendesk][14]. So long as you are collecting feedback through our ready-made UI or manually using the `submitFeedback` method, you can pass that feedback through to either third-party provider. This will afford you the benefit of having the entire user profile card available to the CSR handling the case, and allow you to segment based upon the number of feedback requests a user has submitted.

To take advantage of these integrations, please visit the "Feedback" section within the **Settings** page

## Theming the Braze Feedback UI

Braze UI elements come with a default look and feel that matches the Android standard UI guidelines and provides a seamless experience. You can see these default styles in the `res/values/style.xml` file in the Braze SDK distribution. They are completely open source and completely customizable to suit your application's aesthetic.

```xml
  <style name="Appboy"/>
  <!-- Feedback -->
  <style name="Appboy.Feedback"/>
  <style name="Appboy.Feedback.Layout"/>
  <style name="Appboy.Feedback.NavigationBar">
    <item name="android:padding">4dp</item>
    <item name="android:background">@color/com_appboy_feedback_form_navigation_bar_background</item>
  </style>
  <style name="Appboy.Feedback.NavigationBarCancelButton">
    <item name="android:layout_marginRight">2dp</item>
    <item name="android:text">@string/com_appboy_feedback_form_cancel</item>
    <item name="android:textStyle">bold</item>
  </style>
```

## Overriding Styles {#override-feedback}

If you would prefer, you can override these styles to create a look and feel that better suits your app. To override a style, copy it in its entirety to the `styles.xml` file in your own project and make modifications. The whole style must be copied over to your local `styles.xml` file in order for all of the attributes to be correctly set.

### Correct Style Override {#correct-feedback}

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

### Incorrect Style Override {#incorrect-feedback}

```xml
<style name="Appboy.Feed.List">
  <item name="android:background">@color/mint</item>
  <item name="android:cacheColorHint">@color/mint</item>
</style>
```

### Feedback Form Style Elements {#feedback-style}

![Android Feedback][20]

[1]: {% image_buster /assets/img_archive/droidfeed.png %} "Android News Feed and Feedback Form"
[2]: http://developer.android.com/guide/components/fragments.html
[3]: http://developer.android.com/guide/components/fragments.html#Adding "Android Documentation: Fragments"
[5]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#submitFeedback-java.lang.String-java.lang.String-boolean- "Feedback documentation"
[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/tracking_sessions/
[13]: http://www.desk.com
[14]: http://www.zendesk.com
[20]: {% image_buster /assets/img_archive/Image31Theming.png %} "Android Feedback"

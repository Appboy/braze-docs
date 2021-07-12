---
nav_title: In-App Messaging
platform: Unity
page_order: 2
description: "This reference article covers in-app messaging integration guidelines for the Unity platform."

---

# In-App Messaging

## Configuring Default In-app Message Behavior

{% tabs %}
{% tab Android %}

On Android, in-app messages from Braze are automatically displayed natively. To disable this functionality, deselect "Automatically Display In-App Messages" in the Braze configuration editor.

You may alternatively set `com_appboy_inapp_show_inapp_messages_automatically` to `false` in your Unity project's `braze.xml`.

{% endtab %}
{% tab iOS %}

On iOS, in-app messages from Braze are automatically displayed natively. To disable this functionality, set game object listeners in the Braze configuration editor, and ensure "Braze Displays In-App Messages" is not selected.

{% endtab %}
{% endtabs %}

## Configuring In-App Message Display Behavior

You may optionally change the display behavior of In-App Messages at runtime via the following:

```csharp
// Sets In-App Messages to display immediately when triggered.
Appboy.AppboyBinding.SetInAppMessageDisplayAction(BrazeUnityInAppMessageDisplayActionType.IAM_DISPLAY_NOW);

// Sets In-App Messages to display at a later time and be saved in a stack.
Appboy.AppboyBinding.SetInAppMessageDisplayAction(BrazeUnityInAppMessageDisplayActionType.IAM_DISPLAY_LATER);

// Sets In-App Messages to be discarded after being triggered.
Appboy.AppboyBinding.SetInAppMessageDisplayAction(BrazeUnityInAppMessageDisplayActionType.IAM_DISCARD);
```

## Receiving In-App Message Data in Unity

You may register Unity Game Objects to be notified of incoming in-app messages. We recommend setting game object listeners from the Braze configuration editor. In the configuration editor, listeners must be set separately for Android and iOS.

- If you need to configure your game object listener at runtime, use `AppboyBinding.ConfigureListener()` and specify `BrazeUnityMessageType.IN_APP_MESSAGE`.

## Parsing In-App Messages

Incoming `string` messages received in your in-app message game object callback can be parsed into our pre-supplied model objects for convenience.

Use `InAppMessageFactory.BuildInAppMessage()` to parse your in-app message. The resulting object will either be an instance of [`IInAppMessage.cs`][13] or [`IInAppMessageImmersive.cs`][12] depending on its type.

### Example In-App Message Callback

```csharp
// Automatically logs a button click, if present.
void InAppMessageReceivedCallback(string message) {
  IInAppMessage inApp = InAppMessageFactory.BuildInAppMessage(message);
  if (inApp is IInAppMessageImmersive) {
    IInAppMessageImmersive inAppImmersive = inApp as IInAppMessageImmersive;
    if (inAppImmersive.Buttons != null && inAppImmersive.Buttons.Count > 0) {
      inAppImmersive.LogButtonClicked(inAppImmersive.Buttons[0].ButtonID);
    }
  }
}
```

## Analytics

Clicks and impressions must be manually logged for in-app messages not displayed directly by Braze.

Use `LogClicked()` and `LogImpression()` on [`IInAppMessage`][13] to log clicks and impressions on your message.

Use `LogButtonClicked(int buttonID)` on [`IInAppMessageImmersive`][12] to log button clicks. Note that buttons are represented as lists of[`InAppMessageButton`][8] instances, each of which contains a `ButtonID`.

[8]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/models/InAppMessage/InAppMessageButton.cs
[12]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/models/InAppMessage/IInAppMessageImmersive.cs
[13]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/models/InAppMessage/IInAppMessage.cs

---
nav_title: Customization
platform: iOS
page_order: 1
description: "This reference article covers in-app messaging customization options for your iOS application."
channel:
  - in-app messages

---

# Customization {#in-app-message-customization}

All of Braze's in-app message types are highly customizable across messages, images, [Font Awesome][26] icons, click actions, analytics, editable styling, custom display options, and custom delivery options. Multiple options can be configured on a per in-app message basis from [within the dashboard][13]. Braze additionally provides multiple levels of advanced customization to satisfy a variety of use cases and needs.

{% alert important %}
By default, in-app messages are enabled after completing the standard SDK integration, including GIF support.
<br><br>
__Note that integration of `SDWebImage` is required if you plan on using our Braze UI for displaying images__ within iOS In-App Messages, News Feed, or Content Cards.
{% endalert %}

## Key Value Pair Extras

`ABKInAppMessage` objects may carry key-value pairs as `extras`. These are specified on the dashboard when creating a campaign. Key-value pairs can be used to send data down along with an in-app message for further handling by your app.

## Setting Delegates

In-app message display and delivery customizations can be accomplished in code by setting our optional delegates.

### In-App Message Delegate

The [`ABKInAppMessageUIDelegate`][34] delegate can be used to receive triggered in-app message payloads for further processing, receive display lifecycle events, and control display timing. 

Set your `ABKInAppMessageUIDelegate` delegate object on the Braze instance by calling:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance].inAppMessageController.inAppMessageUIController setInAppMessageUIDelegate:self];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.inAppMessageController.inAppMessageUIController.setInAppMessageUIDelegate(self);
```

{% endtab %}
{% endtabs %}

See our [in-app message sample app][35] for an example. Note that if you are not including Braze's UI library in your project (uncommon), this delegate is unavailable.

### Core In-App Message Delegate

If you are not including Braze's UI library in your project and would like to receive triggered in-app message payloads for further processing or custom display in your app, implement the [`ABKInAppMessageControllerDelegate`][16] protocol.

Set your `ABKInAppMessageControllerDelegate` delegate object on the Braze instance by calling:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[Appboy sharedInstance].inAppMessageController.delegate = self;
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.inAppMessageController.delegate = self
```

{% endtab %}
{% endtabs %}

You can alternatively set your core in-app message delegate at initialization time via `appboyOptions` using the key `ABKInAppMessageControllerDelegateKey`.
{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[Appboy startWithApiKey:@"YOUR-API_KEY"
          inApplication:application
      withLaunchOptions:options
      withAppboyOptions:@{ ABKInAppMessageControllerDelegateKey : self }];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.start(withApiKey: "YOUR-API-KEY",
                 in:application,
                 withLaunchOptions:launchOptions,
                 withAppboyOptions:[ ABKInAppMessageControllerDelegateKey : self ])
```
{% endtab %}
{% endtabs %}

## Customizing Orientation

### Setting Orientation For All In-App Messages

To set a fixed orientation for all in-app messages, you can set the `supportedOrientationMask` property on `ABKInAppMessageUIController`. Add the following code after your app's call to `startWithApiKey:inApplication:withLaunchOptions:`:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Set fixed in-app message orientation to portrait.
// Use UIInterfaceOrientationMaskLandscape to display in-app messages in landscape
id<ABKInAppMessageUIControlling> inAppMessageUIController = [Appboy sharedInstance].inAppMessageController.inAppMessageUIController;
((ABKInAppMessageUIController *)inAppMessageUIController).supportedOrientationMask = UIInterfaceOrientationMaskPortrait;
```

{% endtab %}
{% tab swift %}

```swift
// Set fixed in-app message orientation to portrait
// Use .landscape to display in-app messages in landscape
if let controller = Appboy.sharedInstance()?.inAppMessageController.inAppMessageUIController as? ABKInAppMessageUIController {
  controller.supportedOrientationMask = .portrait
}
```

{% endtab %}
{% endtabs %}

Following this, all in-app messages will be displayed in the supported orientation, regardless of device orientation. Please note that the device orientation must also be supported by the in-app message's `orientation` property in order for the message to display. For more information, see the section below.

### Setting Orientation Per In-App Message

You may alternatively set orientation on a per-message basis. To do this, [set an in-app message delegate][23]. Then, in your `beforeInAppMessageDisplayed:` delegate method, set the `orientation` property on the `ABKInAppMessage`. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Set inAppMessage orientation to portrait
inAppMessage.orientation = ABKInAppMessageOrientationPortrait;

// Set inAppMessage orientation to landscape
inAppMessage.orientation = ABKInAppMessageOrientationLandscape;
```

{% endtab %}
{% tab swift %}

```swift    
  // Set inAppMessage orientation to portrait
  inAppMessage.orientation = ABKInAppMessageOrientation.portrait

  // Set inAppMessage orientation to landscape
  inAppMessage.orientation = ABKInAppMessageOrientation.landscape
```

{% endtab %}
{% endtabs %}

In-app messages will not display if the device orientation does not match the `orientation` property on the in-app message.

For *iPads*, in-app messages will appear in the style of the user's preferred orientation regardless of actual screen orientation.

## Custom Handling In-App Message Display

When the [`ABKInAppMessageControllerDelegate`][16] is set, the following delegate method will be called before in-app messages are displayed:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (ABKInAppMessageDisplayChoice) beforeInAppMessageDisplayed:(ABKInAppMessage *)inAppMessage;
```

{% endtab %}
{% tab swift %}

```swift
func beforeInAppMessageDisplayed(inAppMessage: ABKInAppMessage!) -> ABKInAppMessageDisplayChoice
```

{% endtab %}
{% endtabs %}

If you have only implemented [`ABKInAppMessageUIDelegate`][34], the following UI delegate method will be called instead:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (ABKInAppMessageDisplayChoice) beforeInAppMessageDisplayed:(ABKInAppMessage *)inAppMessage withKeyboardIsUp:(BOOL)keyboardIsUp;
```

{% endtab %}
{% tab swift %}

```swift
func beforeInAppMessageDisplayed(inAppMessage: ABKInAppMessage!, withKeyboardIsUp keyboardIsUp: Bool) -> ABKInAppMessageDisplayChoice
```

{% endtab %}
{% endtabs %}

You can customize in-app message handling by implementing this delegate method and returning one of the following values for `ABKInAppMessageDisplayChoice`:

| `ABKInAppMessageDisplayChoice` | Behavior |
| -------------------------- | -------- |
| `ABKDisplayInAppMessageNow` | The message will be displayed immediately |
| `ABKDisplayInAppMessageLater` | The message will be not be displayed and will be placed back on the top of the stack |
| `ABKDiscardInAppMessage` | The message will be discarded and will not be displayed |

You can use the `beforeInAppMessageDisplayed:` delegate method to add in-app message display logic, customize in-app messages before Braze displays them, or opt-out of Braze's in-app message display logic and UI entirely.

For an implementation example, see our [In-App Message Sample Application][36].

### Overriding In-App Messages Before Display

If you would like to alter the display behavior of in-app messages, you should add any necessary display logic to your `beforeInAppMessageDisplayed:` delegate method. For example, you might want to display the in-app message from the top of the screen if the keyboard is currently being displayed, or take the in-app message data model and display the in-app message yourself.

If the IAM campaign is not displaying when the session has been started, make sure you have the necessary display logic added to your `beforeInAppMessageDisplayed:` delegate method. This allows the IAM campaign to display from the top of the screen even if the keyboard is being displayed.

### Hiding the Status Bar During Display

For `Full` and `HTML` in-app messages, the SDK will attempt to place the message over the status bar by default. However, in some cases the status bar may still appear on top of the in-app message. As of version [3.21.1 of the iOS SDK](https://github.com/Appboy/appboy-ios-sdk/blob/master/CHANGELOG.md#3211), you can force the status bar to hide when displaying `Full` and `HTML` in-app messages by setting `ABKInAppMessageHideStatusBarKey` to `YES` within the `appboyOptions` passed to `startWithApiKey:`.

### Logging Impressions and Clicks

Logging in-app message impressions and clicks is not automatic when you implement completely custom handling (*i.e.* if you circumvent Braze's in-app message display by returning `ABKDiscardInAppMessage` in your `beforeInAppMessageDisplayed:`). If you choose to implement your own UI using our in-app message models, you must log analytics with the following methods on the `ABKInAppMessage` class:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Registers that a user has viewed an in-app message with the Braze server.
- (void) logInAppMessageImpression;
// Registers that a user has clicked on an in-app message with the Braze server.
- (void) logInAppMessageClicked;
```

{% endtab %}
{% tab swift %}

```swift
// Registers that a user has viewed a in-app message with the Braze server.
func logInAppMessageImpression()
// Registers that a user has clicked on a in-app message with the Braze server.
func logInAppMessageClicked()
```

{% endtab %}
{% endtabs %}

Furthermore, you should be logging button clicks on subclasses of `ABKInAppMessageImmersive` (*i.e*., `Modal` and `Full` in-app messages):

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
// Logs button click analytics
- (void)logInAppMessageClickedWithButtonID:(NSInteger)buttonID;
```

{% endtab %}
{% tab swift %}

```swift
// Logs button click analytics
func logInAppMessageClickedWithButtonID(buttonId: NSInteger)
```

{% endtab %}
{% endtabs %}

## Customizing In-App Message Behavior on Click
The `inAppMessageClickActionType` property on the `ABKInAppMessage` defines the action behavior after the in-app message is clicked. This property is read-only. If you want to change the in-app message's click behavior, you can call the following method on `ABKInAppMessage`:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[inAppMessage setInAppMessageClickAction:clickActionType withURI:uri];
```

{% endtab %}
{% tab swift %}

```swift
inAppMessage.setInAppMessageClickAction(clickActionType: clickActionType, withURI: uri)
```

{% endtab %}
{% endtabs %}

The `inAppMessageClickActionType` can be set to one of the following values:

| `ABKInAppMessageClickActionType` | On-Click Behavior |
| -------------------------- | -------- |
| `ABKInAppMessageDisplayNewsFeed` | The News Feed will be displayed when the message is clicked, and the message will be dismissed. **Note**: The `uri` parameter will be ignored, and the `uri` property on the `ABKInAppMessage` will be set to nil. |
| `ABKInAppMessageRedirectToURI` | The given URI will be displayed when the message is clicked, and the message will be dismissed. **Note**: The `uri` parameter cannot be nil. |
| `ABKInAppMessageNoneClickAction` | The message will be dismissed when clicked. **Note**: The `uri` parameter will be ignored, and the `uri` property on the `ABKInAppMessage` will be set to nil. |

### Customizing In-App Message Body Clicks

The following [`ABKInAppMessageUIDelegate`][34] delegate method is called when an in-app message is clicked:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (BOOL) onInAppMessageClicked:(ABKInAppMessage *)inAppMessage;
```

{% endtab %}
{% tab swift %}

```swift
func onInAppMessageClicked(inAppMessage: ABKInAppMessage!) -> Bool
```

{% endtab %}
{% endtabs %}

### Customizing In-App Message Button Clicks

For clicks on in-app message buttons and HTML in-app message buttons (*i.e.*, links), [`ABKInAppMessageUIDelegate`][34] includes the following delegate methods:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (BOOL)onInAppMessageButtonClicked:(ABKInAppMessageImmersive *)inAppMessage
                             button:(ABKInAppMessageButton *)button;

- (BOOL)onInAppMessageHTMLButtonClicked:(ABKInAppMessageHTML *)inAppMessage
                             clickedURL:(nullable NSURL *)clickedURL
                               buttonID:(NSString *)buttonID;
```

{% endtab %}
{% tab swift %}

```swift
func onInAppMessageButtonClicked(inAppMessage: ABKInAppMessageImmersive!,
                                 button: ABKInAppMessageButton) -> Bool

func onInAppMessageHTMLButtonClicked(inAppMessage: ABKInAppMessageHTML!,
                                     clickedURL: URL, buttonID: String) -> Bool
```

{% endtab %}
{% endtabs %}

Each method returns a `BOOL` value to indicate if Braze should continue to execute the click action.

To access the click action type of a button in a delegate method, you can use the following code:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
if ([inAppMessage isKindOfClass:[ABKInAppMessageImmersive class]]) {
      ABKInAppMessageImmersive *immersiveIAM = (ABKInAppMessageImmersive *)inAppMessage;
      NSArray<ABKInAppMessageButton *> *buttons = immersiveIAM.buttons;
      for (ABKInAppMessageButton *button in buttons) {
         // Button action type is accessible via button.buttonClickActionType
      }
   }
```

{% endtab %}
{% tab swift %}

```swift
if inAppMessage is ABKInAppMessageImmersive {
      let immersiveIAM = inAppMessage as! ABKInAppMessageImmersive;
      for button in inAppMessage.buttons as! [ABKInAppMessageButton]{
        // Button action type is accessible via button.buttonClickActionType
      }
    }
```

{% endtab %}
{% endtabs %}

>  When an in-app message has buttons, the only click actions that will be executed are the ones on the ABKInAppMessageButton model. The in-app message body will not be clickable even though the ABKInAppMessage model will have the default click action ("News Feed") assigned.

## Dismiss Modal on Outside Tap

The default value is `NO`. This determines if the modal in-app message will be dismissed when the user taps outside of the in-app message.

To enable outside tap dismissals, add a dictionary named `Braze` to your `Info.plist` file. Inside the `Braze` Dictionary add the `DismissModalOnOutsideTap` boolean subentry and set the value to `YES`. Note that prior to Braze iOS SDK v4.0.2, the dictionary key `Appboy` must be used in place of `Braze`.

Example `Info.plist` contents:

```
<key>Braze</key>
<dict>
	<key>DismissModalOnOutsideTap</key>
	<boolean>YES</boolean>
</dict>
```

### Description of Dismiss Modal on Outside Tap

| DismissModalOnOutsideTap | Description |
|----------|-------------|
| YES       | Modal in-app messages will be dismissed on outside tap     |
| NO        | Default, modal in-app messages will not be dismissed on outside tap |
{: .reset-td-br-1 .reset-td-br-2}

## Display In-App Messages In a Custom View Controller

In-app messages can also be displayed within a custom view controller which you pass to Braze. Braze will animate the customized in-app message in and out, as well as handle analytics of the in-app message. The view controller must meet the following requirements:

- It must be a subclass or an instance of `ABKInAppMessageViewController`.
- The view of the returned view controller should be an instance of `ABKInAppMessageView` or its subclass.

The following UI delegate method is called every time an in-app message is offered to `ABKInAppMessageViewController` to allow the app would like to pass a custom view controller to Braze for in-app message display:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (ABKInAppMessageViewController *)inAppMessageViewControllerWithInAppMessage:(ABKInAppMessage *)inAppMessage;
```

{% endtab %}
{% tab swift %}

```swift
func inAppMessageViewControllerWithInAppMessage(inAppMessage: ABKInAppMessage!) -> ABKInAppMessageViewController!
```

{% endtab %}
{% endtabs %}

All of our in-app message view controllers are open-sourced. You can use subclasses or categories to customize the display or behavior of in-app messages.

See the [in-app message view controllers][37] for more details.

## Custom In-App Message Triggering

By default, in-app messages are triggered by event types that are logged by the SDK. If you would like to trigger in-app messages by server-sent events you are also able to achieve this.

To enable this feature you would send a silent push to the device which allows the device to log an SDK based event. This SDK event would subsequently trigger the user-facing in-app message.

### Step 1: Handle Silent Push and Key-Value Pairs
Add the following code within the `application(_:didReceiveRemoteNotification:fetchCompletionHandler:)` method:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
- (void)handleExtrasFromPush:(NSDictionary *)userInfo {
  NSLog(@"A push was received.");
  if (userInfo !=nil && userInfo[@"IS_SERVER_EVENT"] !=nil && userInfo[@"CAMPAIGN_NAME"]!=nil) {
    [[Appboy sharedInstance] logCustomEvent:@"IAM Trigger" withProperties:@{@"campaign_name": userInfo[@"CAMPAIGN_NAME"]}];
  }
 };
```

{% endtab %}
{% tab swift %}

```swift
func handleExtras(userInfo: [AnyHashable : Any]) {
  NSLog("A push was received");
  if userInfo != nil && (userInfo["IS_SERVER_EVENT"] as? String) != nil && (userInfo["CAMPAIGN_NAME"] as? String) != nil {
    Appboy.sharedInstance()?.logCustomEvent("IAM Trigger", withProperties: ["campaign_name": userInfo["CAMPAIGN_NAME"]])
  }
}
```

{% endtab %}
{% endtabs %}

When the silent push is received an SDK recorded event "In-App Message Trigger" will be logged against the user profile. Note that these In-App Messages will only trigger if the silent push is received while the application is in the foreground.

### Step 2: Create a Push Campaign

Create a silent push campaign which is triggered via the server sent event. For details on how to create a silent push campaign please review this section of our [Docs][39].

![serverEventTrigger][40]

The push campaign must include key-value pair extras which indicate that this push campaign is sent with the intention to log an SDK custom event. This event will be used to trigger the in-app message:

![IAMSilentPush][41]

The code within the `application(_:didReceiveRemoteNotification:fetchCompletionHandler:)` method checks for key `IS_SERVER_EVENT` and will log an SDK custom event if this is present.

You can alter either the event name or event properties by sending the desired value within the key-value pair extras of the push payload. These extras can be used as the parameter of either the event name or as an event property when logging the custom event.

### Step 3: Create an In-App Message Campaign

Create your user visible in-app message campaign from within Braze’s dashboard. This campaign should have an Action Based delivery, and be triggered from the custom event logged from within the `application(_:didReceiveRemoteNotification:fetchCompletionHandler:)` method.

In the example below the specific in-app message to be trigger has been configured by sending the event property as part of the initial silent push.

![IAMPushTrigger][42]

>  Due to a push message being used to record an SDK logged custom event, Braze will need to store a push token for each user to enable this solution. For iOS users, Braze will only store a token from the point that a user has been served the OS's push prompt. Before this, the user will not be reachable using push and the above solution will not be possible.

## Method Declarations

For additional information see the following header files:

- [`ABKInAppMessage.h`][14]
- [`ABKInAppMessageController.h`][15]
- [`ABKInAppMessageControllerDelegate.h`][16]

## Implementation Samples

See [`AppDelegate.m`][36], [`ViewController.m`][35] and [`CustomInAppMessageViewController.m`][19] in the in-app message sample app.

[13]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/#creating-an-in-app-message
[14]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKInAppMessage.h
[15]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKInAppMessageController.h
[16]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/include/ABKInAppMessageControllerDelegate.h
[19]: https://github.com/Appboy/appboy-ios-sdk/blob/master/Samples/InAppMessage/BrazeInAppMessageSample/BrazeInAppMessageSample/CustomInAppMessageViewController.m
[23]: #setting-delegates
[26]: http://fortawesome.github.io/Font-Awesome/
[33]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/troubleshooting/#step-2-devices-register-for-apns-and-provide-braze-with-push-tokens
[34]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyUI/ABKInAppMessage/ABKInAppMessageUIDelegate.h
[35]: https://github.com/Appboy/appboy-ios-sdk/blob/master/Samples/InAppMessage/BrazeInAppMessageSample/BrazeInAppMessageSample/ViewController.m
[36]: https://github.com/Appboy/appboy-ios-sdk/blob/master/Samples/InAppMessage/BrazeInAppMessageSample/BrazeInAppMessageSample/AppDelegate.m
[37]: https://github.com/Appboy/appboy-ios-sdk/tree/master/AppboyUI/ABKInAppMessage/ViewControllers
[39]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/push_notifications/silent_push_notifications/
[40]: {% image_buster /assets/img_archive/iosServerSentPush.png %}
[41]: {% image_buster /assets/img_archive/iOSServerPush.png %}
[42]: {% image_buster /assets/img_archive/iosIAMeventTrigger.png %}

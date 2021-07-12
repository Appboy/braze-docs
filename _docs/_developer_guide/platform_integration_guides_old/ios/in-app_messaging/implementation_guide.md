---
nav_title: Advanced Implementation (Optional)
platform: iOS
page_order: 6
description: "This advanced implementation guide covers iOS in-app message code considerations, three use cases built by our team, and accompanying code snippets."
channel:
  - in-app messages
---

{% alert important %}
Looking for the out-of-the-box in-app message developer integration guide? Find it [here]({{site.baseurl}}/developer_guide/platform_integration_guides/ios/in-app_messaging/customization/).
{% endalert %}

# In-App Messaging Implementation Guide

> This optional and advanced implementation guide covers in-app message code considerations, three custom use cases built by our team, and accompanying code snippets. Visit our Braze Demo repository [here](https://github.com/braze-inc/braze-growth-shares-ios-demo-app)! Please note that this implementation guide is centered around a Swift implementation, but Objective-C snippets are provided for those interested.  Looking for HTML implementations? Take a look at our [HTML template repository](https://github.com/braze-inc/in-app-message-templates)!

## Code Considerations

The following guide offers an optional custom developer integration to use in addition to out-of-the-box in-app messages. Custom view controllers are included below with each use case, offering examples to extend functionality and natively customize the look and feel of your in-app messages.

### ABKInAppMessage Subclasses

The code snippet below is a UI delegate method from the Braze SDK that determines what subclass view you want to populate your in-app message with. We cover a basic implementation in this guide and show how the full, slide up and modal subclasses can be implemented in captivating ways. Please note that if you want to set up your custom view controller, you must set up all other in-app message subclasses. Once you have a solid understanding of the concepts behind subclassing, check out our [use cases](#sample-use-cases) below to get started implementing in-app messaging subclasses.

{% tabs %}
{% tab Swift %}
__ABKInAppMessage Subclasses__<br>

```swift
extension AppboyManager: ABKInAppMessageUIDelegate {
  func inAppMessageViewControllerWith(_ inAppMessage: ABKInAppMessage) -> ABKInAppMessageViewController {
    switch inAppMessage {
    case is ABKInAppMessageSlideup:
      return slideupViewController(inAppMessage: inAppMessage) //Custom Method
    case is ABKInAppMessageModal: 
      return modalViewController(inAppMessage: inAppMessage) //Custom Method
    case is ABKInAppMessageFull:
      return fullViewController(inAppMessage: inAppMessage) //Custom Method
    case is ABKInAppMessageHTML:
      return ABKInAppMessageHTMLViewController(inAppMessage: inAppMessage)
    default:
      return ABKInAppMessageViewController(inAppMessage: inAppMessage)
    }
  }
}
```
{% endtab %}
{% tab Objective-C %}
__ABKInAppMessage Subclasses__<br> 

```objc
- (ABKInAppMessageViewController *)inAppMessageViewControllerWithInAppMessage:(ABKInAppMessage *)inAppMessage {
  if ([inAppMessage isKindOfClass:[ABKInAppMessageSlideup class]]) {
    return [self slideupViewControllerWithInAppMessage:inAppMessage]; //Custom Method
  } else if ([inAppMessage isKindOfClass:[ABKInAppMessageModal class]]) {
    return [self modalViewControllerWithInAppMessage:inAppMessage]; //Custom Method
  } else if ([inAppMessage isKindOfClass:[ABKInAppMessageFull class]]) {
    return [self fullViewControllerWithInAppMessage:inAppMessage]; //Custom Method
  } else if ([inAppMessage isKindOfClass:[ABKInAppMessageHTML class]]) {
    return [[ABKInAppMessageHTMLViewController alloc] initWithInAppMessage:inAppMessage];
  } else {
    return [[ABKInAppMessageViewController alloc] initWithInAppMessage:inAppMessage];
  }
}
```
{% endtab %}
{% endtabs %}

## Sample Use Cases

There are three sample customer use cases provided. Each sample has video walkthroughs, code snippets, and a look into how in-app messages may look and be used in the Braze dashboard:
- [Custom Slideup In-App Message](#custom-slideup-in-app-message)
- [Custom Modal In-App Message](#custom-modal-in-app-message)
- [Custom Full In-App Message](#custom-full-in-app-message)

### Custom Slideup In-App Message

While building out your slide-up in-app message, you may notice you aren't able to modify the placement of the message. While this option is not explicitly offered out-of-the-box, modification like this is made possible by subclassing the `ABKInAppMessageSlideupViewController` and overriding the `slideConstraint` value with your own custom constraint value. Visit the [SlideFromBottomViewController](https://github.com/braze-inc/braze-growth-shares-ios-demo-app/blob/master/Braze-Demo/ViewController/In-App-Messages/SlideFromBottomViewController.swift) to get started.

{% include video.html id="j6dvNSbK2-8" align="center" %}

#### __Adding Additional Behavior to our Default UI__<br><br>

{% tabs %}
{% tab Swift %}
__Override and Set Custom Constraint__<br>
Override `beforeMoveInAppMessageViewOnScreen()` and set your own custom constraint value to suit your needs. The original value is set in the superclass.

```swift
override func beforeMoveInAppMessageViewOnScreen() {
  super.beforeMoveInAppMessageViewOnScreen()
  setOffset()
}
```

{% details Version 3.34.0 or earlier %}
```swift
override func beforeMoveInAppMessageViewOnScreen() {
  setSlideConstraint()
}
```
{% enddetails %}

{% endtab %}
{% tab Objective-C %}
__Override and Set Custom Constraint__<br> 
Override `beforeMoveInAppMessageViewOnScreen()` and set your own custom constraint value to suit your needs. The original value is set in the superclass.

```objc
- (void)beforeMoveInAppMessageViewOnScreen {
  [super beforeMoveInAppMessageViewOnScreen];
  [self setOffset];
}
```

{% details Version 3.34.0 or earlier  %}
```objc
- (void)beforeMoveInAppMessageViewOnScreen {
  [self setSlideConstraint:self.slideConstraint];
}
```
{% enddetails %}
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Swift %}
__Update `offset` Variable__<br>
Update the `offset` variable and set your own offset to suit your needs.
```swift
func setSlideConstraint() {
  offset = 0
}
```

```swift
override var offset: CGFloat {
  get {
    return super.offset
  }
  set {
    super.offset = newValue + adjustedOffset
  }
}
```

{% details Version 3.34.0 or earlier  %}
__Update `slideConstraint` Variable__<br>
The `slideConstraint` public variable comes from the superclass `ABKInAppMessageSlideupViewController`. 

```swift
func setSlideConstraint() {
    slideConstraint?.constant = bottomSpacing
}
```

```swift
private var bottomSpacing: CGFloat {
    return AppboyManager.shared.activeApplicationViewController.topMostViewController().view.safeAreaInsets.bottom
}
``` 
Visit the Braze Demo repository for the [`topMostViewController()`](https://github.com/braze-inc/braze-growth-shares-ios-demo-app/blob/master/Braze-Demo/Utils/UIViewController_Util.swift#L17) function referenced above.
{% enddetails %}
{% endtab %}
{% tab Objective-C %}
__Update `offset` Variable__<br>
Update the `offset` variable and set your own offset to suit your needs.
```objc
- (void)setOffset {
  self.offset = 0;
}
```

```objc
- (CGFloat)offset {
  return [super offset];
}
 
- (void)setOffset:(CGFloat)offset {
  [super setOffset:offset + [self adjustedOffset]];
}
```
{% details Version 3.34.0 or earlier  %}
__Update `slideConstraint` Variable__<br>
The `slideConstraint` public variable comes from the superclass `ABKInAppMessageSlideupViewController`. 

```objc
- (void)self.setSlideConstraint:(NSLayoutConstraint *)slideConstraint {
  slideConstraint.constant = bottomSpacing;
}
```

```objc
- (CGFloat)bottomSpacing {
  return [AppboyManager shared].activeApplicationViewController.topMostViewController.view.safeAreaInsets.bottom;
}
```
{% enddetails %}
{% endtab %}
{% endtabs %}

__Adjust Constraint for Device Orientation__<br>
Adjust the respective value in `viewWillTransition()` because the subclass assumes responsibility for keeping the constraint synced during layout changes.

### Custom Modal In-App Message

An `ABKInAppMessageModalViewController` can be subclassed to leverage a `UIPickerView` offering engaging ways to collect valuable user attributes. The example below shows how you can use Connected Content to capture custom attributes from a dynamic list of items. Visit the [ModalPickerViewController](https://github.com/braze-inc/braze-growth-shares-ios-demo-app/blob/master/Braze-Demo/ViewController/In-App-Messages/ModalPickerViewController/ModalPickerViewController.swift) to get started.

{% include video.html id="FhRCxkLRr3M" align="center" %}

{% tabs %}
{% tab Swift %}
__Using `view_type` for UI Display Behavior__<br>
The `ABKInAppMessage` object has an `extras` dictionary that we can query to find the `view_type` key (if any) and display the correct type of view. It’s important to note that in-app messages are configured on a per-message basis, so custom and out-of-the-box modal views can work harmoniously.

```swift
func modalViewController(inAppMessage: ABKInAppMessage) -> ABKInAppMessageModalViewController {
  switch inAppMessage.extras?[InAppMessageKey.viewType.rawValue] as? String {
  case InAppMessageViewType.picker.rawValue:
    return ModalPickerViewController(inAppMessage: inAppMessage)
  default:
    return ABKInAppMessageModalViewController(inAppMessage: inAppMessage)
  }
}
```
{% endtab %}
{% tab Objective-C %}
__Using `view_type` for UI Display Behavior__<br>
The `ABKInAppMessage` object has an `extras` dictionary that we can query to find the `view_type` key (if any) and display the correct type of view. It’s important to note that in-app messages are configured on a per-message basis, so custom and out-of-the-box modal views can work harmoniously.

```objc
- (ABKInAppMessageModalViewController *)modalViewControllerWithInAppMessage:(ABKInAppMessage *)inAppMessage {
  InAppMessageData *inAppMessageData = [[InAppMessageData alloc] init];
  NSString *key = [inAppMessageData rawValueForInAppMessageKey:InAppMessageKeyViewType];
  NSString *viewType = [inAppMessageData rawValueForInAppMessageViewType:InAppMessageViewTypePicker];
   
  if ([inAppMessage.extras objectForKey:key] && [inAppMessage.extras[key] isEqualToString:viewType]) {
    return [[ModalViewController alloc] initWithInAppMessage:inAppMessage];
  } else {
    return [[ABKInAppMessageModalViewController alloc] initWithInAppMessage:inAppMessage];
  }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Swift %}
__Override and Provide Custom View__<br>
Override `loadView()` and set your own custom view to suit your needs.
```swift
override var nibname: String{
  return "ModalPickerViewController"
}

override func loadView() {
  Bundle.main.loadNibNamed(nibName, owner: self, options: nil)
}
```
{% endtab %}
{% tab Objective-C %}
__Override and Provide Custom View__<br>
Override `loadView()` and set your own custom view to suit your needs.
```objc
- (void)loadView {
  NSString *nibName = @"ModalPickerViewController";
  [[NSBundle mainBundle] loadNibNamed:nibName owner:self options:nil];
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Swift %}
__Format Variables for a Dynamic List__<br>
Before reloading the `UIPickerView` components, the `inAppMessage` message variable is output as a _String_. This message must be formatted as an array of items to be displayed correctly. As an example, this can be achieved using [`components(separatedBy: ", ")`](https://developer.apple.com/documentation/foundation/nsstring/1413214-components).
```swift
override func viewDidLoad() {
  super.viewDidLoad()
 
  items = inAppMessage.message.separatedByCommaSpaceValue
  pickerView.reloadAllComponents()
}
```
{% endtab %}
{% tab Objective-C %}
__Format Variables for PickerView__<br>
Before reloading the `UIPickerView` components, the `inAppMessage` message variable is output as a _String_. This message must be formatted as an array of items to be displayed correctly. As an example, this can be achieved using [`componentsSeparatedByString`](https://developer.apple.com/documentation/foundation/nsstring/1413214-componentsseparatedbystring?language=objc).
```objc
- (void)viewDidLoad {
  [super viewDidLoad];
   
  self.items = [[NSArray alloc] initWithArray:[self.inAppMessage.message componentsSeparatedByString:@", "]];
  [self.pickerView reloadAllComponents];
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab Swift %}
__Assign Custom Attribute__<br>
Using the subclass, after a user presses submit, pass the attribute with its corresponding selected value to Braze.
```swift
@IBAction func primaryButtonTapped(_ sender: Any) {
  guard let item = selectedItem, !item.isEmpty, let attributeKey = inAppMessage.extras?[InAppMessageKey.attributeKey.rawValue] as? String else { return }
     
  AppboyManager.shared.setCustomAttributeWithKey(attributeKey, andStringValue: item)
}
```
{% endtab %}
{% tab Objective-C %}
__Assign Custom Attribute__<br>
Using the subclass, after a user presses submit, pass the attribute with its corresponding selected value to Braze.
```objc
- (IBAction)primaryButtonTapped:(id)sender {
  InAppMessageData *inAppMessageData = [[InAppMessageData alloc] init];
  NSString *key = [inAppMessageData rawValueForInAppMessageKey:InAppMessageKeyAttributeKey];
   
  if (self.selectedItem.length > 0 && [self.inAppMessage.extras objectForKey:key]) {
    [[AppboyManager shared] setCustomAttributeWithKey:self.inAppMessage.extras[key] andStringValue:self.selectedItem];
  }
}
```
{% endtab %}
{% endtabs %}

### Custom Full In-App Message

Use custom full in-app messages to create interactive, user-friendly prompts to collect valuable customer data. The example below shows an implementation of the custom full in-app message reimagined as an interactive push primer with notification preferences. Visit the [FullListViewController](https://github.com/braze-inc/braze-growth-shares-ios-demo-app/blob/master/Braze-Demo/ViewController/In-App-Messages/FullListViewController/FullListViewController.swift) to get started.

{% include video.html id="_P-LNHpXI88" align="center" %}

#### Intercepting In-App Message Touches
![Touches][1]{: style="float:right;max-width:30%;margin-left:10px;border:0"}
Intercepting in-app message touches is crucial in making the custom full in-app message buttons function correctly. By default, the `ABKInAppMessageImmersive` adds a tap gesture recognizer onto the message so users are able to dismiss messages without buttons. Through the use of adding a `UISwitch` or button to the `UITableViewCell` view hierarchy, the touches now get handled by our custom view. As of iOS 6, buttons and other controls have precedence when working with gesture recognizers, making our custom full in-app message work as it should. 

[1]: {% image_buster /assets/img/iam_implementation_guide.png %}

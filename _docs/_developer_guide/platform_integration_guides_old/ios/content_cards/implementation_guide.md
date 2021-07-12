---
nav_title: Advanced Implementation (Optional)
platform: iOS
page_order: 7
description: "This advanced implementation guide covers iOS Content Card code considerations, three use cases built by our team, accompanying code snippets, and guidance on logging impressions, clicks, and dismissals."
channel:
  - content cards

---

{% alert important %}
Looking for the out-of-the-box Content Card developer integration guide? Find it [here]({{site.baseurl}}/developer_guide/platform_integration_guides/ios/content_cards/data_model/).
{% endalert %}

# Content Card Implementation Guide

> This optional and advanced implementation guide covers Content Card code considerations, three custom use cases built by our team, accompanying code snippets, and guidance on logging impressions, clicks, and dismissals. Visit our Braze Demo Repository [here](https://github.com/braze-inc/braze-growth-shares-ios-demo-app)! Please note that this implementation guide is centered around a Swift implementation, but Objective-C snippets are provided for those interested.

## Code Considerations

### Import Statements and Helper Files

When working with custom implementation Content Cards, it is recommended to integrate them using a single `import Appboy-iOS-SDK` statement with the use of a helper file. This approach limits issues that arise from excessive SDK imports, making it easier to track, debug, and alter code. An example helper file can be found [here](https://github.com/braze-inc/braze-growth-shares-ios-demo-app/blob/master/Braze-Demo/BrazeManager.swift).

### Content Cards as Custom Objects

Much like a rocketship adding a booster, your own custom objects can be extended to function as Content Cards. Limited API surfaces such as this provide flexibility to work with different data backends interchangeably. This can be done by conforming to the `ContentCardable` protocol and implementing the initializer (as seen below) and, through the use of the `ContentCardData` struct, allows you to access the `ABKContentCard` data. The `ABKContentCard` payload will be used to initialize the `ContentCardData` struct and the custom object itself, all from a `Dictionary` type via the initializer the protocol comes with.

The initializer also includes a `ContentCardClassType` enum. This enum is used to decide which object to initialize. Through the use of key-value pairs within the Braze dashboard, you can set an explicit `class_type` key that will be used to determine what object to initialize. These key-value pairs for Content Cards come through in the `extras` variable on the `ABKContentCard`. Another core component of the initializer is the `metaData` dictionary parameter. The `metaData` includes everything from the `ABKContentCard` parsed out into a series of keys and values. Once the relevant cards are parsed and converted to your custom objects, the app is ready to begin working with them as if they were instantiated from JSON or any other source. 

Once you have a solid understanding of these code considerations, check out our [use cases](#sample-use-cases) below to get started implementing your custom objects.

{% tabs local %}
{% tab ContentCardable %}
{% subtabs global %}
{% subtab Swift %}
__ContentCardable Protocol__<br>
A `ContentCardData` object that represents the `ABKContentCard` data along with a `ContentCardClassType` enum. An initializer used to instantiate custom objects with `ABKContentCard` metadata.
```swift
protocol ContentCardable {
  var contentCardData: ContentCardData? { get }
  init?(metaData: [ContentCardKey: Any], classType contentCardClassType: ContentCardClassType)
}
 
extension ContentCardable {
  var isContentCard: Bool {
    return contentCardData != nil
  }
   
  func logContentCardClicked() {
    BrazeManager.shared.logContentCardClicked(idString: contentCardData?.contentCardId)
  }
   
  func logContentCardDismissed() {
    BrazeManager.shared.logContentCardDismissed(idString: contentCardData?.contentCardId)
  }
   
  func logContentCardImpression() {
    BrazeManager.shared.logContentCardImpression(idString: contentCardData?.contentCardId)
  }
}
```
__Content Card Data Struct__<br>
`ContentCardData` represents the parsed out values of an `ABKContentCard`.

```swift
struct ContentCardData: Hashable {
  let contentCardId: String
  let contentCardClassType: ContentCardClassType
  let createdAt: Double
  let isDismissable: Bool
  ...
  // other Content Card properties such as expiresAt, pinned, etc.
}
 
extension ContentCardData: Equatable {
  static func ==(lhs: ContentCardData, rhs: ContentCardData) -> Bool {
    return lhs.contentCardId == rhs.contentCardId
  }
}
```
{% endsubtab %}
{% subtab Objective-C %}
__ContentCardable Protocol__<br>
A `ContentCardData` object that represents the `ABKContentCard` data along with a `ContentCardClassType` enum, an initializer used to instantiate custom objects with `ABKContentCard` metadata.
```objc
@protocol ContentCardable <NSObject>
 
@property (nonatomic, strong) ContentCardData *contentCardData;
- (instancetype __nullable)initWithMetaData:(NSDictionary *)metaData
                                  classType:(enum ContentCardClassType)classType;
 
- (BOOL)isContentCard;
- (void)logContentCardImpression;
- (void)logContentCardClicked;
- (void)logContentCardDismissed;
 
@end
```
__Content Card Data Struct__<br>
`ContentCardData` represents the parsed out values of an `ABKContentCard`.

```objc
@interface ContentCardData : NSObject
 
+ (ContentCardClassType)contentCardClassTypeForString:(NSString *)rawValue;
 
- (instancetype)initWithIdString:(NSString *)idString
                       classType:(ContentCardClassType)classType
                       createdAt:(double)createdAt isDismissible:(BOOL)isDismissible;
 
@property (nonatomic, readonly) NSString *contentCardId;
@property (nonatomic) ContentCardClassType classType;
@property (nonatomic, readonly) double *createdAt;
@property (nonatomic, readonly) BOOL isDismissible;
...
// other Content Card properties such as expiresAt, pinned, etc.    
 
@end
```
{% endsubtab %}
{% endsubtabs %}
{% endtab %}
{% tab Custom Objects %}
{% subtabs global %}
{% subtab Swift %}
__Custom Object Initializer__<br>
MetaData from an `ABKContentCard` is used to populate your object's variables. The key-value pairs set up in the Braze Dashboard are represented in the "extras" dictionary.

```swift
extension CustomObject: ContentCardable {
  init?(metaData: [ContentCardKey: Any], classType contentCardClassType: ContentCardClassType) {
    guard let idString = metaData[.idString] as? String,
      let createdAt = metaData[.created] as? Double,
      let isDismissable = metaData[.dismissable] as? Bool,
      let extras = metaData[.extras] as? [AnyHashable: Any],
      else { return nil }
 
    let contentCardData = ContentCardData(contentCardId: idString, contentCardClassType: contentCardClassType, createdAt: createdAt, isDismissable: isDismissable)
    let customObjectProperty = extras["YOUR-CUSTOM-OBJECT-PROPERTY"] as? String
           
    self.init(contentCardData: contentCardData, property: customObjectProperty)
  }
}
```

__Identifying Types__<br>
The `ContentCardClassType` enum represents the `class_type` value in the Braze Dashboard. This value is also used as a filter identifier to display Content Cards in different places. 

```swift
enum ContentCardClassType: Hashable {
  case yourValue
  case yourOtherValue
  ...
  case none
 
  init(rawType: String?) {
    switch rawType?.lowercased() {
    case "your_value": // these values much match the value set in the Braze dashboard
      self = .yourValue
    case "your_other_value": // these values much match the value set in the Braze dashboard
      self = .yourOtherValue
    ...
    default:
      self = .none
    }
  }
}
```
{% endsubtab %}
{% subtab Objective-C %}
__Custom Object Initializer__<br>
MetaData from an `ABKContentCard` is used to populate your object's variables. The key-value pairs set up in the Braze Dashboard are represented in the "extras" dictionary.


```objc
- (id _Nullable)initWithMetaData:(nonnull NSDictionary *)metaData classType:(enum ContentCardClassType)classType {
  self = [super init];
  if (self) {
    if ([metaData objectForKey:ContentCardKeyIdString] && [metaData objectForKey:ContentCardKeyCreated] && [metaData objectForKey:ContentCardKeyDismissible] && [metaData objectForKey:ContentCardKeyExtras]) {
      NSDictionary  *extras = metaData[ContentCardKeyExtras];
      NSString *idString = metaData[ContentCardKeyIdString];
      double createdAt = [metaData[ContentCardKeyCreated] doubleValue];
      BOOL isDismissible = metaData[ContentCardKeyDismissible];
 
      if ([extras objectForKey: @"YOUR-CUSTOM-PROPERTY")
        _customObjectProperty = extras[@"YOUR-CUSTOM-OBJECT-PROPERTY"];
 
      self.contentCardData = [[ContentCardData alloc] initWithIdString:idString classType:classType createdAt:createdAt isDismissible:isDismissible];
 
      return self;
    }
  }
  return nil;
}
```

__Identifying Types__<br>
The `ContentCardClassType` enum represents the `class_type` value in the Braze Dashboard. This value is also used as a filter identifier to display Content Cards in different places. 

```objc
typedef NS_ENUM(NSInteger, ContentCardClassType) {
  ContentCardClassTypeNone = 0,
  ContentCardClassTypeYourValue,
  ContentCardClassTypeYourOtherValue,
  ...
};
 
+ (NSArray *)contentCardClassTypeArray {
  return @[ @"", @"your_value", @"your_other_value" ];
}
 
+ (ContentCardClassType)contentCardClassTypeForString:(NSString*)rawValue {
  if ([[self contentCardClassTypeArray] indexOfObject:rawValue] == NSNotFound) {
    return ContentCardClassTypeNone;
  } else {
    NSInteger value = [[self contentCardClassTypeArray] indexOfObject:rawValue];
    return (ContentCardClassType) value;
  }
}
```
{% endsubtab %}
{% endsubtabs %}
{% endtab %}

{% tab Handling Content Cards %}
{% subtabs global %}
{% subtab Swift %}
__Requesting Content Cards__<br>
As long as the observer is still retained in memory, the notification callback from the Braze SDK can be expected.

```swift
func loadContentCards() {
  BrazeManager.shared.addObserverForContentCards(observer: self, selector: #selector(contentCardsUpdated))
  BrazeManager.shared.requestContentCardsRefresh()
}
```

__Handling the Content Cards SDK Callback__<br>
Forward the notification callback to the helper file to parse the payload data for your custom object(s).
```swift
@objc func contentCardsUpdated(_ notification: Notification) {
  guard let contentCards = BrazeManager.shared.handleContentCardsUpdated(notification, for: [.yourValue]) as? [CustomObject],!contentCards.isEmpty else { return }
 
 // do something with your array of custom objects
}
```

__Working with Content Cards__<br>
The `class_type` is passed in as a filter to only return Content Cards that have a matching `class_type`.

```swift
func handleContentCardsUpdated(_ notification: Notification, for classTypes: [ContentCardClassType]) -> [ContentCardable] {
  guard let updateIsSuccessful = notification.userInfo?[ABKContentCardsProcessedIsSuccessfulKey] as? Bool, updateIsSuccessful, let cards = contentCards else { return [] }
             
  return convertContentCards(cards, for: classTypes)
}
```
{% endsubtab %}
{% subtab Objective-C %}
__Requesting Content Cards__<br>
As long as the observer is still retained in memory, the notification callback from the Braze SDK can be expected.

```objc
- (void)loadContentCards {
  [[BrazeManager shared] addObserverForContentCards:self selector:@selector(contentCardsUpdated:)];
  [[BrazeManager shared] requestContentCardsRefresh];
}
```

__Handling the Content Cards SDK Callback__<br>
Forward the notification callback to the helper file to parse the payload data for your custom object(s).
```objc
- (void)contentCardsUpdated:(NSNotification *)notification {
  NSArray *classTypes = @[@(ContentCardClassTypeYourValue)];
  NSArray *contentCards = [[BrazeManager shared] handleContentCardsUpdated:notification forClassTypes:classTypes];
 
  // do something with your array of custom objects
}
```

__Working with Content Cards__<br>
The `class_type` is passed in as a filter to only return Content Cards that have a matching `class_type`.

```objc
- (NSArray *)handleContentCardsUpdated:(NSNotification *)notification forClassType:(ContentCardClassType)classType {  
  BOOL updateIsSuccessful = [notification.userInfo[ABKContentCardsProcessedIsSuccessfulKey] boolValue];
  if (updateIsSuccessful) {
    return [self convertContentCards:self.contentCards forClassType:classType];
  } else {
    return @[];
  }
}
```
{% endsubtab %}
{% endsubtabs %}
{% endtab %}

{% tab Working with Payload Data %}
{% subtabs global %}
{% subtab Swift %}
__Working with Payload Data__<br>
Loops through the array of Content Cards and only parses the cards with a matching `class_type`. The payload from an ABKContentCard is parsed into a `Dictionary`.

```swift
func convertContentCards(_ cards: [ABKContentCard], for classTypes: [ContentCardClassType]) -> [ContentCardable] {
  var contentCardables: [ContentCardable] = []
    
  for card in cards {
    let classTypeString = card.extras?[ContentCardKey.classType.rawValue] as? String
    let classType = ContentCardClassType(rawType: classTypeString)
    guard classTypes.contains(classType) else { continue }
       
    var metaData: [ContentCardKey: Any] = [:]
    switch card {
    case let banner as ABKBannerContentCard:
      metaData[.image] = banner.image
    case let captioned as ABKCaptionedImageContentCard:
      metaData[.title] = captioned.title
      metaData[.cardDescription] = captioned.cardDescription
      metaData[.image] = captioned.image
    case let classic as ABKClassicContentCard:
      metaData[.title] = classic.title
      metaData[.cardDescription] = classic.cardDescription
    default:
      break
    }
 
    metaData[.idString] = card.idString
    metaData[.created] = card.created
    metaData[.dismissible] = card.dismissible
    metaData[.urlString] = card.urlString
    metaData[.extras] = card.extras
    ...
    // other Content Card properties such as expiresAt, pinned, etc.
      
    if let contentCardable = contentCardable(with: metaData, for: classType) {
      contentCardables.append(contentCardable)
    }
  }
  return contentCardables
}
```

__Initalizing your Custom Objects from Content Card Payload Data__<br>
The `class_type` is used to determine which of your custom objects will be initialized from the payload data.

```swift
func contentCardable(with metaData: [ContentCardKey: Any], for classType: ContentCardClassType) -> ContentCardable? {
  switch classType {
  case .yourValue:
    return CustomObject(metaData: metaData, classType: classType)
  case .yourOtherValue:
    return OtherCustomObject(metaData: metaData, classType: classType)
  ...
  default:
    return nil
  }
}
```
{% endsubtab %}
{% subtab Objective-C %}
__Working with Payload Data__<br>
Loops through the array of Content Cards and only parses the cards with a matching `class_type`. The payload from an ABKContentCard is parsed into a `Dictionary`.

```objc
- (NSArray *)convertContentCards:(NSArray<ABKContentCard*> *)cards forClassType:(ContentCardClassType)classType {
  NSMutableArray *contentCardables = [[NSMutableArray alloc] init];      for (ABKContentCard *card in cards) {
    NSString *classTypeString = [card.extras objectForKey:ContentCardKeyClassType];
    ContentCardClassType cardClassType = [ContentCardData contentCardClassTypeForString: classTypeString];
    if (cardClassType != classType) { continue; }
     
    NSMutableDictionary *metaData = [[NSMutableDictionary alloc] init];
    if ([card isKindOfClass:[ABKBannerContentCard class]]) {
      ABKBannerContentCard *banner = (ABKBannerContentCard *)card;
      metaData[ContentCardKeyImage] = banner.image;
    } else if ([card isKindOfClass:[ABKCaptionedImageContentCard class]]) {
      ABKCaptionedImageContentCard *captioned = (ABKCaptionedImageContentCard *)card;
      metaData[ContentCardKeyTitle] = captioned.title;
      metaData[ContentCardKeyCardDescription] = captioned.cardDescription;
      metaData[ContentCardKeyImage] = captioned.image;
    } else if ([card isKindOfClass:[ABKClassicContentCard class]]) {
      ABKClassicContentCard *classic = (ABKClassicContentCard *)card;
      metaData[ContentCardKeyCardDescription] = classic.title;
      metaData[ContentCardKeyImage] = classic.image;
    }
     
    metaData[ContentCardKeyIdString] = card.idString;
    metaData[ContentCardKeyCreated] = [NSNumber numberWithDouble:card.created];
    metaData[ContentCardKeyDismissible] = [NSNumber numberWithBool:card.dismissible];
    metaData[ContentCardKeyUrlString] = card.urlString;
    metaData[ContentCardKeyExtras] = card.extras;
    ...
    // other Content Card properties such as expiresAt, pinned, etc.   
 
    id<ContentCardable> contentCardable = [self contentCardableWithMetaData:metaData forClassType:classType];
    if (contentCardable) {
      [contentCardables addObject:contentCardable];
    }
  }
 
  return contentCardables;
}
```

__Initalizing your Custom Objects from Content Card Payload Data__<br>
The `class_type` is used to determine which of your custom objects will be initialized from the payload data.

```obj-c
- (id<ContentCardable>)contentCardableWithMetaData:(NSDictionary *)metaData forClassType:(ContentCardClassType)classType {
  switch (classType) {
    case ContentCardClassTypeYourValue:
      return [[CustomObject alloc] initWithMetaData:metaData classType:classType];
    case ContentCardClassTypeYourOtherValue:
      return nil;
    ...
    default:
      return nil;
  }
}
```
{% endsubtab %}
{% endsubtabs %}
{% endtab %}
{% endtabs %}

## Sample Use Cases

There are three sample customer use cases provided. Each use case offers a detailed explanation, relevant code snippets, and a look into how Content Card variables may look and be used in the Braze dashboard:
- [Content Cards As Supplemental Content](#content-cards-as-supplemental-content)
- [Content Cards in a Message Center](#content-cards-in-a-message-center)
- [Interactive Content Cards](#interactive-content-cards)

### Content Cards as Supplemental Content

![Supplementary Content PNG][1]{: style="float:right;max-width:25%;margin-left:15px;border:0;"}

You can seamlessly blend Content Cards into an existing feed, allowing data from multiple feeds to load simultaneously. This creates a cohesive, harmonious experience with Braze Content Cards and existing feed content.

The example to the right shows a `UICollectionView` with a hybrid list of items that are populated via local data and Content Cards powered by Braze. With this, Content Cards can be indistinguishable alongside existing content.

#### Dashboard Configuration

This Content Card is delivered by an API triggered campaign with API triggered key-value pairs. This is ideal for campaigns where the card's values depend on external factors to determine what content to display to the user. Note that `class_type` should be known at set-up time.

![Supplementary Content PNG][2]{: style="max-width:60%;"}

##### Ready to log analytics?
Visit the [following section](#logging-impressions-clicks-and-dismissals) to get a better understanding of how the flow of data should look.

### Content Cards in a Message Center
<br>
Content Cards can be used in a message center format where each message is its own card. Each message in the message center is populated via a Content Card payload, and each card contains additional key-value pairs that power on-click UI/UX. In the example below, one message directs you to an arbitrary custom view, while another opens to a webview that displays custom HTML.

![Message Center PNG][3]{: style="border:0;"}{: style="max-width:80%;border:0"}

#### Dashboard Configuration

For the following message types, the key-value pair `class_type` should be added to your dashboard configuration. The values assigned here are arbitrary but should be distinguishable between class types. These key-value pairs are the key identifiers that the application looks at when deciding where to go when the user clicks on an abridged inbox message. 

| Arbitrary Custom View Message (Full Page) | Webview Message (HTML) |
| ---- | ---- |
| <br>The key-value pairs for this use case include:<br><br>- `message_header` set as `Full Page`<br>- `class_type` set as `message_full_page`<br><br><br>![Message Center JPG1][4]{: style="max-width:100%;"} | The key-value pairs for this use case include:<br><br>- `message_header` set as `HTML`<br>- `class_type` set as `message_webview`<br>- `message_title`<br><br>This message also looks for an HTML key-value pair, but if you are working with a web domain, a URL key-value pair is also valid.<br><br>![Message Center JPG2][5] |
{: .reset-td-br-1 .reset-td-br-2}

#### Further Explanation

The message center logic is driven by the `contentCardClassType` that is provided by the key-value pairs from Braze. Using the `addContentCardToView` method, you are able to both filter and identify these class types.

{% tabs %}
{% tab Swift %}
__Using `class_type` for On Click Behavior__<br>
When a message is clicked, the `ContentCardClassType` handles how the next screen should be populated.
```swift
func addContentCardToView(with message: Message) {
    switch message.contentCardData?.contentCardClassType {
      case .message(.fullPage):
        loadContentCardFullPageView(with: message as! FullPageMessage)
      case .message(.webView):
        loadContentCardWebView(with: message as! WebViewMessage)
      default:
        break
    }
}
```
{% endtab %}
{% tab Objective-C %}
__Using `class_type` for On Click Behavior__<br>
When a message is clicked, the `ContentCardClassType` handles how the next screen should be populated.
```objc
- (void)addContentCardToView:(Message *)message {
  switch (message.contentCardData.classType) {
    case ContentCardClassTypeMessageFullPage:
      [self loadContentCardFullPageView:(FullPageMessage *)message];
      break;
    case ContentCardClassTypeMessageWebview:
      [self loadContentCardWebView:(WebViewMessage *)message];
      break;
    default:
      break;
  }
}
```
{% endtab %}
{% endtabs %}

##### Ready to log analytics?
Visit the [following section](#logging-impressions-clicks-and-dismissals) to get a better understanding of how the flow of data should look.

![Interactive Content PNG][6]{: style="border:0;"}{: style="float:right;max-width:45%;border:0;margin-left:15px;"} 

### Interactive Content Cards
<br>
Content Cards can be leveraged to create dynamic and interactive experiences for your users. In the example to the right, we have a Content Card pop-up appear at checkout providing users last-minute promotions. 

Well-placed cards like this are a great way to give users a "nudge" toward specific user actions. 
<br><br><br>
#### Dashboard Configuration

The dashboard configuration for interactive Content Cards is quick and straightforward. The key-value pairs for this use case include a `discount_percentage` set as the desired discount amount and `class_type` set as `coupon_code`. These key-value pairs are how type-specific Content Cards get filtered and displayed on the checkout screen.

![Interactive Content JPG][7]{: style="max-width:70%;"} 

##### Ready to log analytics?
Visit the [following section](#logging-impressions-clicks-and-dismissals) to get a better understanding of how the flow of data should look.

## Logging Impressions, Clicks, and Dismissals

After extending your custom objects to function as Content Cards, logging valuable metrics like impressions, clicks, and dismissals is quick and simple. This can be done through the use of a `ContentCardable` protocol that references and provides data to a helper file to be logged by the Braze SDK.

#### __Implementation Components__<br><br>

{% tabs %}
{% tab Swift %}
__Logging Analytics__<br>
The logging methods can be called directly from objects conforming to the `ContentCardable` protocol.
```swift
customObject.logContentCardImpression()
customObject.logContentCardClicked()
customObject.logContentCardDismissed()
```

__Retreiving the `ABKContentCard`__<br>
The `idString` passed in from your custom object is used to identify the associated Content Card to log analytics.

```swift
extension BrazeManager {
  func logContentCardImpression(idString: String?) {
    guard let contentCard = getContentCard(forString: idString) else { return }
 
    contentCard.logContentCardImpression()
  }
   
  private func getContentCard(forString idString: String?) -> ABKContentCard? {
    return contentCards?.first(where: { $0.idString == idString })
  }
}
```
{% endtab %}
{% tab Objective-C %}
__Logging Analytics__<br>
The logging methods can be called directly from objects conforming to the `ContentCardable` protocol.
```objc
[customObject logContentCardImpression];
[customObject logContentCardClicked];
[customObject logContentCardDismissed];
```

__Retreiving the `ABKContentCard`__<br>
The `idString` passed in from your custom object is used to identify the associated Content Card to log analytics.

```objc
- (void)logContentCardImpression:(NSString *)idString {
  ABKContentCard *contentCard = [self getContentCard:idString];
  [contentCard logContentCardImpression];
}
 
- (ABKContentCard *)getContentCard:(NSString *)idString {
  NSPredicate *predicate = [NSPredicate predicateWithFormat:@"self.idString == %@", idString];
  NSArray *filteredArray = [self.contentCards filteredArrayUsingPredicate:predicate];
 
  return filteredArray.firstObject;
}
```
{% endtab %}
{% endtabs %}

{% alert important %}
For a control variant Content Card, a custom object should still be instantiated, and UI logic should set the object's corresponding view as hidden. The object can then log an impression to inform our analytics of when a user would have seen the control card.
{% endalert %}

## Helper Files

{% details ContentCardKey Helper File %}
{% tabs %}
{% tab Swift %}
```swift
enum ContentCardKey: String {
  case idString
  case created
  case classType = "class_type"
  case dismissible
  case extras
  ...
}
```
{% endtab %}
{% tab Objective-C %}
```objc
static NSString *const ContentCardKeyIdString = @"idString";
static NSString *const ContentCardKeyCreated = @"created";
static NSString *const ContentCardKeyClassType = @"class_type";
static NSString *const ContentCardKeyDismissible = @"dismissible";
static NSString *const ContentCardKeyExtras = @"extras";
...
```
{% endtab %}
{% endtabs %}
{% enddetails %}

[1]: {% image_buster /assets/img/cc_implementation/supplementary.png %}
[2]: {% image_buster /assets/img/cc_implementation/supplementary_content.png %}
[3]: {% image_buster /assets/img/cc_implementation/message_center.png %}
[4]: {% image_buster /assets/img/cc_implementation/full_page.png %}
[5]: {% image_buster /assets/img/cc_implementation/html_webview.png %}
[6]: {% image_buster /assets/img/cc_implementation/discount2.png %}
[7]: {% image_buster /assets/img/cc_implementation/discount.png %}
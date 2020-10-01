---
nav_title: Implementation Guide
platform: iOS
page_order: 7
description: "This implementation guide covers Content Card code considerations, three use cases built by our team, accompanying code snippets, and guidance on logging impressions, clicks, and dismissals."
---

# Content Card Implementation Guide

> This implementation guide covers Content Card code considerations, three use cases built by our team, accompanying code snippets, and guidance on logging impressions, clicks, and dismissals. Visit our Braze Demo Repository [here](https://github.com/braze-inc/braze-growth-shares-ios-demo-app)! Please note that this implementation guide is centered around a Swift implementation, but Objective-C snippets are provided for those interested.

## Code Considerations

### Import Statements and Helper Files

When building out Content Cards, you should integrate them using a single "import Appboy-iOS-SDK" statement and helper file. This approach limits issues that arise from excessive SDK imports, making it easier to track, debug, and alter code. An example helper file can be found [here](https://github.com/braze-inc/braze-growth-shares-ios-demo-app/blob/master/Braze%20Demo/AppboyManager.swift).

### Content Cards as Custom Objects

Much like a rocketship adding a booster, your own custom objects can be extended to function as Content Cards in a way that does not depend on the Braze SDK. This can be done by conforming to the ContentCardable protocol and implementing the initializer (as seen below) and through the use of the ContentCardData struct, allows you to access the ABKContentCard data. 

The initializer also includes a ContentCardClassType enum. Through the use of key-value pairs within the Braze dashboard, you can set an explicit class_type key that will be used to determine what object to initialize. Once you have a solid understanding of these code considerations, check out our [use cases](#sample-use-cases) below to get started implementing your own custom objects.

{% include video.html id="55KTZqYAl7Y" align="center" %}

{% tabs local %}
{% tab Swift %}
__No ABKContentCard Dependencies__<br>
ContentCardData represents the parsed out values of an ABKContentCard.

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
    AppboyManager.shared.logContentCardClicked(idString: contentCardData?.contentCardId)
  }
  
  func logContentCardDismissed() {
    AppboyManager.shared.logContentCardDismissed(idString: contentCardData?.contentCardId)
  }
  
  func logContentCardImpression() {
    AppboyManager.shared.logContentCardImpression(idString: contentCardData?.contentCardId)
  }
}

struct ContentCardData: Hashable {
  let contentCardId: String
  let contentCardClassType: ContentCardClassType
  let createdAt: Double
  let isDismissable: Bool
}

extension ContentCardData: Equatable {
  static func ==(lhs: ContentCardData, rhs: ContentCardData) -> Bool {
    return lhs.contentCardId == rhs.contentCardId
  }
}
```
{% endtab %}
{% tab Objective-C %}

__No ABKContentCard Dependencies__<br>
ContentCardData represents the parsed out values of an ABKContentCard.

```objc
@interface ContentCardData : NSObject

+ (ContentCardClassType)contentCardClassTypeForString:(NSString*)rawValue;

- (instancetype)initWithIdString:(NSString *)idString classType:(ContentCardClassType)classType createdAt:(double)createdAt isDismissable:(BOOL)isDismissable;

@property (nonatomic, readonly) NSString *contentCardId;
@property (nonatomic) ContentCardClassType classType;
@property (nonatomic, readonly) double *createdAt;
@property (nonatomic, readonly) BOOL isDismissable;

@end

@protocol ContentCardable <NSObject>

@property (nonatomic, strong) ContentCardData *contentCardData;
- (instancetype __nullable)initWithMetaData:(NSDictionary *)metaData classType:(enum ContentCardClassType)classType;

- (BOOL)isContentCard;
- (void)logContentCardImpression;
- (void)logContentCardClick;
- (void)logContentCardDismissal;

@end
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Custom Object Initializer__<br>
MetaData from an ABKContentCard is used to populate your object's variables. The key-value pairs set up the Braze Dashboard are represented in the “extras” dictionary.

```swift
extension Tile: ContentCardable {
  init?(metaData: [ContentCardKey: Any], classType contentCardClassType: ContentCardClassType) {
    guard let idString = metaData[.idString] as? String,
      let createdAt = metaData[.created] as? Double,
      let isDismissable = metaData[.dismissable] as? Bool,
      let extras = metaData[.extras] as? [AnyHashable: Any],
      let tileIdString = extras["tile_id"] as? String,
      let tileId = Int(tilleIdString),  
      let title  = extras["tile_title"] as? String,
      let priceString = extras["tile_price"] as? String,
      let price = Decimal(string: priceString),
      let imageUrl = extras["tile_image"] as? String
      else { return nil }
    
    let tags = extras[ContentCardKey.tags.rawValue] as? String ?? ""
    let contentCardData = ContentCardData(contentCardId: idString, contentCardClassType: contentCardClassType, createdAt: createdAt, isDismissable: isDismissable)
    
    self.init(contentCardData: contentCardData, id: tileId, title: title, price: price, tags: tags.separatedByCommaSpaceValue, imageUrl: imageUrl)
  }
}
```
{% endtab %}
{% tab Objective-C %}
__Custom Object Initializer__<br>
MetaData from an ABKContentCard is used to populate your object's variables. The key-value pairs set up the Braze Dashboard are represented in the “extras” dictionary.

```objc
- (id _Nullable)initWithMetaData:(nonnull NSDictionary *)metaData classType:(enum ContentCardClassType)classType {
  self = [super init];
  if (self) {
    if (metaData[@"idString"] != nil && metaData[@"created"] != nil && metaData[@"dismissable"] != nil && metaData[@"extras"] != nil) {
      NSDictionary  *extras = metaData[@"extras"];
      NSString *idString = metaData[@"idString"];
      double createdAt = [metaData[@"createdAt"] doubleValue];
      BOOL isDismissable = metaData[@"isDimissable"];
      if (extras[@"tile_id"] != nil && extras[@"tile_title"] != nil && extras[@"tile_price"] != nil && extras[@"tile_image"] !=  nil) {
        NSString *tagsString = extras[@"tile_tags"];
        _idString = [extras[@"tile_id"] integerValue];
        _title = extras[@"tile_title"];
        _tags = [tagsString componentsSeparatedByString:@", "];
        _imageUrl = extras[@"tile_image"];
        self.price = [NSDecimalNumber decimalNumberWithString:extras[@"tile_price"]];
        self.contentCardData = [[ContentCardData alloc] initWithIdString:idString classType:classType createdAt: createdAt isDismissable:isDismissable];
        return self;
      }
    }
  }
  return nil;
}
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Identifying Types__<br>
The ContentCardClassType enum represents the class_type value in the Braze Dashboard.

```swift
enum ContentCardClassType: Hashable {
  case ad
  case coupon
  case item(ItemType)
  case message(MessageCenterViewType)
  case none
  
  enum ItemType {
    case tile
  }
  
  enum MessageCenterViewType {
    case fullPage
    case webView
  }

  init(rawType: String?) {
    switch rawType?.lowercased() {
    case "coupon_code":
      self = .coupon
    case "home_tile":
      self = .item(.tile)
    case "message_full_page":
      self = .message(.fullPage)
    case "message_webview":
      self = .message(.webView)
    case "ad_banner":
      self = .ad
    default:
      self = .none
    }
  }
}
```
{% endtab %}
{% tab Objective-C %}
__Identifying Types__<br>
The ContentCardClassType enum represents the class_type value in the Braze Dashboard.

```objc
typedef NS_ENUM(NSInteger, ContentCardClassType) {
  ContentCardClassTypeNone = 0,
  ContentCardClassTypeAdBanner,
  ContentCardClassTypeCoupon,
  ContentCardClassTypeHomeTile,
  ContentCardClassTypeMessageFullPage,
  ContentCardClassTypeMessageWebview,
};
#define kContentCardClassTypeArray @"", @"ad_banner", @"coupon_code", @"home_tile", @"message_full_page", @"message_webview", nil

+ (ContentCardClassType)contentCardClassTypeForString:(NSString*)rawValue {
  NSArray *contentCardClassTypeArray = [[NSArray alloc] initWithObjects:kContentCardClassTypeArray];
  if ([contentCardClassTypeArray indexOfObject:rawValue] == NSNotFound) {
    return ContentCardClassTypeNone;
  } else {
    NSInteger value = [contentCardClassTypeArray indexOfObject:rawValue];
    return (ContentCardClassType) value;
  }
}
```
{% endtab %}
{% endtabs %}

## Sample Use Cases

There are 3 sample customer use cases provided. Each sample has video walkthroughs, code snippets, and a look into how Content Card variables may look and be used in the Braze dashboard:
- [Content Cards As Supplemental Content](#content-cards-as-supplemental-content)
- [Content Cards in a Message Center](#content-cards-in-a-message-center)
- [Interactive Content Cards](#interactive-content-cards)

### Content Cards as Supplemental Content

You can seamlessly blend Content Cards into an existing feed, allowing data from multiple feeds to load simultaneously. This creates a cohesive, harmonious experience with Braze Content Cards and existing feed content.

{% include video.html id="9nmwlQKnnfE" align="center" %}
#### __Load Content Cards Alongside Existing Content__<br><br>

{% tabs local %}
{% tab Swift %}
__Load the data simultaneously with OperationQueues__
```swift
addOperation { [weak self] in
      guard let self = self else { return }
      self.loadTiles(self.tile)
    }
    
    addOperation { [weak self] in
      guard let self = self else { return }
      self.loadContentCards()
      self.semaphore.wait()
    }
    
    addBarrierBlock {
      completion(tiles, ads)
    }
```
{% endtab %}
{% tab Objective-C %}
__Load the data simultaneously with OperationQueues__
```objc
HomeListOperationQueue * __weak weakSelf = self;

[self addOperationWithBlock:^{
  if (weakSelf) {
    [weakSelf loadContentCards];
    dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
  }
}];
    
[self addOperationWithBlock:^{
  if (weakSelf) {
    [weakSelf loadLocalTilesWithCompletion:^(NSMutableArray * localTiles, NSError*) {
      [tiles addObjectsFromArray:localTiles];
    }];
  }
}];
    
[self addBarrierBlock:^{
  completion(tiles, ads);
}];
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Local Data Operation__
```swift
func loadTiles(_ completion: @escaping ([Tile]) -> ()) {
      switch result {
      case .success(let metaData):
        completion(metaData.tiles)
      case .failure:
        // handle error
    }
  }
```
{% endtab %}
{% tab Objective-C %}
__Local Data Operation__
```objc
- (void)loadTilesWithCompletion:(void (^)(NSMutableArray*, NSError *))completion {
  [localDataCoordinator loadTilesFromLocalDataWithCompletion:^(NSMutableArray *tiles, NSError *error) {
    if (error == nil) {
      completion(tiles, error);
    } else {
      // handle error
    }
  }];
}
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Content Card Operation__
```swift
func loadContentCards() {
    AppboyManager.shared.addObserverForContentCards(observer: self, selector: #selector(contentCardsUpdated))
    AppboyManager.shared.requestContentCardsRefresh()
  }
  
  @objc private func contentCardsUpdated(_ notification: Notification) {
    let contentCards = AppboyManager.shared.handleContentCardsUpdated(notification, for: [.item(.tile), .ad])
    contentCardCompletionHandler(contentCards)
    semaphore.signal()
  }
```
{% endtab %}
{% tab Objective-C %}
__Content Card Operation__
```objc
- (void)loadContentCards {
  [[AppboyManager shared] addObserverForContentCards:self selector:@selector(contentCardsUpdated:)];
  [[AppboyManager shared] requestContentCardsRefresh];
}


- (void)contentCardsUpdated:(NSNotification *)notification {
  NSArray *classTypes = @[@(ContentCardClassTypeHomeTile),@(ContentCardClassTypeAdBanner)];
  NSArray *contentCards = [[AppboyManager shared] handleContentCardsUpdated:notification forClassTypes:classTypes];
  contentCardCompletionHandler(contentCards);
  dispatch_semaphore_signal(semaphore);
}
```
{% endtab %}
{% endtabs %}

### Content Cards in a Message Center
Content Cards can be used in a message center format where each message is its own card. Each card contains additional key-value pairs that power on-click UI/UX.
{% include video.html id="dmaT61p8kW8" align="center" %}

{% tabs %}
{% tab Swift %}
__Using 'class_type' for On Click Behavior__<br>
When a message is clicked, the ContentCardClassType handles how the next screen should be populated.
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
__Using 'class_type' for On Click Behavior__<br>
When a message is clicked, the ContentCardClassType handles how the next screen should be populated.
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

### Interactive Content Cards
Content Cards can be leveraged to create interactive experiences for your users. In the demo below, we have a Content Card pop-up appear at checkout providing users last-minute promotions. Well-placed cards like this are a great way to give users a "nudge" toward specific user actions. 

{% include video.html id="76ivkU6Zmdg" align="center" %}
#### Interactable View<br><br>

{% tabs %}
{% tab Swift %}
__Requesting Content Cards__
```swift
func loadContentCards() {
    AppboyManager.shared.addObserverForContentCards(observer: self, selector: #selector(contentCardsUpdated))
    AppboyManager.shared.requestContentCardsRefresh()
  }
```
{% endtab %}
{% tab Objective-C %}
__Requesting Content Cards__
```objc
- (void)loadContentCards {
  [[AppboyManager shared] addObserverForContentCards:self selector:@selector(contentCardsUpdated:)];
  [[AppboyManager shared] requestContentCardsRefresh];
}
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Getting Type-Specific Content Cards__<br>
The class_type is passed in as a filter to only return Content Cards that have a matching class_type.
```swift
@objc func contentCardsUpdated(_ notification: Notification) {
    guard let contentCards = AppboyManager.shared.handleContentCardsUpdated(notification, for: [.coupon]) as? [Coupon], !contentCards.isEmpty else { return }
}
```
{% endtab %}
{% tab Objective-C %}
__Getting Type-Specific Content Cards__<br>
The class_type is passed in as a filter to only return Content Cards that have a matching class_type.
```objc
- (void)contentCardsUpdated:(NSNotification *)notification {
  NSArray *classTypes = @[@(ContentCardClassTypeCouponCode)];
  NSArray *contentCards = [[AppboyManager shared] handleContentCardsUpdated:notification forClassTypes:classTypes];
}
```
{% endtab %}
{% endtabs %}

## Logging Impressions, Clicks, and Dismissals

After extending your own custom objects to function as Content Cards, logging valuable metrics like impressions, clicks, and dismissals is quick and simple. This can be done through the use of a ContentCardable protocol that references and provides data to a helper file to be logged by the Braze SDK.

{% include video.html id="N05Ojf-BKs0" align="center" %}
#### __Implementation Components__<br><br>

{% tabs %}
{% tab Swift %}
__Custom Objects Call the Logging Methods__<br>
From the ContentCardable protocol
```swift
func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
  let message = messages[indexPath.row]
  message.logContentCardImpression()
}
```
{% endtab %}
{% tab Objective-C %}
__Custom Objects Call the Logging Methods__<br>
From the ContentCardable protocol
```objc
-(void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath {
  Message *message = self.messages[indexPath.row];
  [message logContentCardImpression];
}
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Retrieve the Content Card from the ContentCardId__<br>
AppboyManager.Swift file handles the ABK dependencies
```swift
protocol ContentCardable {
  func logContentCardImpression() {
    AppboyManager.shared.logContentCardImpression(idString: contentCardData?.contentCardId)
  }
}
```
{% endtab %}
{% tab Objective-C %}
__Retrieve the Content Card from the ContentCardId__<br>
AppboyManager.Swift file handles the ABK dependencies
```objc
- (void)logContentCardImpression {
  [[AppboyManager shared] logContentCardImpression:self.contentCardData.contentCardId];
}
```
{% endtab %}
{% endtabs %}
{% tabs local %}
{% tab Swift %}
__Call ABKContentCard Functions__<br>
From the `Appboy.sharedInstance()?.contentCardsController.contentCards` array
```swift
func logContentCardImpression(idString: String?) {
  guard let contentCard = getContentCard(forString: idString) else { return }

  contentCard.logContentCardImpression()
  }
  
  private func getContentCard(forString idString: String?) -> ABKContentCard? {
    return contentCards?.first(where: { $0.idString == idString })
  }
```
{% endtab %}
{% tab Objective-C %}
__Call ABKContentCard Functions__<br>
From the `Appboy.sharedInstance()?.contentCardsController.contentCards` array
```objc
- (void)logContentCardImpression:(NSString * __nullable)idString {
  NSPredicate *predicate = [NSPredicate predicateWithFormat:@"self.idString == %@", idString];
  NSArray *filteredArray = [self.contentCards filteredArrayUsingPredicate:predicate];
  ABKContentCard *contentCard = filteredArray.firstObject;
  [contentCard logContentCardImpression];
}
```
{% endtab %}
{% endtabs %}
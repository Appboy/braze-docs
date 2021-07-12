---
nav_title: Google Tag Manager
platform: iOS
page_order: 7
description: "This article covers how to initalize, configure, and implement the Google Tag manager into your iOS app."

---

# Google Tag Manager for iOS

## Initializing the SDK {#initializing-ios-google-tag-provider}

Braze's iOS SDK can be initialized and controlled by tags configured within [Google Tag Manager][5].

But first - before using Google Tag Manager - be sure to first follow our [Initial SDK Setup][1].

## Configuring Your Google Tag Manager {#configuring-ios-google-tag-manager}

In our example, we'll pretend we are a music streaming app that wants to log different events as users listen to songs. Using Google Tag Manager for iOS, we can control which of our 3rd party vendors receive this event, and create tags specific to Braze.

### Custom Events

Custom events are logged with `actionType` set to `logEvent`. The Braze custom tag provider in our example is expecting the custom event name to be set using `eventName`.

To get started, create a trigger that looks for an "Event Name" that equals `played song`

![Event Name Trigger for events][3]

Next, create a new Tag ("Function Call") and enter the Class Path of your [custom tag provider](#adding-ios-google-tag-provider) described later in this article. 

This tag will be triggered when you log the `played song` event we just created. 

In our example tag's custom parameters (key-value pairs), we've set `eventName` to `played song` - which will be the custom event name logged to Braze.

{% alert important %}
When sending a custom event, be sure to set `actionType` to `logEvent`, and set a value for `eventName` as shown in the screenshot below. 

The custom tag provider in our example will use these key to determine what action to take, and what event name to send to Braze when it receives data from Google Tag Manager.
{% endalert %}

![Function Call Tag][4]

You can also include additional key-value pair arguments to the tag, which will be sent as custom event properties to Braze. `eventName` and `actionType` will not be ignored for custom event properties. In our example tag below, we'll pass in `genre` which was defined using a tag Variable in Google Tag Manager - sourced from the custom event we logged in our app.

The `genre` event property is sent to Google Tag Manager a "Firebase - Event Parameter" variable since Google Tag Manager for iOS uses Firebase as the data layer.

![Tag Variable Event Name][6]

Lastly, when a user plays a song in our app, we will log an event through Firebase/Google Tag Manager using the Firebase Analytics event name that matches our tag's trigger name, `played song`.

{% tabs %}
{% tab OBJECTIVE-C %}

```obj-c
NSDictionary *parameters = @{@"genre" : @"pop",
                             @"number of times listened" : @42};
[FIRAnalytics logEventWithName:@"played song" parameters:parameters];
```

{% endtab %}
{% endtabs %}

### Logging Custom Attributes

Custom attributes are set via an `actionType` set to `customAttribute`. The Braze custom tag provider is expecting the custom attribute key/value to be set via `customAttributeKey` and `customAttributeValue`.

{% tabs %}
{% tab OBJECTIVE-C %}

```obj-c
NSDictionary *parameters = @{@"customAttributeKey" : @"favorite song",
                             @"customAttributeValue" : @"Private Eyes"};
[FIRAnalytics logEventWithName:@"customAttribute" parameters:parameters];
```

{% endtab %}
{% endtabs %}

### Calling ChangeUser

Calls to `changeUser()` are made via an `actionType` set to `changeUser`. The Braze custom tag provider is expecting the Braze user ID to be set via an `externalUserId` key-value pair within your tag.

{% tabs %}
{% tab OBJECTIVE-C %}

```obj-c
NSDictionary *parameters = @{@"externalUserId" : userId};
[FIRAnalytics logEventWithName:@"changeUser" parameters:parameters];
```

{% endtab %}
{% endtabs %}

## Braze SDK Custom Tag Provider {#adding-ios-google-tag-provider}

With the tags and triggers set up, you will also need to implement Google Tag Manager in your iOS app which can be found in Google's [documentation][2].

Once Google Tag Manager is installed in your app, add a custom tag provider to call Braze SDK methods based on the tags you've configured within Google Tag Manager. 

Be sure to note the "Class Path" to the file which - this is what you'll enter when setting up a Tag in the [Google Tag Manager][5] console.

This example shows one of many ways to structure your custom tag provider, where we determine which Braze SDK method to call based on the `actionType` key-value pair sent down from the GTM Tag.

The `actionType` we've supported in our example are `logEvent`, `customAttribute`, and `changeUser`, but you may prefer to change how your tag provider handles data from Google Tag Manager.

Add the following code to your `BrazeGTMTagManager.h` file:

{% tabs %}
{% tab OBJECTIVE-C %}

```obj-c
@import Firebase;
@import GoogleTagManager;

@interface BrazeGTMTagManager : NSObject <TAGCustomFunction>

@end
```

{% endtab %}
{% endtabs %}

And add the following code to your `BrazeGTMTagManager.m` file:

{% tabs %}
{% tab OBJECTIVE-C %}

```obj-c
#import <Foundation/Foundation.h>
#import "BrazeGTMTagManager.h"
#import "Appboy-iOS-SDK/AppboyKit.h"

static NSString *const ActionTypeKey = @"actionType";

// Custom Events
static NSString *const LogEventActionType = @"logEvent";
static NSString *const LogEventEventName = @"eventName";

// Custom Attributes
static NSString *const CustomAttributeActionType = @"customAttribute";
static NSString *const CustomAttributeKey = @"customAttributeKey";
static NSString *const CustomAttributeValueKey = @"customAttributeValue";

// Change User
static NSString *const ChangeUserActionType = @"changeUser";
static NSString *const ChangeUserExternalUserId = @"externalUserId";

@implementation BrazeGTMTagManager

- (NSObject *)executeWithParameters:(NSDictionary *)parameters {
  NSMutableDictionary *mutableParameters = [parameters mutableCopy];
  
  NSString *actionType = mutableParameters[ActionTypeKey];
  if (!actionType) {
    NSLog(@"There is no Braze action type key in this call. Doing nothing.", nil);
    return nil;
  }
  
  [mutableParameters removeObjectForKey:ActionTypeKey];
  
  if ([actionType isEqualToString:LogEventActionType]) {
    [self logEvent:mutableParameters];
  } else if ([actionType isEqualToString:CustomAttributeActionType]) {
    [self logCustomAttribute:mutableParameters];
  } else if ([actionType isEqualToString:ChangeUserActionType]) {
    [self changeUser:mutableParameters];
  } else {
    NSLog(@"Invalid action type. Doing nothing.");
  }
  return nil;
}

- (void)logEvent:(NSMutableDictionary *)parameters {
  NSString *eventName = parameters[LogEventEventName];
  [parameters removeObjectForKey:LogEventEventName];
  [[Appboy sharedInstance] logCustomEvent:eventName withProperties:parameters];
}

- (void)logCustomAttribute:(NSMutableDictionary *)parameters {
  NSString *customAttributeKey = parameters[CustomAttributeKey];
  id customAttributeValue = parameters[CustomAttributeValueKey];
  
  if ([customAttributeValue isKindOfClass:[NSString class]]) {
    [[Appboy sharedInstance].user setCustomAttributeWithKey:customAttributeKey
                                             andStringValue:customAttributeValue];
  } else if ([customAttributeValue isKindOfClass:[NSDate class]]) {
    [[Appboy sharedInstance].user setCustomAttributeWithKey:customAttributeKey
                                               andDateValue:customAttributeValue];
  } else if ([customAttributeValue isKindOfClass:[NSNumber class]]) {
    if (strcmp([customAttributeValue objCType], [@(YES) objCType]) == 0) {
      [[Appboy sharedInstance].user setCustomAttributeWithKey:customAttributeKey
                                                 andBOOLValue:[(NSNumber *)customAttributeValue boolValue]];
    } else if (strcmp([customAttributeValue objCType], @encode(short)) == 0 ||
               strcmp([customAttributeValue objCType], @encode(int)) == 0 ||
               strcmp([customAttributeValue objCType], @encode(long)) == 0) {
      [[Appboy sharedInstance].user setCustomAttributeWithKey:customAttributeKey
                                              andIntegerValue:[(NSNumber *)customAttributeValue integerValue]];
    } else if (strcmp([customAttributeValue objCType], @encode(float)) == 0 ||
               strcmp([customAttributeValue objCType], @encode(double)) == 0) {
      [[Appboy sharedInstance].user setCustomAttributeWithKey:customAttributeKey
                                               andDoubleValue:[(NSNumber *)customAttributeValue doubleValue]];
    } else {
      NSLog(@"Could not map NSNumber value to Appboy custom attribute:%@", customAttributeValue);
    }
  } else if ([customAttributeValue isKindOfClass:[NSArray class]]) {
    [[Appboy sharedInstance].user setCustomAttributeArrayWithKey:customAttributeKey
                                                           array:customAttributeValue];
  }
}

- (void)changeUser:(NSMutableDictionary *)parameters {
  NSString *userId = parameters[ChangeUserExternalUserId];
  [[Appboy sharedInstance] changeUser:userId];
}

@end
```

{% endtab %}
{% endtabs %}

[1]: {{site.baseurl}}/developer_guide/platform_integration_guides/ios/initial_sdk_setup/
[2]: https://developers.google.com/tag-manager/ios/v5/
[3]: {% image_buster /assets/img/android_google_tag_manager/gtm_android_trigger.png %}
[4]: {% image_buster /assets/img/android_google_tag_manager/gtm_android_function_call_tag.png %}
[5]: https://tagmanager.google.com/
[6]: {% image_buster /assets/img/android_google_tag_manager/gtm_android_eventname_variable.png %}

---
nav_title: Defining a News Feed Category
platform: iOS
page_order: 2
description: "This reference article shows how to define a News Feed category in your iOS application."
channel:
  - news feed

---

# Defining a News Feed Category

Instances of the Braze News Feed can be configured to only receive cards from a certain "category". This allows for the effective integration of multiple News Feed streams within a single application. For more information on this feature visit our [News Feed Best Practices][40] doc.

News Feed Categories can be defined by calling one of the following methods as you load the News Feed:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[newsFeed setCategories:ABKCardCategoryAll];
[newsFeed setCategories:ABKCardCategoryAnnouncements];
[newsFeed setCategories:ABKCardCategoryAdvertising];
[newsFeed setCategories:ABKCardCategorySocial];
[newsFeed setCategories:ABKCardCategoryNews];
[newsFeed setCategories:ABKCardCategoryNoCategory];
```

{% endtab %}
{% tab swift %}

```swift
newsFeed.categories = ABKCardCategory.all
newsFeed.categories = ABKCardCategory.announcements
newsFeed.categories = ABKCardCategory.advertising
newsFeed.categories = ABKCardCategory.social
newsFeed.categories = ABKCardCategory.news
newsFeed.categories = ABKCardCategory.noCategory
```

{% endtab %}
{% endtabs %}


You can also populate a feed with a combination of categories as in the following example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[newsFeed setCategories:ABKCardCategoryAnnouncements|ABKCardCategoryAdvertising];
```

{% endtab %}
{% tab swift %}

```swift
newsFeed.categories = ABKCardCategory([.announcements, .advertising])
```

{% endtab %}
{% endtabs %}

[40]: {{site.baseurl}}/help/best_practices/news_feed/

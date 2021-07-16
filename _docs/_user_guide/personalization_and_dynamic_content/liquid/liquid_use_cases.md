---
nav_title: Liquid Use Case Library
page_order: 10

excerpt_separator: ""
page_type: glossary
layout: liquid_use_case_glossary
description: "This landing page is home to sample Liquid use cases organized by category, such as Anniversaries, App Usage, Countdowns, and more."
---

{% api %}

## Anniversaries and Birthdays

{% apitags %}
Anniversaries and Birthdays
{% endapitags %}

- [Personalize messages based on a user's anniversary year](#anniversary-year)
- [Personalize messages based on a user's birthday week](#birthday-week)
- [Send campaigns to users in their birthday month](#birthday-month)

### Personalize Messages Based on a User's Anniversary Year {#anniversary-year}

This use case shows how to calculate a user’s app anniversary based on their initial sign-up date and display different messages based on how many years they are celebrating.

{% raw %}
```liquid
{% assign this_month = 'now' | date: "%B" %}
{% assign this_day = 'now' | date: "%d" %}
{% assign anniversary_month = {{custom_attribute.${signup_date}}} | date: "%B" %}
{% assign anniversary_day = {{custom_attribute.${signup_date}}} | date: "%d" %}
{% assign anniversary_year = {{custom_attribute.${signup_date}}} | date: "%Y" %}
{% if {{this_month}} == {{anniversary_month}} %}
{% if {{this_day}} == {{anniversary_day}} %}
{% if {{anniversary_year}} == ‘2021’ %}
Happy one year anniversary!
{% elsif {{anniversary_year}} == ‘2020’ %}
Happy two year anniversary!
{% elsif {{anniversary_year}} == ‘2019’ %}
Happy three year anniversary!
{% elsif {{anniversary_year}} == ‘2018’ %}
Happy four year anniversary!
{% elsif {{anniversary_year}} == ‘2017’ %}
Happy five year anniversary!
{% elsif {{anniversary_year}} == ‘2016’ %}
Happy six year anniversary!
{% elsif {{anniversary_year}} == ‘2015’ %}
Happy seven year anniversary!
{% else %}
{% abort_message(not same month) %}
{% else %}
{% abort_message(not same day) %}
{% else %}
{% abort_message(not same year) %}
{% endif %}
{% endif %}
{% endif %}
```
{% endraw %}

**Explanation:** Here, we use the reserved variable `now` to template in the current date and time in [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601 "ISO 8601 Time Code Wiki") format. The filters `%B` (month, i.e. "May") and `%d` (day, i.e. "18") format the current month and day. We then use the same date and time filters on the `signup_date` values to ensure we can compare the two values using conditional tags and logic.

Then we repeat three more variable statements to get the `%B` and `%d` for the `signup_date`, but also adding `%Y` (year, i.e., "2021"). This forms the date and time of the `signup_date` into just the year. Knowing the day and month lets us check if the user's anniversary is today, and knowing the year tells us how many years it's been—which lets us know how many years to congratulate them on!

{% alert tip %} You can create as many conditions as years you've been collecting sign-up dates. {% endalert %}  

### Personalize Messages Based on a User's Birthday Week {#birthday-week}

This use case shows how to find a user's birthday, compare it to the current date, and then display special birthday messages before, during, and after their birthday week.

{% raw %}
```liquid
{% assign this_week = 'now' | date: '%W' %}
{% assign birthday_week = ${date_of_birth}  | date: '%W' %}
{% assign last_week = {{this_week}} | minus: 1 %}
{% assign next_week = {{this_week}} | plus: 1 %}
{% assign birthday_week_conversion = {{birthday_week}} | plus: 0 %}
{% if {{last_week}} == {{birthday_week_conversion}} %}
Happy birthday for last week!
{% elsif {{birthday_week}} == {{this_week}} %}
Happy birthday for this week!
{% elsif {{next_week}} == {{birthday_week_conversion}} %}
Happy birthday for next week!
{% else %}
No birthday for you!
{% endif %}
```
{% endraw %}

**Explanation:** Similar to the [anniversary year](#anniversary-year) use case, here we take the reserved variable `now` and use the `%W` filter (week, i.e., week 12 out of 52 in a year) to get the number week of the year that the user's birthday falls within. If the user's birthday week matches the current week, we send them a message congratulating them! 

We also include statements for `last_week` and `next_week` to further personalize your messaging.

### Send Campaigns to Users in Their Birthday Month {#birthday-month}

This use case shows how to calculate a user's birthday month, check if their birthday falls in the current month, and if so, send a special message.

{% raw %}
```liquid
{% assign this_month = ‘now’ | date: “%B” %}
{% assign birth_month = {{${date_of_birth}}} | date: “%B” %}
{% if {{this_month}} == {{birth_month}} %}
Message body 
{% else %} 
{% abort_message() %}
{% endif %}
```
{% endraw %}

**Explanation:** Similar to the [birthday week](#birthday-week) use case, except here we use the `%B` filter (month, i.e., "May") to calculate which users have a birthday this month. A potential application could be addressing birthday users in a monthly email.

{% endapi %}

{% api %}

## App Usage

{% apitags %}
App Usage
{% endapitags %}

- [Send messages in a user's language if they've logged a session](#app-session-language)
- [Personalize messages based on when a user last opened the app](#app-last-opened)
- [Show a different message if a user last used the app less than three days ago](#app-last-opened-less-than)

### Send Messages in a User's Language if They Haven't Logged a Session {#app-session-language}

This use case checks if a user has logged a session, and if not, includes logic to display a message based on the language manually collected via a custom attribute, if any. If there is no language information tied to their account, it will display the message in the default language. If a user has logged a session, it will pull any language information tied to the user and display the appropriate message. 

{% raw %}
```liquid
{% if {{${last_used_app_date}}} == nil %}
{% if {{custom_attribute.${user_language}}} == 'en' %}
Message in English based on custom attribute
{% elsif {{custom_attribute.${user_language}}} == 'fr' %}
Message in French based on custom attribute
{% else %}
Does not have language - Default language
{% endif %}
{% else %}
{% if ${language} == 'en' %}
Message in English based on Language
{% elsif ${language} == 'fr' %}
Message in French based on Language
{% else %}
Has language - Default language
{% endif %}
{% endif %}
```
{% endraw %}

{% raw %}
**Explanation:** Here, we're using two grouped `if` statements, nested. The first `if` statement checks to see if the user has started a session by checking if the `last_used_app_date` is `nil`. This is because `{{${language}}}` is auto-collected by the SDK when a user logs a session. If the user hasn't logged a session, we won't have their language yet, so this checks if any language-related custom attributes have been saved, and based on that information, will display a message in that language, if possible. 
{% endraw %}

The second `if` statement just checks for the default attribute because the user doesn't have `nil` for the `last_used_app_date`, which means they've logged a session, and we have their language.

{% alert note %}
[`Nil`](https://shopify.github.io/liquid/basics/types/#nil) is a reserved variable that is returned when Liquid code has no results. `Nil` is treated as `false` in an `if` block.
{% endalert %}

### Personalize Messages Based on When a User Last Opened the App {#app-last-opened}

This use case calculates the last time a user opened your app and will display a different personalized message depending on the length of time.

{% raw %}
```liquid
{% assign last_used_date = {{${last_used_app_date}} | date: "%s" %}
{% assign now = 'now' | date: "%s" %}
{% assign difference_in_days = {{now}} | minus: {{last_used_date}} | divided_by: 86400 %}
{% if {{difference_in_days}} < 3 %}
Happy to see you again!
{% else %}
It's been a while; here are some of our latest updates.
{% endif %}
```
{% endraw %}

### Show a Different Message if a User Last Used the App Less Than Three Days Ago {#app-last-opened-less-than}

This use case calculates how long ago a user used your app, and depending on the length of time, will display a different personalized message.

{% raw %}
```liquid
{% assign last_used_date = {{${last_used_app_date}} | date: "%s" %}
{% assign now = 'now' | date: "%s" %}
{% assign difference_in_days = {{now}} | minus: {{last_used_date}} | divided_by: 86400 %}
{% if {{difference_in_days}} < 3 %}
Message for a recently active user
{% else %}
Message for a less active user
{% endif %}
```
{% endraw %}

{% endapi %}

{% api %}

## Countdowns

{% apitags %}
Countdowns
{% endapitags %}

- [Add X days to today's date](#countdown-add-x-days)
- [Calculate a countdown from a set point in time](#countdown-difference-days)
- [Create a countdown for specific shipping dates and priorities](#countdown-shipping-options)
- [Create a countdown in days](#countdown-days)
- [Create a countdown from days to hours to minutes](#countdown-dynamic)
- [Create a countdown to a future date](#countdown-future-date)
- [Display how many days left until a custom date attribute will arrive](#countdown-custom-date-attribute)
- [Display how much time is left, and abort the message if there's only X time left](#countdown-abort-window)
- [In-app message to send X days before user's membership ends](#countdown-membership-expiry)
- [Personalize in-app messages based on user's date and language](#countdown-personalize-language)
- [Template in the date 30 days from now, formatted as month and day](#countdown-template-date)

### Add X Days to Today's Date {#countdown-add-x-days}

This use case adds a specific number of days to the current date to reference and add in messages. For example, you may want to send a mid-week message that shows events in the area for the weekend, like “Here are the movies we’re showing in 3 days!”

{% raw %}
```liquid
{{ “now” | date:‘%s’ | plus:259200 | date:“%F” }}
```
{% endraw %}

The `plus` value will always be in seconds, so we end with the filter `%F` to translate the seconds to days.

{% alert important %}
You may want to include a URL or deep link to a list of events in your message so you can send the user to a list of actions that are happening in the future. 
{% endalert %}

### Calculate a Countdown From a Set Point in Time {#countdown-difference-days}

This use case calculates the difference in days between a specific date and the current date. This difference can be used to display a countdown to your users.

{% raw %}
```liquid
{% assign event_date = '2020-08-19' | date: "%s" %}
{% assign today = 'now' | date: "%s" %}
{% assign difference = event_date | minus: today %}
{% assign difference_days = difference | divided_by: 86400 %}
you have {{ difference_days }} days left!
```
{% endraw %}

### Create a Countdown for Specific Shipping Dates and Priorities {#countdown-shipping-options}

This use case captures different shipping options, calculates the length of time it would take to receive, and displays messages encouraging users to purchase in time to receive their package by a certain date.

{% raw %}
```liquid
{% assign standard_shipping_start = "2019-12-10T00:00-05:00" | date: "%s" %}
{% assign standard_shipping_end = "2019-12-20T13:00-05:00" | date: "%s" %}
{% assign express_shipping_end = "2019-12-22T24:00-05:00" | date: "%s" %}
{% assign overnight_shipping_end = "2019-12-23T24:00-05:00" | date: "%s" %}
{% assign today = 'now' | date: "%s" %}

{% assign difference_s = standard_shipping_end | minus: today %}
{% assign difference_s_days = difference_s | divided_by: 86400.00 | round %}
difference s days: {{difference_s_days}}
{% assign difference_e = express_shipping_end | minus: today %}
{% assign difference_e_days = difference_e | divided_by: 86400.00 | round %}
difference e days: {{difference_e_days}}
{% assign difference_o = overnight_shipping_end | minus: today %}
{% assign difference_o_days = difference | divided_by: 86400.00 | round %}

{% if today >= standard_shipping_start and today <= standard_shipping_end %}
{% if difference_s_days == 0 %}
This is the last day to order with standard shipping, so your order gets here on time for Christmas Eve!
{% elsif difference_s_days == 1 %}
There is {{difference_s_days}} day left to order with standard shipping, so your order gets here on time for Christmas Eve!

{% else %}
There are {{difference_s_days}} days left to order with standard shipping so your order gets here on time for Christmas Eve!
{% endif %}
{% elsif today > standard_shipping_end and today < express_shipping_end %}
{% if difference_e_days == 1 %}
There is {{difference_e_days}} day left to order with express shipping, so your order gets here on time for Christmas Eve!
{% else %}
There are {{difference_e_days}} days left to order with express shipping so your order gets here on time for Christmas Eve!
{% endif%}
{% elsif today >= express_shipping_end and today < overnight_shipping_end %}
This is the last day for overnight shipping so your order gets here on time for Christmas Eve!
{% else %}
{% abort_message() %}
{% endif %}
```
{% endraw %}

### Create a Countdown in Days {#countdown-days}

This use case calculates the time left between a specific event and the current date and displays how many days are left until the event.

{% raw %}
```liquid
{% assign event_date = {{custom_attribute.${last_selected_event_date}}} | date: "%s" %}
{% assign today =  'now' | date: "%s"  %}
{% assign difference =  event_date | minus: today %}
{% assign difference_days = difference | divided_by: 86400 %}
Your order will arrive in {{ difference_days }} days!
```
{% endraw %}

{% alert important %}
You will need a custom attribute field with a `date` value.
{% endalert %}

### Create a Countdown From Days to Hours to Minutes {#countdown-dynamic}

This use case calculates the time left between a specific event and the current date. Depending on the time left until the event, it will change the time value (days, hours, minutes) to display different personalized messages.

For example, if there are two days until a customer's order arrives, you might say, “Your order will arrive in 2 days.” Whereas if there’s less than a day, you could change it to “Your order will arrive in 17 hours.”

{% raw %}
```liquid
{% assign today =  'now' | date: "%s"  %}
{% assign scheme_finish = "2017-10-13T10:30:30" | date: "%s" %}
{% assign difference_seconds =  scheme_finish | minus: today %}
{% assign difference_minutes = difference_seconds | divided_by: 60 %}
{% assign difference_hours = difference_seconds | divided_by: 3600 %}
{% assign difference_days = difference_seconds | divided_by: 86400 %}
{% if {{difference_minutes}} > 59 and {{difference_minutes}} < 1440 %}
You have {{difference_hours}} hours left till your order arrives!
{% elsif {{difference_minutes}} < 59 %}
You have {{difference_minutes}} minutes left till your order arrives!
{% else %}
You have {{difference_days}} days left till your order arrives!
{% endif %}
```
{% endraw %}

{% alert important %}
You will need a custom attribute field with a `date` value. You will also need to set time thresholds of when you want the time to be displayed in days, hours, and minutes.
{% endalert %}

### Create a Countdown to a Future Date {#countdown-future-date}

This use case calculates the difference between the current date and future event date and displays a message noting how many days until the event.

{% raw %}
```liquid
{% assign event_date = '2019-02-19' | date: "%s" %}
{% assign today = 'now' | date: "%s" %}
{% assign difference = event_date | minus: today %}
{% assign difference_days = difference | divided_by: 86400 %}
There are {{difference_days}} until your birthday!
{% endif %}
```
{% endraw %}

### Display How Many Days Left Until a Custom Date Attribute Will Arrive {#countdown-custom-date-attribute}

This use case calculates the difference in days between the current and future dates and displays a message if the difference matches a set number.

In this example, a user will receive a message within two days of the custom date attribute. Otherwise, the message will not be sent.

{% raw %}
```liquid
{% assign today = 'now' | date: '%j' | plus: 0 %}
{% assign surgery_date = {{custom_attribute.${surgery_date}}} | date: '%j' | plus: 0 %}

{% assign difference_days = {{surgery_date}} | minus: {{today}} %}
{% if difference_days == 2 %}
Your surgery is in 2 days on {{custom_attribute.${surgery_date}}}
{% else %}
{% abort_message %}
{% endif %}
```
{% endraw %}

### Display How Much Time Is Left, and Abort the Message if There’s Only X Time Left {#countdown-abort-window}

This use case will calculate how long until a certain date, and depending on the length (skipping messaging if the date is too soon), will display different personalized messages. 

For example, “You have x hours left to buy your ticket for London”, but don’t send the message if it’s within two hours of flight time for London.

{% raw %}
```liquid
{% assign today =  'now' | date: "%s"  %}
{% assign dep_time = {{event_properties.${outboundDate}}} | date: "%s" %}
{% assign time_to_dep = dep_time | minus: today %}
{% if {{time_to_dep}} < 7200 %}
{% abort_message("OutboundDate less than 2 hours") %}
{% elsif {{time_to_dep}} > 7200 and {{time_to_dep}} < 86400 %}
Don’t forget to buy your ticket to {{event_properties.${toStation}}} within next 24 hours!
{% else %}
Still traveling to {{event_properties.${toStation}}} in more than 24 hours? Book now!
{% endif %}
```
{% endraw %}

{% alert important %} You will need a custom event property. {% endalert %}

### In-App Message to Send X Days Before Users' Membership Ends {#countdown-membership-expiry}

This use case captures your membership expiry date, calculates how long until it expires, and displays different messages based on how long until your membership expires.

{% raw %}
```liquid
{% assign membership_expiry = {{custom_attribute.${membership_expiry_date}}} | date: "%s" %}
{% assign today = 'now' | date: "%s" %}
{% assign difference = membership_expiry | minus: today %}
{% assign difference_days = difference | divided_by: 86400 %}

{% if difference_days > 4 and difference_days <= 7 %}
You have {{difference_days}} days left in your trial, make sure you upgrade!

{% elsif difference_days > 2 and difference_days <= 4 %}
HURRY! You have {{difference_days}} days left in your trial, make sure you upgrade!

{% elsif difference_days == 2 %}
LAST CHANCE! You have {{difference_days}} days left in your trial. Make sure you upgrade!

{% else %}
You have few days left in your trial. Make sure to upgrade!
{% endif %}
```
{% endraw %}

### Personalize In-App Messages Based on Users' Date and Language {#countdown-personalize-language}

This use case calculates a countdown to an event, and based on a user's language setting, will display the countdown in their language.

For example, you might send a series of upsell messages to users once a month to let them know how long an offer is still valid with four in-app messages:

- Initial
- 2 days left
- 1 day left
- Final day

{% raw %}
```liquid
{% assign today = 'now' | date: "%s" %}
{% assign end_date = "2021-04-16T23:59:59" | date: "%s" %}
{% assign difference = end_date | minus: today %}
{% assign difference_days = difference | divided_by: 86400 %}
{% if {{difference_days}} >= 3 %}
{% if ${language} == 'de' %}

Hallo, das Angebot gilt bis zum 16.04.

{% elsif ${language} == 'ch' %}
Grüezi, das Angebot gilt bis zum 16.04.

{% elsif ${language} == 'en' %}
The offer is valid until 16.04.

{% else %}
The offer is valid until 16.04.

{% endif %}
{% elsif {{difference_days}} == 2 %}
{% if ${language} == 'de' %}
INSERT MESSAGE

{% elsif ${language} == 'ch' %}
INSERT MESSAGE

{% elsif ${language} == 'en' %}
INSERT MESSAGE

{% else %}
INSERT MESSAGE
{% endif %}

{% elsif {{difference_days}} == 1 %}
{% if ${language} == 'de' %}
INSERT MESSAGE

{% elsif ${language} == 'ch' %}
INSERT MESSAGE

{% elsif ${language} == 'en' %}
INSERT MESSAGE

{% else %}
INSERT MESSAGE
{% endif %}

{% elsif {{difference_days}} == 0 %}
{% if ${language} == 'de' %}
Hallo, das Angebot gilt noch heute.

{% elsif ${language} == 'ch' %}
Hallo, das Angebot gilt noch heute.

{% elsif ${language} == 'en' %}
Grüezi, das Angebot gilt noch heute.

{% else %}
Hi, the offer is only valid today.
{% endif %}

{% else %}
{% abort_message('calculation failed') %}
{% endif %}
```
{% endraw %}

{% alert important %}
You will need to assign a `date` value and include abort logic if the given date falls outside of the date range. For exact day calculations, the assigned end date must include 23:59:59.
{% endalert %}

### Template in the Date 30 Days from Now, Formatted as Month and Day {#countdown-template-date}

This use case will display the date 30 days from now to use in messaging.

{% raw %}
```liquid
{% assign today
 = 'now' | date: "%s" %}
{% assign thirty_days = {{today}} | plus: 2592000 | date: "%B %d" %}
```
{% endraw %}

{% endapi %}

{% api %}

## Custom Attribute

{% apitags %}
Custom Attribute
{% endapitags %}

- [Personalize a message based on matching custom attributes](#attribute-matching)
- [Subtract two custom attributes to display the difference as a monetary value](#attribute-monetary-difference)
- [Reference a user's first name if their full name is stored in the first_name field](#attribute-first-name)

### Personalize a Message Based on Matching Custom Attributes {#attribute-matching}

This use case checks if a user has specific custom attributes and, if so, will display different personalized messages. 

{% raw %}
```liquid
{% if custom_attribute.${hasShovel} == true and custom_attribute.${VisitToGroundTooTough} > 0 %}
The ground is very hard. The dirt road goes East.
{% elsif custom_attribute.${hasShovel} == true %}
The dirt road goes East.
{% elsif custom_attribute.${VisitToStart} > 0 %}
The dirt road goes East.
The shovel here.
{% else %}
You are at a dead-end of a dirt road. The road goes to the east. In the distance, you can see that it will eventually fork off. The trees here are very tall royal palms, and they are spaced equidistant from each other.
There is a shovel here.
{% endif %}
```
{% endraw %}

### Subtract Two Custom Attributes to Display the Difference as a Monetary Value {#attribute-monetary-difference}

This use case captures two monetary custom attributes, then calculates and displays the difference to let users know how far they have to reach their goal.

{% raw %}
```liquid
{% assign event_goal = {{custom_attribute.${last_selected_event_personal_goal}}} %}
{% assign current_raised =  {{custom_attribute.${last_selected_event_personal_amount_raised}}} %}
{% assign difference =  event_goal | minus: current_raised %}
You only have ${{ difference | round: 0 | number_with_delimiter }} left to raise!
{% endif %}
```
{% endraw %}

### Reference a User's First Name if Their Full Name is Stored in the first_name Field {#attribute-first-name}

This use case captures a user's first name (if both first and last name are stored in a single field) and then uses this first name to display a welcome message.

{% raw %}
```liquid
{{${first_name} | truncatewords: 1, "" | default: 'hi'}}
{% assign name = {{${first_name}}} | split: ' ' %}
Hi {{name[0]}}, here's your message!
```
{% endraw %}

{% endapi %}

{% api %}

## Custom Event

{% apitags %}
Custom Event
{% endapitags %}

- [Abort push notification if a custom event is within two hours of now](#event-abort-push)
- [Send a campaign each time a user performs a custom event three times](#event-three-times)
- [Send a message to users who have only purchased from one category](#event-purchased-one-category)

### Abort Push Notification if a Custom Event is Within Two Hours of Now {#event-abort-push}

This use case calculates the time until an event, and depending on the amount of time left, will display different personalized messages.

For example, you may want to prevent a push from going out if a custom event property will pass in the next two hours. This example uses the scenario of an abandoned cart for a train ticket.

{% raw %}
```liquid
{% assign today =  'now' | date: "%s"  %}
{% assign dep_time = {{event_properties.${outboundDate_Time}}} | date: "%s" %}
{% assign time_to_dep = dep_time | minus: today %}
{% if {{time_to_dep}} <= 7200 %}
{% abort_message("OutboundDate less than 2 hours") %}
{% elsif {{time_to_dep}} > 7200 and {{time_to_dep}} < 86400 %}
Don’t forget to buy your ticket to {{event_properties.${toStation}}} within next 24 hours
{% else %}
Still traveling to {{event_properties.${toStation}}} in more than 24 hours? Book now
{% endif %}
```
{% endraw %}

### Send a Campaign Each Time a User Performs a Custom Event Three Times {#event-three-times}

This use case checks if a user has performed a custom event three times, and if so, will display a message or send a campaign. 

{% raw %}
```liquid
{% assign cadence = custom_attribute.${example} | minus: 1 | modulo: 3 %}
{% if custom_attribute.${example} == blank %}
{% abort_message('error calculating cadence') %}
{% elsif cadence != 0 %}
{% abort_message('skip message') %}
{% endif %}
Did you forget something in your shopping cart?
```
{% endraw %}

{% alert important %} You must have an event property of the custom event count or use a webhook to your Braze endpoint. This is to increment a custom attribute (`example_event_count`) every time the user performs the event. This example uses a cadence of three (1, 4, 7, 10, etc.).{% endalert %}

### Send a Message to Users Who Have Only Purchased from One Category {#event-purchased-one-category}

This use case captures a list of the categories a user has purchased from, and if only one purchase category exists, it will display a message.

{% raw %}
```liquid
{% assign category = {{custom_attribute.${categories_purchased}}} %}
{% assign uniq_cat = {{category | uniq }} %}
{% if {{uniq_cat | size}} == 1%}
{{uniq_cat}}
{% else %}
{% abort_message() %}
{% endif %}
```
{% endraw %}

{% endapi %}

{% api %}

## Language

{% apitags %}
Language
{% endapitags %}

- [Display month names in a different language](#language-display-month)
- [Personalize messaging based on day of the week and user's language](#language-personalize-message)

### Display Month Names in a Different Language {#language-display-month}

This use case will display the current date, month, and year, with the month in a different language. The example provided uses Swedish.

{% raw %}
```liquid
{% assign day = 'now' | date: "%e" %}
{% assign year =  'now' | date: "%Y" %}
{% assign month =  'now' | date: "%B" %}

{% if {{month}} == 'January' %}
{{day}} Januari {{year}}
{% elsif {{month)) == 'February' %}
{{day}} Februari {{year}}
{% elsif {{month)) == 'March' %}
{{day}} Mars {{year}}
{% elsif {{month)) == 'April' %}
{{day}} April {{year}}
{% elsif {{month)) == 'May' %}
{{day}} Maj {{year}}
{% elsif {{month)) == 'June' %}
{{day}} Juni {{year}}
{% elsif {{month)) == 'July' %}
{{day}} Juli {{year}}
{% elsif {{month)) == 'August' %}
{{day}} Augusti {{year}}
{% elsif {{month)) == 'September' %}
{{day}} September {{year}}
{% elsif {{month)) == 'October' %}
{{day}} Oktober {{year}}
{% elsif {{month)) == 'November' %}
{{day}} November {{year}}
{% elsif {{month)) == 'December' %}
{{day}} December {{year}}
{% endif %}
```
{% endraw %}

### Personalize Messaging Based on Day of the Week and User's Language {#language-personalize-message}

This use case checks the current day of the week and, based on the day, if the user's language is set to one of the language options provided, it will display a specific message in their language.

The example provided stops on Tuesday but can be repeated for each day of the week.

{% raw %}
```liquid
{% assign today  = 'now' | date: "%A" %}

{% if today == 'Monday' %}
{% if ${language} == 'es' %}
Compra hoy y lleva tu aprendizaje de idiomas a niveles más altos. 🚀

{% elsif ${language} == 'en' %}
Purchase today and take your language learning to the next level. 🚀

{% elsif ${language} == 'zh' %}
今天就购买并将您的语言提高到一个新水平吧。🚀

{% else %}
It's Monday, but the language doesn't match 
{% endif %}

{% elsif today == 'Tuesday' %}

{% if ${language} == 'zh' %}
不要忘记解锁以获取完整版本哦。🔓

{% elsif ${language} == 'en' %}
Don't forget to unlock the full version of your language. 🔓

{% elsif ${language} == 'ja' %}
すべての機能を使ってみませんか 🔓

{% elsif ${language} == 'es' %}
No te olivides de desbloquear la versión completa del programa de idiomas. 🔓

{% else %}
tuesday default
{% endif %}
{% endif %}
```
{% endraw %}

{% endapi %}

{% api %}

## Miscellaneous

{% apitags %}
Miscellaneous
{% endapitags %}

- [Capitalize the first letter of every word in a string](#misc-capitalize-words-string)
- [Compare custom attribute value against an array](#misc-compare-array)
- [Create an upcoming event reminder](#misc-event-reminder)
- [Find a string within an array](#misc-string-in-array)
- [Find the largest value in an array](#misc-largest-value)
- [Find the smallest value in an array](#misc-smallest-value)
- [Query the end of a string](#misc-query-end-of-string)
- [Query values in an array from a custom attribute with multiple combinations](#misc-query-array-values)

### Capitalize the First Letter of Every Word in a String {#misc-capitalize-words-string}

This use case takes a string of words, splits them into an array, and capitalizes the first letter of each word.

{% raw %}
```liquid
{% assign words_array = {{custom_attribute.${address}}} | split: ' ' %}
{% for words in {{words_array}} %}
{{ words | capitalize | append: ' ' }}
{% endfor %} 
```
{% endraw %}

### Compare Custom Attribute Value Against an Array {#misc-compare-array}

This use case takes a list of favorite stores, checks if any of a user's favorite stores are in that list, and if so, will display a special offer from those stores.

{% raw %}
```liquid
{% assign favorite_stores = 'Target,Walmart,Costco' | split: ',' %}
{% for store in favorite_stores %}
{% if {{custom_attribute.${favorited_stores}}} contains {{store}} %}
Today's offer from {{store}}

{% break %}

{% else %}
{% abort_message("No Attribute Found") %}
{% endif %}
{% endfor %}
```
{% endraw %}

{% alert important %} This sequence has a `break` tag in the primary conditional statement. This causes the loop to stop when a match is found. If you want to display many or all matches, remove the `break` tag. {% endalert %}

### Create an Upcoming Event Reminder {#misc-event-reminder}

This use case allows users to set up upcoming reminders based on custom events. The example scenario allows a user to set a reminder for a policy renewal date that is 26 or more days away, where reminders are sent 26, 13, 7, or 2 days before the policy renewal date.

{% raw %}
```liquid
{% comment %}
Depending on how the reminder_capture property is passed to Braze, with/without a timestamp, the number of days could impact whether a user falls on either side of the 26/13/7/2-day windows.
Once users have been assigned to a Reminder journey/flow, they are then scheduled to enter a subsequent Canvas.
This 'Event Listener' can be used to split out users into different journeys based on the Custom Event properties sent to Braze.
{% endcomment %}

{% comment %}
When testing, ensure the Campaign ID, Campaign API Endpoint, Canvas ID, Canvas API Endpoint are entered correctly. In this example, Canvas ID and Canvas API endpoint have been set up for sharing with the client; in practice, this can be testing using a Campaign ID and Campaign API endpoint.
{% endcomment %}

{% comment %}
The following step calculates how much there is between today's date and the Reminder Date as 'time_to_reminder'.
{% endcomment %}

{% assign today = "now" | date: "%s" %}
{% assign reminder_start_date = {{event_properties.${reminder_date}}} | date: "%s" %}
{% assign time_to_reminder = reminder_start_date | minus: today %}

{% comment %}
The following step checks if the time_to_reminder is more than 26 days away; if this is true, then the user is scheduled to enter the subsequent Canvas 26 days before the reminder_date.
The time is converted from 'seconds from 1970' to the appropriate Reminder Date in the required ISO 8601 format.
N.B. Additional time zones would need to be catered for by adding an additional API Schedule property of "in_local_time"
{% endcomment %}

{% if {{time_to_reminder}} > 2246400 %}
{% assign time_to_first_message = reminder_start_date | plus: 2246400 %}
{{ time_to_first_message | date: "%Y-%m-%dT%H:%M" }}
{
"canvas_id": "954e15bc-af93-9dc8-a863-ad2580f1750e",
"recipients": [
{
"external_user_id": "{{${user_id}}}"
}
],
"trigger_properties" : {
"enquiry_id" : "{{event_properties.${reminder_id}}}",
"reminder_date" : "{{event_properties.${reminder_date} | date: "%Y-%m-%dT%H:%M:%S+0000}}",
"message_personalisation_X" : "{{event_properties.${property_x}}}",
"message_personalisation_Y" : "{{event_properties.${property_x}}}",
"message_personalisation_Z" : "{{event_properties.${property_z}}}"
},

"schedule": {
"time": "{{ time_to_first_message | date: "%Y-%m-%dT%H:%M:%S+0000" }}"
}
}

{% comment %}
The following step checks if the time_to_reminder is less than 26 days away but more than 13 days away.
Users are scheduled to enter the journey on day 13.
{% endcomment %}

{% elsif 1123200 < {{time_to_reminder}} and {{time_to_reminder}} < 2246399 %}
{% assign time_to_first_message = reminder_start_date | plus: 1123200 %}

{
"canvas_id": "954e15bc-af93-9dc8-a863-ad2580f1750e",
"recipients": [
{
"external_user_id": "{{${user_id}}}"
}
],
"trigger_properties" : {
"enquiry_id" : "{{event_properties.${reminder_id}}}",
"reminder_date" : "{{event_properties.${reminder_date} | date: "%Y-%m-%dT%H:%M:%S+0000}}",
"message_personalisation_X" : "{{event_properties.${property_x}}}",
"message_personalisation_Y" : "{{event_properties.${property_x}}}",
"message_personalisation_Z" : "{{event_properties.${property_z}}}"
},

"schedule": {
"time": "2021-03-24T20:04:00+0000"
}
}

{% comment %}
The following step checks if the time_to_reminder is less than 13 days away but more than seven days away.
Users are scheduled to enter the journey on day 7.
{% endcomment %}

{% elsif 604800 < {{time_to_reminder}} and {{time_to_reminder}} < 1123199 %}
{% assign time_to_first_message = reminder_start_date | plus: 604800 %}

{
"canvas_id": "954e15bc-af93-9dc8-a863-ad2580f1750e",
"recipients": [
{
"external_user_id": "{{${user_id}}}"
}
],
"trigger_properties" : {
"enquiry_id" : "{{event_properties.${reminder_id}}}",
"reminder_date" : "{{event_properties.${reminder_date} | date: "%Y-%m-%dT%H:%M:%S+0000}}",
"message_personalisation_X" : "{{event_properties.${property_x}}}",
"message_personalisation_Y" : "{{event_properties.${property_x}}}",
"message_personalisation_Z" : "{{event_properties.${property_z}}}"
},

"schedule": {
"time": "{{ time_to_first_message | date: "%Y-%m-%dT%H:%M:%S+0000" }}"
}
}

{% comment %}
The following step checks if the time_to_reminder is less than seven days away but more than two days away.
Users are scheduled to enter the journey on day 2.
{% endcomment %}

{% else {{time_to_reminder}} < 604799 and {{time_to_reminder}} > 172860 %}
{% assign time_to_first_message = reminder_start_date | plus: 172800 %}

{
"canvas_id": "954e15bc-af93-9dc8-a863-ad2580f1750e",
"recipients": [
{
"external_user_id": "{{${user_id}}}"
}
],
"trigger_properties" : {
"enquiry_id" : "{{event_properties.${reminder_id}}}",
"reminder_date" : "{{event_properties.${reminder_date} | date: "%Y-%m-%dT%H:%M:%S+0000}}",
"message_personalisation_X" : "{{event_properties.${property_x}}}",
"message_personalisation_Y" : "{{event_properties.${property_x}}}",
"message_personalisation_Z" : "{{event_properties.${property_z}}}"
},

"schedule": {
"time": "{{ time_to_first_message | date: "%Y-%m-%dT%H:%M:%S+0000" }}"
}
}
{% endif %}
```
{% endraw %}

{% alert important %} 

You will need a custom event `reminder_capture`, and the custom event properties must include at least:

- `reminder-id`: Identifier of the custom event
- `reminder_date`: User-submitted date when their reminder is due
- `message_personalisation_X`: Any properties needed to personalize the message at the time of sending

{% endalert %}

### Find a String Within an Array {#misc-string-in-array}

This use case checks if a custom attribute array contains a specific string, and if it exists, will display a specific message.

{% raw %}
```liquid
{% if custom_attribute.${PartnershipProgramsNotLinked} contains 'Hertz' %}
Link your Hertz account to use Hertz Fast Lane.

{% elsif custom_attribute.${airportCompleted} == false %}
Clear helps you breeze through airport security. Complete your one-time in-person setup next time you are at the airport. It only takes about 5 minutes.

{% else %}
Your account is all setup
{% endif %}
```
{% endraw %}

### Find the Largest Value in an Array {#misc-largest-value}

This use case calculates the highest value in a given custom attribute array to use in user messaging.

For example, you may want to show a user what the current high score is or the highest bid on an item.

{% raw %}
```liquid
{% assign maxValue = 0 %}
{% for attribute in {{custom_attribute.${array_attribute}}} %}
{% assign compareValue = {{attribute | plus: 0}} %}
{% if compareValue > maxValue %}
{% assign maxValue = compareValue %}
{% endif %}
{% endfor %}
{{maxValue}}
```
{% endraw %}

{% alert important %}
You must use a custom attribute that has an integer value and is part of an array (list). {% endalert %}

### Find the Smallest Value in an Array {#misc-smallest-value}

This use case calculates the lowest value in a given custom attribute array to use in user messaging.

For example, you may want to show a user what the lowest score is or the cheapest item.

{% raw %}
```liquid
{% assign minValue = custom_attribute.${array_attribute}[0] | plus: 0 %}
{% for attribute in {{custom_attribute.${array_attribute}}} %}
{% assign compareValue = {{attribute | plus: 0}} %}
{% if compareValue < minValue %}
{% assign minValue = compareValue %}
{% endif %}
{% endfor %}
{{minValue}}
```
{% endraw %}

{% alert important %} You must use a custom attribute that has an integer value and is part of an array (list). {% endalert %}

### Query the End of a String {#misc-query-end-of-string}

This use case queries the end of a string to use in messaging.

{% raw %}
```liquid
{% assign interest = {{custom_attribute.${Buyer Interest}} | first } %}
{% assign marketplace = {{{{interest}} | split: "" | reverse | join: "" |  truncate: 4, ""}} %}
{% if {{marketplace}} == '3243' %}

Your last marketplace search was on {{custom_attribute.${Last marketplace buyer interest} | date: '%d.%m.%Y'}}. Check out all of our new offers.

{% else %}
{% abort_message() %}
{% endif %}
```
{% endraw %}

### Query Values in an Array From a Custom Attribute with Multiple Combinations {#misc-query-array-values}

This use case takes a list of soon-to-be-expired shows, checks if any of a user's favorite shows are in that list, and if so, will display a message notifying the user that they will expire soon.

{% raw %} 
```liquid
{% assign expired_shows = 'Modern Family,The Rookie,Body of Proof,Felicity' | split: ',' %}
{% for show in expired_shows %}
{% if {{custom_attribute.${Favorite Shows}}} contains {{show}} %}
{% assign new_shows = new_shows | append: {{show}} | append: '*' %}
{% endif %}
{% endfor %}
{% assign new_shows_clean = new_shows | split: '*' %}
{% if new_shows_clean.size != 0 %}

All episodes of {{new_shows_clean | join: ', ' }} expire on 9/8 - watch them now before they're gone!

{% else %}
{% abort_message("Not Found") %}
{% endif %}
```
{% endraw %}

{% alert important %} You will need to find matches between the arrays first, then build logic at the end to split up the matches. {% endalert %}


{% endapi %}

{% api %}

## Platform Targeting

{% apitags %}
Platform Targeting
{% endapitags %}

- [Differentiate in-app message copy by device OS](#platform-device-os)
- [Target only a specific platform](#platform-target)
- [Target only iOS devices with a specific OS version](#platform-target-ios-version)
- [Target only Web browsers](#platform-target-web)
- [Target a specific mobile carrier](#platform-target-carrier)

### Differentiate In-App Message Copy by Device OS {#platform-device-os}

This use case checks what platform a user is on, and depending on their platform, will display specific messaging.

For example, you may want to show mobile users shorter versions of message copy while showing other users the regular, longer version of the copy. You could also show mobile users certain messaging relevant to them but wouldn’t be relevant to Web users. For example, iOS messaging might talk about Apple Pay, but Android messaging should mention Google Pay.

{% raw %}
```liquid
{% if targeted_device.${platform} == "ios" or targeted_device.${platform} == "android" %}
This is a shorter copy.

{% else % %}
This is the regular copy and much longer than the short version. 
{% endif %}
```
{% endraw %}

{% raw %}
{% alert note %} 
Liquid is case-sensitive, `targeted_device.${platform}` returns the value in all lowercase. 
{% endalert %}
{% endraw %}

### Target Only a Specific Platform {#platform-target}

This use case will capture the users' device platform, and depending on the platform, will display a message.

For example, you may want to only send a message to Android users. This can be used as an alternative to selecting an app within the Segmentation tool.

{% raw %}
```liquid
{% if {{targeted_device.${platform}}} == 'android' %} 

This is a message for an Android user! 

{% else %}  
{% abort_message %] 
{% endif %}
```
{% endraw %}

### Target Only iOS Devices with a Specific OS Version {#platform-target-ios-version}

This use case checks if a user's OS version falls within a certain set of versions and if so, will display a specific message.

The example used sends a warning to users on iOS 10.0 or below that they are phasing out support for the user’s device OS.

{% raw %}
```liquid
{% if {{targeted_device.${os}}} == "10.0" or {{targeted_device.${os}}} == "10.0.1" or {{targeted_device.${os}}} == "10.0.2" or {{targeted_device.${os}}} == “10.0.3” or {{targeted_device.${os}}} == “10.1” or {{targeted_device.${os}}} == “10.2” or {{targeted_device.${os}}} == “10.2.1” or {{targeted_device.${os}}} == “10.3” or {{targeted_device.${os}}} == “10.3.1” or {{targeted_device.${os}}} == “10.3.2” or {{targeted_device.${os}}} == “10.3.3” or {{targeted_device.${os}}} == “10.3.4” or {{targeted_device.${os}}} == “9.3.1” or {{targeted_device.${os}}} == “9.3.2” or {{targeted_device.${os}}} == “9.3.3” or {{targeted_device.${os}}} == “9.3.4” or {{targeted_device.${os}}} == "9.3.5" %}

We are phasing out support for your device's operating system. Be sure to update to the latest software for the best app experience.

{% else%}
{% abort_message%}
{% endif %}
```
{% endraw %}

### Target Only Web Browsers {#platform-target-web}

This use case checks if a user's target device runs on Mac or Windows and, if so, will display a specific message.

{% raw %}
```liquid
{% if {{targeted_device.${os}}} == 'Mac' OR {{targeted_device.${os}}} == 'Windows' %}

This message will display on your desktop web browser.

{% else %}
{% abort_message %}
{% endif %}
```
{% endraw %}

### Target a Specific Mobile Carrier {#platform-target-carrier}

This use case checks if a user's device carrier is Verizon, and if so, will display a specific message.

For push notifications and in-app message channels, you can specify the device carrier in your message body using Liquid. If the recipient’s device carrier doesn’t match, the message won’t be sent.

{% raw %}
```liquid
{%if {targeted_device.${carrier}} contains "verizon" or {targeted_device.${carrier}} contains "Verizon" %}

This is a message for Verizon users!

{% else %}
{% abort_message %}
{% endif %}
```
{% endraw %}

{% endapi %}

{% api %}

## Time Zones

{% apitags %}
Time Zones
{% endapitags %}

- [Append the CST timezone to a custom attribute](#time-append-cst)
- [Insert a timestamp](#time-insert-timestamp)
- [Only send a Canvas push during a window of time in a user's local time zone](#time-canvas-window)
- [Send a reoccurring in-app message campaign between a window of time in a user's local time zone](#time-reocurring-iam-window)
- [Send different messages on weekdays vs. weekends in a user's local time zone](#time-weekdays-vs-weekends)
- [Send different messages based on time of day in a user's local time zone](#time-of-day)

### Append the CST Timezone to a Custom Attribute {#time-append-cst}

This use case displays a custom date attribute in a given time zone.

Option 1:
{% raw %}
```liquid
{{custom_attribute.${application_expires_date} | time_zone: -0005 | date: '%B, %d %Y' }}
```
{% endraw %}

Option 2:
{% raw %}
```liquid
{{custom_attribute.${application_expires_date} | time_zone: 'America/Chicago' | date: '%B %d %Y %z' }}
```
{% endraw %}

### Insert a Timestamp {#time-insert-timestamp}

This use case displays a message that includes a timestamp in their current time zone.

The example provided below will display the date as YYYY-mm-dd HH:MM:SS, such as 2021-05-03 10:41:04.

{% raw %}
```liquid
{{${user_id} | default: 'You'}} received a campaign, rendered at ({{ "now" | timezone: ${time_zone} | date: "%Y-%m-%d %H:%M:%S" }})
```
{% endraw %}

### Only Send a Canvas Push During a Window of Time in a User's Local Time Zone {#time-canvas-window}

This use case checks a user's time in their local time zone, and if it falls within a set time, it will display a specific message.

{% raw %}
```liquid
{% assign time = 'now' | time_zone: ${time_zone} %}
{% assign hour = time | date: '%H' | plus: 0 %}
{% if hour > 20 or hour < 8 %}
{% abort_message("Outside allowed time window") %}
{% endif %}

Here's a message that will send between 8 am and 8 pm!
```
{% endraw %}

### Send a Reoccuring In-App Message Campaign Between a Window of Time in a User's Local Time Zone {#time-reocurring-iam-window}

This use case will display a message if a user's current time falls within a set window.

For example, the scenario below lets a user know that a store is closed.

{% raw %}
```liquid
{% assign time = 'now' | time_zone: ${time_zone} %} 
{% assign hour = time | date: '%H' | plus: 0 %}
{% if hour > 21 or hour < 10 %}

Store's closed. Come back between 11 am and 9 pm!

{% else%} 
{% abort_message("not sent because the store is open") %}
{% endif %}
```
{% endraw %}

### Send Different Messages on Weekdays vs. Weekends in a User's Local Time Zone {#time-weekdays-vs-weekends}

This use case will check if a user's current day of the week is Saturday or Sunday, and depending on the day, will display different messages.

{% raw %}
```liquid
{% assign today = 'now' | time_zone: ${time_zone} | date: "%A" %}
{% if {{today}} == 'Saturday' or {{today}} == 'Sunday' %}
It’s {{today}}, why don’t you open the app for your transactions?

{% else %}
It’s {{today}}, why don’t you visit the store?
{% endif %}
```
{% endraw %}

### Send Different Messages Based on Time of Day in a User's Local Time Zone {#time-of-day}

This use case will display a message if a user's current time falls outside a set window.

For example, you may want to tell a user about a time-sensitive opportunity that depends on the time of day.

{% raw %}
```liquid
{% assign time = 'now' | time_zone: ${time_zone} %}
{% assign hour = time | date: '%H' | plus: 0 %}
{% if hour > 20 or hour < 8 %}
{% abort_message("Outside allowed time window") %}
{% endif %}

Check out this new bar after work today. HH specials!
```
{% endraw %}

{% alert note %} This is the opposite of [Quiet Hours]({{site.baseurl}}/user_guide/engagement_tools/campaigns/scheduling_and_organizing/time_based_campaign/#time-based-functionalities-for-campaigns). {% endalert %}

{% endapi %}

{% api %}

## Week/Day/Month

{% apitags %}
Week/Day/Month
{% endapitags %}

- [Pull the previous month's name into a message](#month-name)
- [Send a campaign at the end of every month](#month-end)
- [Send a campaign on the last (weekday) of the month](#day-of-month-last)
- [Send a different message each day of the month](#day-of-month)
- [Send a different message each day of the week](#day-of-week)

### Pull the Previous Month's Name into a Message {#month-name}

This use case will take the current month and display the previous month to be used in messaging.

{% raw %}
```liquid
{% assign today = 'now' | date: "%m" %}
{% assign last_month = {{today}} | minus: 1 %}
{% if last_month == 1 %}
{% assign month = "January" %}
{% elsif last_month == 2 %}
{% assign month = "February" %}
{% elsif last_month == 3 %}
{% assign month = "March" %}
{% elsif last_month == 4 %}
{% assign month = "April" %}
{% elsif last_month == 5 %}
{% assign month = "May" %}
{% elsif last_month == 6 %}
{% assign month = "June" %}
{% elsif last_month == 7 %}
{% assign month = "July" %}
{% elsif last_month == 8 %}
{% assign month = "August" %}
{% elsif last_month == 9 %}
{% assign month = "September" %}
{% elsif last_month == 10 %}
{% assign month = "October" %}
{% elsif last_month == 11 %}
{% assign month = "November" %}
{% elsif last_month == 12 %}
{% assign month = "December" %}
{% endif %}

Here's an overview of what your spending looked like in {{month}}.
```
{% endraw %}

### Send a Campaign at the End of Every Month {#month-end}

This use case will check if the current date falls within a list of dates, and depending on the date, will display a specific message.

{% alert note %} This does not account for leap years (February 29). {% endalert %}

{% raw %}
```liquid
{% assign current_date = 'now' | date: '%b %d' %}

{% if current_date == "Jan 31" or current_date == "Feb 28" or current_date == "Mar 31" or current_date == "Apr 30" or current_date == "May 31" or current_date == "Jun 30" or current_date == "Jul 31" or current_date == "Aug 31" or current_date == "Sep 30" or current_date == "Oct 31" or current_date == "Nov 30" or current_date == "Dec 31" %}

The date is correct

{% else %}
{% abort_message() %}
{% endif %}
```
{% endraw %}

### Send a Campaign on the Last (Weekday) of the Month {#day-of-month-last}

This use case captures the current month and day and calculates if the current day falls within the last weekday of the month.

For example, you may want to send a survey to your users on the last Wednesday of the month asking for product feedback.

{% raw %}
```liquid
{% comment %}Pull the day, day name, month, and year from today's date.{% endcomment %}
{% assign current_day = "now" | date: "%d" %}
{% assign current_day_name = "now" | date: "%a" %}
{% assign current_month = "now" | date: "%b" %}
{% assign current_year = "now" | date: "%Y" %}

{% comment %}Assign the correct number of days for the current month.{% endcomment %}

{% if current_month == "Jan" %}
{% assign last_day_of_month = 31 %}
{% elsif current_month == "Mar" %}
{% assign last_day_of_month = 31 %}
{% elsif current_month == "Apr" %}
{% assign last_day_of_month = 30 %}
{% elsif current_month == "May" %}
{% assign last_day_of_month = 31 %}
{% elsif current_month == "Jun" %}
{% assign last_day_of_month = 30 %}
{% elsif current_month == "Jul" %}
{% assign last_day_of_month = 31 %}
{% elsif current_month == "Aug" %}
{% assign last_day_of_month = 31 %}
{% elsif current_month == "Sep" %}
{% assign last_day_of_month = 30 %}
{% elsif current_month == "Oct" %}
{% assign last_day_of_month = 31 %}
{% elsif current_month == "Nov" %}
{% assign last_day_of_month = 30 %}
{% elsif current_month == "Dec" %}
{% assign last_day_of_month = 31 %}
{% endif %}

{% comment %}Assign the correct number of days if the current month is February, taking into account leap years.{% endcomment %}

{% assign leap_year_remainder = {{current_year | modulo: 4 }} != "0" %}
{% if leap_year_remainder == 0 and current_month == "Feb" %}
{% assign last_day_of_month = 29 %}
{% elsif leap_year_remainder != "0" and current_month == "Feb" %}
{% assign last_day_of_month = 28 %}
{% endif %}

{% comment %}Check that today's date is within a week of the last day of the month. If not, abort the message. If so, check that today is Wednesday. If not, abort the message.{% endcomment %}

{% assign diff_in_days = last_day_of_month | minus: current_day | plus: 1%}
{% if diff_in_days <= 7 %}
{% unless current_day_name == "Wed" %}
{% abort_message() %}
{% endunless %}
{% else %}
{% abort_message() %}
{% endif %}


```
{% endraw %}

### Send a Different Message Each Day of the Month {#day-of-month}

This use case checks if the current date matches one on a list, and depending on the day, will display a distinct message.

{% raw %}
```liquid
{% assign today = 'now' | time_zone: {{${time_zone}}} | date: "%Y-%m-%d" %}
{% assign day_1 = "2019-12-01" | time_zone: {{${time_zone}}} | date: "%Y-%m-%d" %}
{% assign day_2 = "2019-12-02" | time_zone: {{${time_zone}}} | date: "%Y-%m-%d" %}
{% assign day_3 = "2019-12-03" | time_zone: {{${time_zone}}} | date: "%Y-%m-%d" %}

{% if today == day_1 %}
Message for 2019-12-01

{% elsif today == day_2 %}
Message for 2019-12-02

{% elsif today == day_3%}
Message for 2019-12-03

{% else %}
{% abort_message() %}
{% endif %}
```
{% endraw %}

### Send a Different Message Each Day of the Week {#day-of-week}

This use case checks the current day of the week, and depending on the day, will display a distinct message.

{% raw %}
```liquid
{% assign today = 'now' | date: "%A" %}
{% case ‘today' %}
{% when 'Monday' %}
Monday copy

{% when 'Tuesday' %}
Tuesday copy

{% when 'Wednesday' %}
Wednesday copy

{% when  'Thursday' %}
Thursday copy

{% when  'Friday' %}
Friday copy

{% when 'Saturday' %}
Saturday copy

{% when 'Sunday' %}
Sunday copy

{% else %}
Default copy
{% endcase %}
```
{% endraw %}

{% alert note %} You can replace the line "default copy" with {% raw %}`{% abort_message() %}`{% endraw %} to prevent the message from sending if the day of the week is unknown. {% endalert %}

{% endapi %}

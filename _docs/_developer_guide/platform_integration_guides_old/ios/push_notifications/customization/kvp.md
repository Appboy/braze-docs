---
nav_title: Key-Value Pairs
platform: iOS
page_order: 0.2
description: "This article covers how to extract data from push notification key-value pairs."
channel:
  - push

---

# Extracting Data from Push Notification Key-Value Pairs

Braze allows you to send custom-defined string key-value pairs, known as extras, along with a push notification to your application. Extras can be defined via the dashboard or API and will be available as key-value pairs within the notification dictionary passed to your push delegate implementations.
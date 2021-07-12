---
nav_title: Troubleshooting
platform: FireOS
page_order: 20

page_type: solution
description: "This article provides troubleshooting scenarios for possible issues you may experience with push notifications."
channel: push

---
# Troubleshooting

## Utilizing the Push Error Logs

Braze provides a log of Push Notification Errors within the "Message Activity Log". This error log provides a variety of warnings which can be very helpful for identifying why your campaigns aren't working as expected.  Clicking on an error message will redirect you to relevant documentation to help you troubleshoot a particular incident.

![Push Error Log][11]

## Troubleshooting Scenarios

### No "Push Registered" Users Showing in the Braze Dashboard (Prior to Sending Messages)
  - Ensure that your app is correctly configured to allow push notifications.
  - Ensure that the Client Id and Client Secret configured in your Braze dashboard are correct.

### Push Notifications Not Displayed on Users' Devices
There are a few scenarios why this could be occurring:

  - If you force-quit your application, your push notifications will not be displayed while your app is not running.
  - Make sure the ["Notification Priority"][15] setting is set to "HIGH" in your campaign
  - The ADM API Key in your `api_key.txt` is incorrect or contins invalid characters.
  - The AppboyAdmReceiver is not properly registered in `AndroidManifest.xml` with intent filters for `<action android:name="com.amazon.device.messaging.intent.RECEIVE" />` and `<action android:name="com.amazon.device.messaging.intent.REGISTRATION" />`.

### "Push Registered" Users No Longer Enabled After Sending Messages

  - This typically occurs when users have uninstalled the application, causing their ADM Registration ID to become invalid.

[11]: {% image_buster /assets/img_archive/message_activity_log.png %}
[15]: {{ site.baseurl }}/developer_guide/platform_integration_guides/fireos/push_notifications/advanced_settings/#notification-priority

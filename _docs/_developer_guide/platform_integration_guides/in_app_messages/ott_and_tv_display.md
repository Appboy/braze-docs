---
nav_title: OTT and TV Display
page_order: 5
description: ""
platforms:
- Android
- FireOS
---

# OTT and TV Display

{% tabs %}
{% tab Android & FireOS %}

The Braze Android SDK natively supports displaying In-App Messages on OTT devices like Android TV or Fire Stick.

## Key Differences

Some key differences exist in the display of standard In-App Messages between native and OTT devices.

For OTT devices:

- In-App Messages that require touch mode (such as Slideup) are disabled on OTT.
- The currently selected/focused item, such as a button or close button, will be highlighted.
- Body clicks on the In-App Message itself, i.e. not on a button, are not supported.

{% endtab %}
{% endtabs %}
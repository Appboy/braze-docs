---
nav_title: Tealium Currents Connector
page_order: 3
alias: /partners/tealium_currents/
description: "This article outlines the partnership between Braze Currents and Tealium's Customer Data Platform (CDP)."
page_type: partner
tool: currents

---

# About Tealium & Currents

> [Tealium's Customer Data Hub](https://tealium.com/products/) is a single approach to collecting and managing data and making it available to every customer touchpoint and marketing technology vendor in real-time to fuel exceptional customer experiences and insights.

Braze uses the [GET method of Tealium's HTTP Connect API](https://community.tealiumiq.com/t5/Customer-Data-Hub/Tealium-Collect-HTTP-API/ta-p/16893#toc-hId-584399348). 

## Integration Details

To get started, determine the appropriate URL to use for your Braze data. Then, add this information to the Tealium integration page on the Braze dashboard, and press __save__. 

### Event Details

All fields in all Currents events are supported for Tealium. Please refer to the [Currents Event Glossary]({{site.baseurl}}/user_guide/data_and_analytics/braze_currents/event_glossary/) for a full list. 

#### Sample Event

```json
{
    "event_type": "users.messages.sms.Delivery",
    "id": "a1234567-89ab-cdef-0123-456789abcdef",
    "time": 1477502783,
    "user": {
        "user_id": "0123456789abcdef01234567",
        "external_user_id": "user_id",
        "timezone": "America/Chicago"
    },
    "properties": {
        "canvas_id": "11234567-89ab-cdef-0123-456789abcdef",
        "canvas_name": "My Canvas",
        "canvas_variation_id": "31234567-89ab-cdef-0123-456789abcdef",
        "canvas_step_id": "41234567-89ab-cdef-0123-456789abcdef",
        "dispatch_id": "12345qwert",
        "to_phone_number": "+16462345678",
        "subscription_group_id": "41234567-89ab-cdef-0123-456789abcdef",
        "from_phone_number": "+12123470922"
    }
}
```

All events will contain:
- `event_type` to identify which event is being sent. 
  - For custom events, the `event_type` will be `users.behaviors.CustomEvent`, and the event name will be sent as the `name` property.
- `user` object with information about the user.
- `properties` object with event-specific parameters. 
  - For events that support additional custom parameters, an additional object will be included within the `properties` object to reflect these properties.
  - For purchase events, this field is `purchase_properties`.
  - For custom events, this field is `custom_properties`.


[support]: {{site.baseurl}}/support_contact/

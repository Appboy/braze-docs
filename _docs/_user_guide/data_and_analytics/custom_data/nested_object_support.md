---
nav_title: Nested Object Support
page_title: Nested Object Support for Custom Event Properties
page_order: 4
alias: /nested_object_support/
page_type: reference
description: "This reference article describes nested object support for custom event properties, and includes example use cases, limitations, and frequently asked questions."
---

# Nested Object Support for Custom Event Properties

Nested Object Support allows you to send nested JSON data as properties of custom events and purchases. This nested data can be used for templating personalized information in messages, for triggering message sends, and for segmentation.

## Limitations

- Nested data can only be sent with [custom events]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/custom_events/) and [purchase events]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/purchase_events/). This is not yet supported with user attributes.
- Event property objects that contain array or object values can have an event property payload of up to 50KB.
- Available on events/purchases sent via API only, the Braze SDKs are not yet supported.

## Usage Examples

### API Request Body

{% tabs %}
{% tab Music Example %}

Shown below is a `/users/track` example with a "Created Playlist" custom event. Once a playlist has been created, to capture the properties of the playlist, we will send an API request that lists "songs" as a property, and an array of the nested properties of the songs.

```
...
"properties": {
  "songs": [
    {
      "title": "Smells Like Teen Spirit",
      "artist": "Nirvana",
      "album": {
        "name": "Nevermind",
        "yearReleased": "1991"
      }
    },
    {
      "title": "While My Guitar Gently Weeps",
      "artist": "the Beatles",
      "album": {
        "name": "The Beatles",
        "yearReleased": "1968"
      }
    }
  ]
}
...
```
{% endtab %}
{% tab Restaurant Example%}

Shown below is a `/users/track` example with an "Ordered" custom event. Once an order has been completed, to capture properties of that order, we will send an API request that lists "r_details" as a property, and the nested properties of that order.

```
...
"properties": {
  "r_details": {
    "name": "McDonalds",
    "identifier": "12345678",
    "location" ; {
      "city": "Montclair",
      "state": "NJ"
    }
  }
}
...
```
{% endtab %}
{% endtabs %}

### Liquid Templating

The Liquid templating examples below show how to reference the nested properties saved from the above API request and use them in your Liquid messaging. Using Liquid and dot notation, traverse the nested data to find the specific node you would like to include in your messages.

{% tabs local %}
{% tab Music Example %}
Templating in Liquid in a message triggered by the "Created Playlist" event:

{% raw %}
`{{event_properties.${songs}[0].album.name}}`: "Nevermind"<br>
`{{event_properties.${songs}[1].title}}`: "While My Guitar Gently Weeps"
{% endraw %}

{% endtab %}
{% tab Restaurant Example %}
Templating in Liquid in a message triggered by the "Ordered" event:

{% raw %}
`{{event_properties.${r_details}.location.city}}`: "Montclair"
{% endraw %}

{% endtab %}
{% endtabs %}

### Message Triggering

To use these properties to trigger a campaign, select your custom event or purchase, and add a __Nested Property__ filter.

{% tabs %}
{% tab Music Example %}

Triggering a campaign with nested properties from the "Created Playlist" event:

![Triggering Campaign]({% image_buster /assets/img/nested_object2.png %})

The trigger condition `songs[].album.yearReleased` "is" "1968" will match an event where any of the songs have an album released in 1968. We use the bracket notation `[]` for traversing through arrays, and match if __any__ item in the traversed array matches the event property.<br>
{% endtab %}
{% tab Restaurant Example %}

Triggering a campaign with nested properties from the "Ordered" event:

![Triggering Campaign]({% image_buster /assets/img/nested_object1.png %})

`r_details.name`: "Mcdonalds"<br>
`r_details.location.city`: "Montclair"
{% endtab %}
{% endtabs %}

{% alert note %} Triggering is not yet supported for In-App Messages.  {% endalert %}
{% alert note %} If your event property contains the `[]` or `.` characters, escape them by wrapping the chunk in double-quotes. For instance, `"songs[].album".yearReleased` will match an event with the literal property `"songs[].album"`.  {% endalert %}

### Segmentation

Use [Segment Extensions]({{site.baseurl}}/user_guide/engagement_tools/segments/segment_extension/) to segment users based on nested event properties. Segmentation uses the same notation as triggering (described above).

## Frequently Asked Questions

### Does this consume additional data points?

There is no change in how we charge data points as a result of adding this capability.

### How much nested data can be sent?

If one or more of the event's properties contains nested data, the maximum payload for all combined properties on an event is 50 KB. Any request over that size limit will be rejected.

[1]: {% image_buster /assets/img/nested_object1.png %}
[2]: {% image_buster /assets/img/nested_object2.png %}


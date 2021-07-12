---
nav_title: Read & Unread Indicators
page_order: 2

platform: Web
page_type: reference
description: "This reference article covers read and unread indicators in Content Cards."
channel: content cards

---

# Read & Unread Indicators

Braze provides indicators on Content Cards as pictured below:

|Indicator|Example |
|---|---|
| Read | ![ReadContentCard][2] |
| Unread | ![UnreadContentCard][3] |
{: .reset-td-br-1 .reset-td-br-2}

## Changing colors

To change the color of the unread indicator of a card, add custom CSS to your webpage. For example, changing it to green with the following CSS:

```css
.ab-unread-indicator { background-color: green !important; }
```

## Disabling the Indicators

In order to disable this functionality add the following style to your `css`:

```css
.ab-unread-indicator { display: none; }
```

[2]:{% image_buster /assets/img_archive/readcontentcard.png %}
[3]:{% image_buster /assets/img_archive/unreadcontentcard.png %}

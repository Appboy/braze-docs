---
nav_title: Editor Blocks
alias: "/dnd/editor_blocks/"
channel: email
page_order: 2
description: "This reference article covers the different editor blocks that are provided in the email Drag & Drop editor."
---

# Drag & Drop Editor Blocks

Editor blocks are the various blocks available in the Drag & Drop Editor under the 'Content' section.  This section includes a series of tiles that represent the different kinds of content you can use in your message. More will become available in the future.

To use them, just drag one inside a column, it will auto-adjust to the column width.  Every content block has its owns settings, such as granular control on padding. The right-side panel automatically switches to a property panel for the selected content element.

## Block types

| Name | Description |
|---|---|
| `Title`  | Allows users to add text with H1,H2,H3 tags, for email. | 
| `Text`  |  Allows users to enter Text into their message.  A toolbar helps with font and text editing functionality. | 
| `Image` | Allows you to insert an image from the Media Library. | 
| `Button` |  Add a standard button.  Properties for this block allow for editing and setting links easily.  | 
| `Divider` |  Insert a solid, dotted, or dashed line to help with spacing.|
| `HTML` |  Insert raw HTML.  Great for advanced Liquid such as connected content or conditional statements. | 
| `Menu` |  Create a flexible menu for the message you're designing. |
{: .reset-td-br-1 .reset-td-br-2} 

## Block type properties
Details for each block type are provided below

### Title

| Properties | Description |
|---|---|
| `Title`  | Select the heading style.  H1, H2, or H3 are available only. | 
|`Font Family`| The style to be used for your title|
|`Font Size`| The size of your text |
|`Text Color`| Modifies the color of the title|
|`Link Color`| Modifies the color of the link|
|`Align`| Moves the title to be either left, center or right oriented|
|`Line Height`| Modify the distance between lines of text|
|`Line spacing`| Modify the distance in between each character|
|`Text direction`| Default left to right, but can be edited to write right-to-left|
{: .reset-td-br-1 .reset-td-br-2}

### Text

| Properties | Description |
|---|---|
|`Text Color`| Modifies the color of the title|
|`Link Color`| Modifies the color of the link|
|`Line Height`| Modify the distance between lines of text|
|`Line spacing`| Modify the distance in between each character|
{: .reset-td-br-1 .reset-td-br-2}

### Image

| Properties | Description |
|---|---|
|`Auto Width`| Modifies the px of the image|
|`Align`| Moves the image to be either left, center or right oriented|
|`URL`| The hosted address for your image|
|`Alternate text`| The written copy that appears in place of an image, when the image fails to load|
{: .reset-td-br-1 .reset-td-br-2}

#### _Note about Auto Width_
Automatic image resizing picks the best size for the image based on a combination of image width and available space in the layout:
- Large images, wider than the available space, will be set at 100% width and will keep this ratio on mobile, using the entire device display width.
- Small images, smaller than the available space, will use the image's natural size to avoid distortion effects or blurry pics.

### Button

| Properties | Description |
|---|---|
|`Link Type`| The desired action when clicking the button.  Sets the appropriate link protocol |
|`URL`| Dynamic based on the Link Type chosen.|
{: .reset-td-br-1 .reset-td-br-2}

### Divider

| Properties | Description |
|---|---|
|`Transparent`| If enabled, 'line' and 'width' options are removed. |
|`Line `| The different formats of a line, whether dotted, spotted, or solid.  In addition, you can modify the thickness and color of the divider line|
|`Width `| Adjusts the spread of the divider in increments of 5  |
|`Align`| Moves the line to be either left, center, or right oriented |
{: .reset-td-br-1 .reset-td-br-2}

### HTML

| Properties | Description |
|---|---|
|`html editor`| Enter the raw HTML |
{: .reset-td-br-1 .reset-td-br-2}


### Menu

| Properties | Description |
|---|---|
|`Configure menu items`| Add a menu item  |
|`Font Family`| The style to be used for your menu|
|`Font Size`| The size of your menu |
|`Text Color`| Modifies the color of the menu|
|`Link Color`| Modifies the color of the menu text|
|`Align`| Moves the menu to be either left, center or right oriented|
|`Letter spacing`| Modify the distance in between each character |
|`Layout`| Either horizontal or vertical|
|`Separator`| Add character(s) in between the menu options |
|`Mobile menu`| Options to modify the icon size, color, and icon type when shown on a mobile device|
|`Item padding`| If enabled, you can modify the padding, by using either the + or - button or by inputting a specific number|
|`All sides`| If 'item padding' is disabled, set a consistent padding number|
{: .reset-td-br-1 .reset-td-br-2}

### Add Liquid Personalization

| Name | Description |
|---|---|
| `Add Personalization` |  Located on the left menu.  Allows you to lookup standard liquid snippets such as default attributes, custom attributes, content blocks etc... | 
{: .reset-td-br-1 .reset-td-br-2}

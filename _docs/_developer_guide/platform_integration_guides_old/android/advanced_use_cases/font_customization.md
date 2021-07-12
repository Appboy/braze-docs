---
nav_title: Font Customization
platform: Android
page_order: 7
description: "This reference article covers font customization options such as defining a font family and shows how to reference it throughout your application."

---

# Font Customization

Fonts in the Braze SDK can be set via XML using the AndroidX libraries according to [Font in XML Guide][1]. To use your custom font with the Braze SDK, you'll first need to create a font family.

## Create A Font Family

The following is an example custom font family definition using the [font family guide][2]. For this example, we use the [Bungee Shade Font][3].

```XML
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">

  <!--Note: You must declare both sets of attributes
      to ensure your fonts load on devices running Android 8.0 (API level 26) or lower.
      See https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html -->

  <font android:fontStyle="normal"
        android:fontWeight="400"
        android:font="@font/bungeeshade"

        app:fontStyle="normal"
        app:fontWeight="400"
        app:font="@font/bungeeshade"/>
</font-family>
```

After storing the font family definition in `/res/font/bungee_font_family.xml`, we can refer to it in XML as `@font/bungee_font_family`.

## Referencing Your Font Family

Now that the font family is created, you can override Braze style defaults in your `styles.xml` to include references to the font family.

For example, the following styles override would use the `bungee` font family from above for _all_ Braze in-app messages and a different font family for _all_ Braze News Feed cards.

```
<style name="Appboy.InAppMessage">
  <item name="android:fontFamily">@font/bungee_font_family</item>
  <item name="fontFamily">@font/bungee_font_family</item>
</style>

<style name="Appboy.Cards">
  <item name="android:fontFamily">@font/another_custom_font_family</item>
  <item name="fontFamily">@font/another_custom_font_family</item>
</style>
```

{% alert warning %}
  Both `android:fontFamily` and `fontFamily` style attributes must be set to maintain compatibility across all SDK versions.
{% endalert %}

[1]: https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html
[2]: https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html#font-family
[3]: https://fonts.google.com/specimen/Bungee+Shade

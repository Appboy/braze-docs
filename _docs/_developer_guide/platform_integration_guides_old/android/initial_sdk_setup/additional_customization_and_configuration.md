---
nav_title: Other SDK Customizations
page_order: 3
platform: Android
description: "This article covers additional customization and configuration options such as verbose logging, suppressing loggind, and how to implement multiple API keys."

---

# Additional Customization and Configuration

## Using R8/Proguard with Braze
[Code shrinking][50] configuration is automatically included with your Braze integration.

Client apps that obfuscate Braze code must store release mapping files for Braze to interpret stack traces. If you would like to continue to keep all Braze code, add the following to your Proguard file:

```
-keep class bo.app.** { *; }
-keep class com.appboy.** { *; }
```

## Enabling Verbose Logging {#android-verbose-logging}

Verbose logs from the Braze SDK are essential to a fast turnaround on support issues. These logs should not be modified for clarity; long log files are preferred! Verbose logging is only intended to be used in development environments and should not be enabled in a released application. Logs sent to our support team should begin as soon as the application is launched and should end well after the observed issue occurs.

To enable verbose logging on the Braze Android SDK:

{% tabs %}
{% tab JAVA %}

```java
AppboyLogger.setLogLevel(Log.VERBOSE);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
AppboyLogger.setLogLevel(Log.VERBOSE)
```

{% endtab %}
{% endtabs %}

{% alert important %}
Verbose logs should be enabled as early as possible in your `Application.onCreate()`, before any other calls to the SDK, to guarantee as much logging as possible.
{% endalert %}

To know if your obtained logs are verbose, look for `V/Appboy` somewhere in your logs. For example:

`2077-11-19 16:22:49.591 ? V/Appboy v9.0.01 .bo.app.d3: Request started`

### Suppressing Braze SDK Logging

The default Log Level for the Braze Android SDK is `INFO`.

To change the Braze Log Level, call [`AppboyLogger.setLogLevel()`][70] with one of the [`android.util.Log`][54] constants or `AppboyLogger.SUPPRESS`. For example:

{% tabs %}
{% tab JAVA %}

```java
// Suppress all logs
AppboyLogger.setLogLevel(AppboyLogger.SUPPRESS);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
// Suppress all logs
AppboyLogger.setLogLevel(AppboyLogger.SUPPRESS)
```

{% endtab %}
{% endtabs %}

## Multiple API Keys

The most common use case for multiple API keys is separating API keys for debug and release build variants.

To easily switch between multiple API keys in your builds, we recommend creating a separate `braze.xml` file for each relevant [build variant][3]. A build variant is a combination of build type and product flavor. Note that by default, [a new Android project is configured with `debug` and `release` build types][8] and no product flavors.

For each relevant build variant, create a new `braze.xml` for it in `src/<build variant name>/res/values/`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
<string name="com_appboy_api_key">REPLACE_WITH_YOUR_BUILD_VARIANT_API_KEY</string>
</resources>
```

When the build variant is compiled, it will use the new API key.

To set an API key in code, please see the [runtime configuration][69] documentation.

[3]: https://developer.android.com/studio/build/build-variants.html
[8]: http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Build-Types
[50]: https://developer.android.com/studio/build/shrink-code
[54]: https://developer.android.com/reference/android/util/Log.html
[69]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/runtime_configuration/
[70]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/support/AppboyLogger.html#setLogLevel-int-

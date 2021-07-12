---
nav_title: Initial SDK Setup
platform: React Native
subplatform: 
- Android
- FireOS
page_order: 0

page_type: reference
description: "This article covers initial SDK setup steps for Android or FireOS apps using React Native."

---

# Initial SDK Setup

### Using react-native link

1. `npm install react-native-appboy-sdk@latest --save`
2. `react-native link`
3. Add the Braze repository to your project:

```
// top-level build.gradle
allprojects {
  repositories {
    maven { url "https://appboy.github.io/appboy-android-sdk/sdk" }
  }
}
```

### Without react-native link

1. `npm install react-native-appboy-sdk@latest --save`
2. Link the project by adding the following:

```
// settings.gradle

include ':react-native-appboy-sdk'
project(':react-native-appboy-sdk').projectDir = new File(settingsDir, '../node_modules/react-native-appboy-sdk/android')
```

```
// top-level build.gradle

allprojects {
  repositories {
    maven { url "https://appboy.github.io/appboy-android-sdk/sdk" }
  }
}
```

```
// app build.gradle

dependencies {
    compile project(':react-native-appboy-sdk')
}
```

### Java changes

Add our `AppboyReactPackage` to the `getPackages()` method of your Application class.

```java
    import com.appboy.reactbridge.AppboyReactPackage;

    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new AppboyReactPackage()
      );
    }
```

If your main activity uses the `singleTask` `launchMode`, add the following code to your main activity to ensure that deep links from push are passed to `Linking.getInitialURL()`.

```java
public void onNewIntent(Intent intent) {
  super.onNewIntent(intent);
  setIntent(intent);
}
```

### Completing the integration

1.  Follow the directions at [our public documentation]({{site.baseurl}}/developer_guide/platform_integration_guides/android/initial_sdk_setup/android_sdk_integration/) to finish your integration. In particular, you will need to set your Braze API key in a new `braze.xml` file and set up session handling by passing a `AppboyLifecycleCallbackListener` instance into `application.registerActivityLifecycleCallbacks`.
2.  When you need to make Braze calls from JavaScript, use the following declaration to import the JavaScript module:

```
const ReactAppboy = require('react-native-appboy-sdk');
```

---
nav_title: Initial SDK Setup
platform: React Native
subplatform: Android and FireOS
page_order: 0
---
# Initial SDK Setup

#### Using react-native link

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

#### Without react-native link

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

### Completing the integration

1.  Follow the directions at [our public documentation]({{ site.baseurl }}/developer_guide/platform_integration_guides/android/initial_sdk_setup/) to finish your integration. In particular, you will need to set your Braze API key and custom endpoint in a new `appboy.xml` file.
2.  When you need to make Braze calls from javascript, use the following declaration to import the javascript module:

```
const ReactAppboy = require('react-native-appboy-sdk');
```

# Adobe Analytics - Media Analytics for Audio and Video

{% hint style="warning" %}
This extension requires the [Adobe Analytics for Media](https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=en) add-on SKU. To learn more, contact your Adobe Customer Success Manager.
{% endhint %}

## Configure Media Analytics extension in Experience Platform Launch

1. In Experience Platform Launch, click the **Extensions** tab.
2. On the **Catalog** tab, locate the **Adobe Media Analytics for Audio and Video** extension, and click **Install**.
3. Type the extension settings.   For more information, see [Configure Media Analytics Extension](./#configure-media-analytics-extension).
4. Click **Save**.
5. Follow the publishing process to update your SDK configuration.

## Configure the Media Analytics extension

{% hint style="info" %}
If you update the Adobe Media Analytics for Audio and Video launch extension to v2.x in your launch property, you must make sure to update and use AEP SDK Media extension v2.0.0 and higher.
{% endhint %}

![Adobe Media Analytics Extension Configuration](../../.gitbook/assets/ext-ma-configuration.png)

To configure the Media Analytics extension, complete the following steps:

### **Collection API Server**

Type the name of the media collection API server. This is the server where the downloaded media tracking data is sent. Important: You need to contact your Adobe account representative for this information.

### **Channel**

Type the channel name property.

### **Player name**

Type the name of the media player in use \(for example, _AVPlayer_, _Native Player_, or _Custom Player_\).

### **Application Version**

Type the version of the media player application/SDK.

{% hint style="info" %}
Legacy settings should not be configured for Media Extension v2.x and higher. Those settings are only for backwards compatibility.
{% endhint %}

If you are using Media Extension v1.x, then go to Legacy settings section 1. Enable the `Use Tracking Server` checkbox. 2. In **Tracking Server**, Type the name of the tracking server to which all media tracking data should be sent.

## Add Media Analytics to your app

{% hint style="info" %}
This extension requires the [Adobe Analytics extension](../adobe-analytics/). You must add the Analytics extension to your Launch property and make sure the extension is correctly configured.
{% endhint %}

{% tabs %}
{% tab title="Android" %}
Latest Android SDK versions - [![Maven Central](https://img.shields.io/maven-central/v/com.adobe.marketing.mobile/core.svg?logo=android&logoColor=white&label=core&style=flat-square)](https://mvnrepository.com/artifact/com.adobe.marketing.mobile/core) [![Maven Central](https://img.shields.io/maven-central/v/com.adobe.marketing.mobile/analytics.svg?logo=android&logoColor=white&label=analytics&style=flat-square)](https://mvnrepository.com/artifact/com.adobe.marketing.mobile/analytics) [![Maven Central](https://img.shields.io/maven-central/v/com.adobe.marketing.mobile/media.svg?logo=android&logoColor=white&label=media&style=flat-square)](https://mvnrepository.com/artifact/com.adobe.marketing.mobile/media)

1. Add the Media extension and its dependencies to your project using the app's Gradle file.

   ```text
   implementation 'com.adobe.marketing.mobile:sdk-core:1.+'
   implementation 'com.adobe.marketing.mobile:analytics:1.+'
   implementation 'com.adobe.marketing.mobile:media:2.+'
   ```

   You can also manually include the libraries. Get `.aar` libraries from [Github](https://github.com/Adobe-Marketing-Cloud/acp-sdks/tree/master/android).

2. Import the Media extension in your application's main activity.

   ```java
   import com.adobe.marketing.mobile.*;
   ```
{% endtab %}

{% tab title="iOS" %}
Latest iOS SDK versions - [![Cocoapods](https://img.shields.io/cocoapods/v/ACPCore.svg?color=orange&label=ACPCore&logo=apple&logoColor=white&style=flat-square)](https://cocoapods.org/pods/ACPCore) [![Cocoapods](https://img.shields.io/cocoapods/v/ACPAnalytics.svg?color=orange&label=ACPAnalytics&logo=apple&logoColor=white&style=flat-square)](https://cocoapods.org/pods/ACPAnalytics) [![Cocoapods](https://img.shields.io/cocoapods/v/ACPMedia.svg?color=orange&label=ACPMedia&logo=apple&logoColor=white&style=flat-square)](https://cocoapods.org/pods/ACPMedia)

1. To add the Media library and its dependencies to your project, add the following pods to your `Podfile`:

   ```text
   pod 'ACPCore', '~> 2.0'
   pod 'ACPAnalytics', '~> 2.0'
   pod 'ACPMedia', '~> 2.0'
   ```

You can also manually include the libraries. Get `.a` libraries from [Github](https://github.com/Adobe-Marketing-Cloud/acp-sdks/tree/master/iOS).

1. In Xcode project, import Media extension:

   **Objective C**

   ```objectivec
    #import "ACPMedia.h"
   ```

   **Swift**

   ```swift
   import ACPMedia
   ```
{% endtab %}

{% tab title="React Native" %}
#### JavaScript

Latest React Native Wrapper versions - [![npm version](https://img.shields.io/npm/v/@adobe/react-native-acpcore.svg?color=green&label=%40adobe%2Freact-native-acpcore&logo=npm&style=flat-square)](https://badge.fury.io/js/%40adobe%2Freact-native-acpcore) [![npm version](https://img.shields.io/npm/v/@adobe/react-native-acpanalytics.svg?color=green&label=%40adobe%2Freact-native-acpanalytics&logo=npm&style=flat-square)](https://badge.fury.io/js/%40adobe%2Freact-native-acpanalytics) [![npm version](https://img.shields.io/npm/v/@adobe/react-native-acpmedia.svg?color=green&label=%40adobe%2Freact-native-acpmedia&logo=npm&style=flat-square)](https://www.npmjs.com/package/@adobe/react-native-acpmedia)

1. Install Media.

```bash
   npm install @adobe/react-native-acpmedia
```

1.1 Link

* **React Native 0.60+**

[CLI autolink feature](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) links the module while building the app.

* **React Native &lt;= 0.59**

```bash
   react-native link @adobe/react-native-acpmedia
```

_Note_ For `iOS` using `cocoapods`, run:

```bash
   cd ios/ && pod install
```

1. Import the extension.

   ```jsx
    import {ACPMedia} from '@adobe/react-native-acpmedia';
   ```

2. Get the extension version.

   ```jsx
    ACPMedia.extensionVersion().then(version => console.log("AdobeExperienceSDK: ACPMedia version: " + version));
   ```
{% endtab %}
{% endtabs %}

## Register Media with Mobile Core

{% tabs %}
{% tab title="Android" %}
### Java

To register media with Mobile Core, call the `setApplication()` method in `onCreate()` and call set up methods, as shown in this sample:

```java
import com.adobe.marketing.mobile.*;

public class MobileApp extends Application {

  @Override
  public void onCreate() {
      super.onCreate();
      MobileCore.setApplication(this);

      try {
          Media.registerExtension();
          Analytics.registerExtension();
          Identity.registerExtension();
          MobileCore.start(new AdobeCallback () {
              @Override
              public void call(Object o) {
                  MobileCore.configureWithAppID("your-launch-app-id");
              }
          });
      } catch (InvalidInitException e) {

      }
  }
}
```
{% endtab %}

{% tab title="iOS" %}
In your app's `application:didFinishLaunchingWithOptions`, register Media with Mobile Core:

### Objective-C

```objectivec
#import "ACPCore.h"
#import "ACPAnalytics.h"
#import "ACPIdentity.h"
#import "ACPMedia.h"

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [ACPCore setLogLevel:ACPMobileLogLevelDebug];
    [ACPCore configureWithAppId:@"your-launch-app-id"];

    [ACPAnalytics registerExtension];
    [ACPIdentity registerExtension];
    [ACPMedia registerExtension];

    [ACPCore start:^{

    }];

    return YES;
  }
```

### Swift

```swift
import ACPCore
import ACPAnalytics
import ACPIdentity
import ACPMedia

func application(_ application: UIApplication,
                 didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    ACPCore.setLogLevel(.debug)
    ACPCore.configure(withAppId: "your-launch-app-id")

    ACPAnalytics.registerExtension()
    ACPIdentity.registerExtension()
    ACPMedia.registerExtension()

    ACPCore.start {

    }

    return true;
}
```
{% endtab %}

{% tab title="React Native" %}
#### JavaScript

When using React Native, registering Media with Mobile Core should be done in native code which is shown under the Android and iOS tabs.
{% endtab %}
{% endtabs %}

## Configuration keys

To update your SDK configuration programmatically, use the following information to change your Media configuration values. For more information, see [Configuration API reference](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/configuration/configuration-api-reference).

| Key | Required | Description | Data Type |
| :--- | :--- | :--- | :--- |
| `media.collectionServer` | Yes | Media Collection Server endpoint to which all the media tracking data is sent. For more information, see [Collection Server](./#collection-api-server). | String |
| `media.channel` | No | Channel name. For more information, see [Channel](./#channel). | String |
| `media.playerName` | No | Name of the media player in use, i.e., "AVPlayer", "HTML5 Player", "My Custom Player". For more information, see [Player Name](./#player-name). | String |
| `media.appVersion` | No | Version of the media player app/SDK. For more information, see [Application Version](./#application-version). | String |

## Platform Support

| Platform | Support Status |
| :--- | :--- |
| Android | Supported |
| Apple iOS​ | Supported |
| React Native \(iOS & Android\) | Supported |


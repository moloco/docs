
# Moloco Android SDK - Banner

## Installation

For a `project level build.gradle` file setting, please see [Android-SDK](README.md).

Now, open the `application level build.gradle` file (located in the app folder). Edit the code so it includes the following repositories and dependencies.

```java
apply plugin: 'com.android.application'

android {
   compileSdkVersion 28
   buildToolsVersion "28.0.0"

   defaultConfig {
       applicationId "com.moloco.molocosampleapp"
       minSdkVersion 16
       targetSdkVersion 28
       versionCode 1
       versionName "1.0"
   }
   buildTypes {
       release {
           minifyEnabled false
           proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
       }
   }
   compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
}

repositories {
   maven { url 'http://artifacts.adsmoloco.com/artifactory/libs-release-local/' }
}

dependencies {
   compile fileTree(dir: 'libs', include: ['*.jar'])
   implementation 'com.moloco.sdk:moloco-sdk-base:1.1.2@aar'
   implementation 'com.moloco.sdk:moloco-sdk-banner:1.1.2@aar'

   implementation 'com.squareup.retrofit2:retrofit:2.0.2'
   implementation 'com.squareup.retrofit2:converter-gson:2.1.0'
   implementation 'com.squareup.retrofit2:converter-scalars:2.5.0'
   implementation 'com.squareup.retrofit2:adapter-rxjava2:2.4.0'
   implementation 'com.google.android.gms:play-services-ads:15.0.1'
   implementation 'io.reactivex.rxjava2:rxandroid:2.0.2'
}
```

Now, you are ready to integrate the Moloco Android SDK into your app.


## Integration
### Import
You should import classes as below.

```java
import com.moloco.ads.MolocoView;
import com.moloco.ads.MolocoView.BannerAdListener;
import com.moloco.common.Moloco;
import com.moloco.common.MolocoErrorCode;
import com.moloco.common.SdkConfiguration;
import com.moloco.network.DisposableManager;

import static com.moloco.common.logging.MLog.LogLevel;
import static com.moloco.common.logging.MLog.LogLevel.DEBUG;
import static com.moloco.common.logging.MLog.LogLevel.INFO;
```

### Activity class setting.
In an Activity (inherited) class, you should implement a BannerAdListener interface as below.

```java
public interface BannerAdListener {
    void onBannerLoaded(MolocoView banner);
    void onBannerFailed(MolocoView banner, MolocoErrorCode errorCode);
    void onBannerClicked(MolocoView banner);
    void onBannerExpanded(MolocoView banner);
    void onBannerCollapsed(MolocoView banner);
}
```

You should add implementations in the onCreate method.

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    /*
        Your logic comes here
    */
}
```

### Initialize MolocoView

```java
MolocoView mMolocoView = findViewById( /* Id of your view. e.g., R.id.banner_molocoview */ );
```

### SdkConfiguration
Initialize SdkConfiguration with a LogLevel (INFO or DEBUG).

```java
LogLevel logLevel = INFO; // or DEBUG
final SdkConfiguration sdkConfiguration = new SdkConfiguration(logLevel);
Moloco.initializeSdk(this, sdkConfiguration);
```

###  Load an Ad with an AdUnit ID.
Initialization requires `AdUnit ID` provided by Moloco. Please contact [Moloco](mailto:support@molocoads.com) if you need `AdUnit ID`.

```java
String adUnitId = "your_ad_unit_id";
mMolocoView.setAdUnitId(adUnitId);
mMolocoView.loadAd();
mMolocoView.setBannerAdListener(this);

// Add some custom keywords on an Ad request parameter.
mMolocoView.addKeyword("sample1");
mMolocoView.addKeyword("sample2");
```

Now you are ready to use **Moloco Android SDK** for Android devices!

If there is any question regarding Moloco Android SDK integration, please contact [Moloco](mailto:support@molocoads.com).
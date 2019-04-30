
# Moloco Android SDK
Moloco Android SDK provides ad serving for mobile applications to facilitate advertising for app publishers.


## Prerequisites
Moloco Android SDK 1.0.0 is compatible with all Android devices running Android **API 16** and above.

## Installation

This section contains instructions for installing Android SDK using Android Studio. Moloco Android SDK is built with Android Studio 3.4 Build #AI-183.5429.30.34.5452501. If you’re using a different version and have a problem using Android SDK, please contact [Moloco](mailto:support@molocoads.com).

First, launch your Android Studio, and click `Start a new Android project`. We will name it as `MolocoSampleApp`. On the `Target Android Devices` screen you can click **Next** default. On the next screen, let’s choose `Empty Activity` for this tutorial.

![alt text](https://storage.googleapis.com/moloco-sdk/android/2.png)

![alt text](https://storage.googleapis.com/moloco-sdk/android/1.png)
  
Click Next, then click the **Finish** button when it is activated. When your project is generated, the project navigator defaults to ‘Android’ format.
  
Expand the project tree by clicking the triangle next to MolocoSampleApp (or whatever you named your project). In the directory structure are two important gradle files: 
- The first is the build.gradle file located in the project’s root folder. We’ll refer to this build.gradle file as `project level build.gradle` file. 
- The second is the build.gradle file that appears when you expand the app folder. We’ll refer to this file as the `application level build.gradle` file.

![alt text](https://storage.googleapis.com/moloco-sdk/android/3.png)

Open the `project level build.gradle` file and confirm that the code reads as follows:

```properties
// Top level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
   repositories {
       jcenter()
   }
   dependencies {
       classpath 'com.android.tools.build:gradle:3.1.3'
       // NOTE: Do not place your application dependencies here; they belong
       // in the individual module build.gradle files
   }
}

allprojects {
   repositories {
       jcenter()
   }
}
```
  
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
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

repositories {
   maven { url 'http://artifacts.adsmoloco.com/artifactory/libs-release-local/' }
}

dependencies {
   compile fileTree(dir: 'libs', include: ['*.jar'])
   implementation 'com.moloco.sdk:moloco-sdk-base:1.0.0@aar'
   implementation 'com.moloco.sdk:moloco-sdk-banner:1.0.0@aar'

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
```
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

```
public interface BannerAdListener {
    void onBannerLoaded(MolocoView banner);
    void onBannerFailed(MolocoView banner, MolocoErrorCode errorCode);
    void onBannerClicked(MolocoView banner);
    void onBannerExpanded(MolocoView banner);
    void onBannerCollapsed(MolocoView banner);
}
```

You should add implementations in the onCreate method.
```
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    /*
        Your logic comes here
    */
}
```

### Initialize MolocoView

```
MolocoView mMolocoView = findViewById( /* Id of your view. e.g., R.id.banner_molocoview */ );
mMolocoView.setWidthPixel(720);
mMolocoView.setHeightPixel(160);
```

### SdkConfiguration
Initialize SdkConfiguration with a LogLevel (INFO or DEBUG).

```
LogLevel logLevel = INFO; // or DEBUG
final SdkConfiguration sdkConfiguration = new SdkConfiguration(logLevel);
Moloco.initializeSdk(this, sdkConfiguration);
```

###  Load an Ad with an ad unit id.
Initialization requires `AdUnitID` provided by Moloco. Please contact [Moloco](mailto:support@molocoads.com) if you need `AdUnit ID`.

```
String adUnitId = "your_ad_unit_id";
mMolocoView.setAdUnitId(adUnitId);
mMolocoView.loadAd();
mMolocoView.setBannerAdListener(this);
```

Now you are to ready use **Moloco Android SDK** for Android devices!

If there is any question regarding Moloco Android SDK integration, please contact [Moloco](mailto:support@molocoads.com).

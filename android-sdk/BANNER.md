
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
   implementation 'com.moloco.sdk:moloco-sdk-base:1.1.7@aar'
   implementation 'com.moloco.sdk:moloco-sdk-banner:1.1.7@aar'

   implementation 'com.squareup.retrofit2:retrofit:2.0.2'
   implementation 'com.squareup.retrofit2:converter-gson:2.1.0'
   implementation 'com.squareup.retrofit2:converter-scalars:2.5.0'
   implementation 'com.squareup.retrofit2:adapter-rxjava2:2.4.0'
   implementation 'com.google.android.gms:play-services-ads:15.0.1'
   implementation 'io.reactivex.rxjava2:rxandroid:2.0.2'

   implementation 'com.android.support:appcompat-v7:28.0.0'
   implementation 'com.android.support.constraint:constraint-layout:1.0.2'
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
        Your logic comes here.
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

### Handle URL by your own rules. (Optional)
We provide a custom URL handler.
In an Activity (inherited) class, you may implement a UrlHandler interface as below.

```java
public interface UrlHandler {
    /*
     * Performs the custom url handling.
     *
     * Returning true causes the current WebView to abort loading the URL since it has been handled already in here.
     * Returning false causes the WebView to continue loading the URL as usual.
     *
     * @return true if the given url was successfully handled; false otherwise.
     *
     */
    public boolean handleResolvedUrl(@NonNull final Context context,
                                     @NonNull final String url,
                                     @Nullable Iterable<String> trackingUrls,
                                     @Nullable String landingUrl);
}
```

You should add implementations in the handleResolvedUrl method.

```java
protected boolean handleResolvedUrl(final Context context,
                                 final String url,
                                 Iterable<String> trackingUrls,
                                 String landingUrl) {
    /*
        Your logic comes here.
    */
}
```

### Load an Ad with an AdUnit ID.
Initialization requires `AdUnit ID` provided by Moloco. Please contact [Moloco](mailto:support@molocoads.com) if you need `AdUnit ID`.

```java
String adUnitId = "your_ad_unit_id";
mMolocoView.setAdUnitId(adUnitId);
mMolocoView.setBannerAdListener(this);

// Set UrlHandler if it is implemented and you want to apply it to the view.
mMolocoView.setUrlHandler(this);

// Add some custom keywords to an ad request parameter.
// It is only available adding an only one keyword for each method call.
mMolocoView.addKeyword("sample1");
mMolocoView.addKeyword("sample2");

mMolocoView.loadAd();
```

Now you are ready to use **Moloco Android SDK** for Android devices!

If there is any question regarding Moloco Android SDK integration, please contact [Moloco](mailto:support@molocoads.com).


# Moloco Android SDK - Native

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
   implementation 'com.moloco.sdk:moloco-sdk-base:1.1.9@aar'
   implementation 'com.moloco.sdk:moloco-sdk-native:1.1.9@aar'

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
import com.moloco.nativeads.MolocoNative;
import com.moloco.nativeads.MolocoNative.NativeAdListener;
import com.moloco.common.Moloco;
import com.moloco.common.MolocoErrorCode;
import com.moloco.common.SdkConfiguration;
import com.moloco.network.DisposableManager;
import com.moloco.network.MolocoNativeAdResponse;

import static com.moloco.common.logging.MLog.LogLevel;
import static com.moloco.common.logging.MLog.LogLevel.DEBUG;
import static com.moloco.common.logging.MLog.LogLevel.INFO;
```

### Activity class setting.
In an Activity (inherited) class, you should implement a NativeAdListener interface as below.

```java
public interface NativeAdListener {
    void onNativeLoaded(final MolocoNativeAdResponse nativeAdResponse);
    void onNativeFailed(final MolocoErrorCode errorCode);
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

protected void onDestroy() {
    DisposableManager.dispose();
    super.onDestroy();
    /*
        Your logic comes here
    */
}
```

### Initialize MolocoNative

```java
MolocoNative mMolocoMiddleNative = new MolocoNative(this, getString(R.string.ad_unit_id_middle_native));
```

### SdkConfiguration
Initialize SdkConfiguration with a LogLevel (INFO or DEBUG).

```java
LogLevel logLevel = INFO; // or DEBUG
final SdkConfiguration sdkConfiguration = new SdkConfiguration(logLevel);
Moloco.initializeSdk(this, sdkConfiguration);
```

### Load an Ad with an AdUnit ID.
Initialization requires `AdUnit ID` provided by Moloco. Please contact [Moloco](mailto:support@molocoads.com) if you need `AdUnit ID`.

```java
// SampleNativeListener is an your implementation class of NativeAdListener interface.
SampleNativeListener molocoMiddleNativeListener = new SampleNativeListener();
mMolocoMiddleNative.setAdListener(molocoMiddleNativeListener);

// Add some custom keywords to an ad request parameter.
// It is only available adding an only one keyword for each method call.
mMolocoMiddleNative.addKeyword("sample1");
mMolocoMiddleNative.addKeyword("sample2");

mMolocoMiddleNative.loadAd();
```

### Register parameters. (Optional)
* public void setLocation([Location](https://developer.android.com/reference/android/location/package-summary) location)
 * Register a Location object.
 * Make sure to register if it is retrievable from the device.

```java
// Example
Location location = getLocationByYourOwnMethod();
mMolocoMiddleNative.setLocation(location);
```

* public void setIpAddress(String ipAddress)
 * Register an IP address.
 * Make sure to register if it is retrievable from the device.

```java
// Example
String ip_address = "127.0.0.1";
mMolocoMiddleNative.setIpAddress(ip_address);
```

* public void setCarrier(String carrier)
 * Register a device carrier.
 * Make sure to register if it is retrievable from the device.

```java
// Example
String carrier = "sample_carrier";
mMolocoMiddleNative.setCarrier(carrier);
```

### Show a Native Ad.
A Native Ad will be shown by your own implementation in NativeAdListener.

```java
private class SampleNativeListener implements NativeAdListener {
    SampleNativeListener() {
        /*
            Your logic comes here
        */
    }

    @Override
    public void onNativeLoaded(final MolocoNativeAdResponse nativeAdResponse) {
        /*
            Your logic comes here
        */
    }

    @Override
    public void onNativeFailed(final MolocoErrorCode errorCode) {
        /*
            Your logic comes here
        */
    }
}
```

### MolocoNativeAdResponse
You can retrieve variables of MolocoNativeAdResponse using methods below:

```java
// AD request ID from a client or generated by AdServer to assist with logging/tracking.
public String getRid()

// AD type. For Native Ad, always 2 is returned.
public Integer getAdtype()

// Campaign ID to assist with logging and tracking.
public String getCid()

// Creative ID to assist with logging and tracking.
public String getCrid()

// Click link.
public String getClk()

// Alternative click tracking link to report click action.
public String getClktracker()

// Call-To-Action button text.
public String getCtatext()

// Icon image resource URL.
public String getIconimage()

// List of impression tracking URLs.
public List<String> getImptracker()

// Main image resource URL.
public String getMainimage()

// Content text string.
public String getText()

// Title text string.
public String getTitle()

// Custom data as JSON string.
public String getCustomdata()
```

### Register parameters. (Default)
These paramters are constructed by SDK based on the device information.
* Carrier
* Device model
* Device type
* Country
* Device OS version
* App version

### Register parameters. (Optional)
User can define some parameters using methods below.

#### public void setLocation([Location](https://developer.android.com/reference/android/location/package-summary) location)
* Register a Location object.
* Make sure to register if it is retrievable from the device.

```java
// Example
Location location = getLocationByYourOwnMethod();
mMolocoMiddleNative.setLocation(location);
```

#### public void setIpAddress(String ipAddress)
* Register an IP address.
* Make sure to register if it is retrievable from the device.

```java
// Example
String ip_address = "127.0.0.1";
mMolocoMiddleNative.setIpAddress(ip_address);
```

#### public void setCarrier(String carrier)
* Register a device carrier.
* Make sure to register if it is retrievable from the device.

```java
// Example
String carrier = "sample_carrier";
mMolocoMiddleNative.setCarrier(carrier);
```

Now you are ready to use **Moloco Android SDK** for Android devices!

If there is any question regarding Moloco Android SDK integration, please contact [Moloco](mailto:support@molocoads.com).

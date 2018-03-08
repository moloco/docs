
# Moloco VAN SDK - Android
Moloco VAN SDK provides session and event tracking for mobile applications to facilitate advertising for app publishers.

## Prerequisites
Moloco VAN SDK 2.1.3 is compatible for devices running Android **API 14** and above. Integrating with Moloco SDK requires a `Product ID` and `Api Key` from Moloco. Contact [Moloco](mailto:support@molocoads.com) if you are missing either value.

## Installation

This section contains instructions for installing VAN SDK using Android Studio. Moloco VAN SDK is built with Android Studio 3.0.1 Build #AI-171.4443003. If you’re using a different version and have a problem using VAN SDK, please contact [Moloco](mailto:support@molocoads.com).
  
First, launch your Android Studio, and click `Start a new Android project`. We will name it as `VanSampleApp`. On the `Target Android Devices` screen you can click **Next** to choose the defaults. On the next screen, let’s choose `Empty Activity` for this tutorial.

![alt text](https://storage.googleapis.com/vansdk/android/1.png)

![alt text](https://storage.googleapis.com/vansdk/android/2.png)
  
Click Next, then click the **Finish** button when it is activated. When your project is generated, the project navigator defaults to ‘Android’ format.
  
Expand the project tree by clicking the triangle next to VanSampleApp (or whatever you named your project). In the directory structure are two important gradle files: 
- The first is the build.gradle file located in the project’s root folder. We’ll refer to this build.gradle file as `project level build.gradle` file. 
- The second is the build.gradle file that appears when you expand the app folder. We’ll refer to this file as the `application level build.gradle` file.

![alt text](https://storage.googleapis.com/vansdk/android/3.png)

Open the `project level build.gradle` file and confirm that the code reads as follows:

```gradle
// Top level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
   repositories {
       jcenter()
       google()
   }
   dependencies {
       classpath 'com.android.tools.build:gradle:3.0.1'
       // NOTE: Do not place your application dependencies here; they belong
       // in the individual module build.gradle files
   }
}

allprojects {
   repositories {
       jcenter()
       google()
   }
}
```
  
Now, open the `application level build.gradle` file (located in the app folder). Edit the code so it includes the following repositories and dependencies.

```gradle
apply plugin: "com.android.application"

android {
   compileSdkVersion 26
   buildToolsVersion "26.0.2"

   defaultConfig {
       applicationId "com.moloco.VanSampleApp"
       minSdkVersion 14
       targetSdkVersion 26
       versionCode 1
       versionName "1.0"
   }
   buildTypes {
       release {
           minifyEnabled false
           proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
       }
   }
}

repositories {
   maven { url "http://artifacts.adsmoloco.com/artifactory/libs-release-local/" }
   maven { url "https://maven.google.com" }
}

dependencies {
   compile fileTree(dir: 'libs', include: ['*.jar'])
   compile "com.google.android.gms:play-services-base:11.8.0"
   compile "com.google.android.gms:play-services-ads:11.8.0"
   compile "com.android.installreferrer:installreferrer:1.0"
   compile "com.android.volley:volley:1.0.0"
   compile "com.moloco.sdk:van-sdk:2.1.3"
}
```

Now, you are ready to integrate the Moloco VAN SDK into your app.

![alt text](https://storage.googleapis.com/vansdk/android/4.png)

### Integration
Integrating Moloco VAN SDK requires initializing `MolocoVAN`. Initialization requires a `Product ID` and an `API Key` provided by Moloco. Initialize Moloco VAN with these values by calling `MolocoVAN.init()` on the app's very first activity.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    MolocoVAN.init(this, "your_product_id", "moloco_api_key");
}
```

### SESSION
Once Moloco VAN is initialized, a `SESSION` event will automatically be sent to Moloco VAN server.

### EventType

Following list of event types are pre-defined under `EventType` enum in `Constants.java` file, to be used for `MolocoVAN.sendEvent()`.

- PURCHASE
- REGISTER
- OPEN_COMMUNITY
- INVITE
- CALL
- DELIVERY
- AUTHORIZE
- ADD_TO_CART
- ADD_TO_WISHLIST
- VIEW_CONTENT
- LEVEL_UP
- LOGIN
- RATE
- RESERVE
- SEARCH
- SELL
- SHARE
- COMPLETE_TUTORIAL

### DataMap
Users may send additional information using a `Map<String, Object>` that can be used for user tracking and targeting. DataMap is used as an argument for both `MolocoVAN.sendEvent()` and `MolocoVAN.sendCustomEvent()` for providing more information about an event.

```java
Map<String, String> dataMap = new HashMap<String, String>();
dataMap.put("gender", "male")
```

### ApiCallback
**ApiCallback** is an interface for handling http responses. Custom ApiCallback must override `onSuccessResponse(String response)` and `onFailureResponse(String response)`. Response is a string representation of error (empty string if no error)

```java
new ApiCallback() {
    @Override
    public void onSuccessResponse(String response) {
        Log.d("com.moloco.VanSampleApp", String.format("Success with response : %s", response));
    }

    @Override
    public void onFailureResponse(String response) {
        Log.d("com.moloco.VanSampleApp", String.format("Error with response : %s", response));
    }
}
```

### SendEvent
A user can send a pre-defined **EventType** to the Moloco VAN server by calling `MolocoVAN.sendEvent()`.
If the MolocoVAN class is not yet initialized then this event will be queued for later dispatch, 

```java
MolocoVAN.sendEvent(Constants.EventType.PURCHASE, dataMap, apiCallback)
```

### SendCustomEvent
A user can send a **Custom Event** to VAN server by calling `MolocoVAN.sendCustomEvent()`. You may choose any one of the CUSTOM_XX (CUSTOM_00 ~ CUSTOM_15) as a `CustomEventType`, along with `CustomEventName`, `DataMap`, and `ApiCallback`. If the MolocoVAN class is not yet initialized, the **Custom Event** is queued for later dispatch.
    
```java
MolocoVAN.sendCustomEvent(Constants.CustomEventType.CUSTOM_00, "my_custom_event", dataMap, apiCallback)
```

Now you are ready use **Moloco VAN** for Android devices! 

Once you complete integration, please work with your Moloco contact to verify receipt of the session and other events from VAN server.

If there is any question regarding with Moloco VAN Android SDK integration, please contact [Moloco](mailto:support@molocoads.com).

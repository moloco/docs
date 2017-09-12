
# Moloco VAN SDK - Android
Moloco VAN SDK provides `session` and `event tracking` to facilitate advertising for mobile app publishers.


## Prerequisites
Moloco VAN SDK 2.0.0 is compatible for devices running Android **API 9** and above. Integrating with Moloco SDK requires a `Product ID` and `Api Key` from Moloco. Contact [Moloco](mailto:support@molocoads.com if you are missing either value.

## Installation

This section contains instructions for installing VAN SDK using Android Studio. Moloco VAN SDK is built with Android Studio 2.3.3 Build #AI-162.4069837. If you’re using a different version and have a problem using VAN SDK, please contact [Moloco](mailto:support@molocoads.com).
  
First, launch your Android Studio, and click `Start a new Android project`. We will name it as `SampleVanApp`. On the `Target Android Devices` screen you can click **Next** to choose the defaults. On the next screen, let’s choose `Empty Activity` for this tutorial.

![alt text](https://storage.googleapis.com/vansdk/android/1.png)

![alt text](https://storage.googleapis.com/vansdk/android/2.png)
  
Click Next, then click the **Finish** button when it is activated. When your project is generated, the project navigator defaults to ‘Android’ format.
  
Expand the project tree by clicking the triangle next to SampleVanApp (or whatever you named your project). In the directory structure are two important gradle files: 
- The first is the build.gradle file located in the project’s root folder. We’ll refer to this build.gradle file as `project level build.gradle` file. 
- The second is the build.gradle file that appears when you expand the app folder. We’ll refer to this file as the `application level build.gradle` file.

![alt text](https://storage.googleapis.com/vansdk/android/3.png)

Open the `project level build.gradle` file and confirm that the code reads as follows:

```properties
// Tolevel build file where you can add configuration options common to all sub-projects/modules.
buildscript {
   repositories {
       jcenter()
   }
   dependencies {
       classpath 'com.android.tools.build:gradle:2.3.3'
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
   compileSdkVersion 25
   buildToolsVersion "25.0.0"

   defaultConfig {
       applicationId "com.moloco.sampleVanApp"
       minSdkVersion 9
       targetSdkVersion 25
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
   maven { url 'http://artifacts.adsmoloco.com/artifactory/libs-release-local/' }
}

dependencies {
   compile fileTree(dir: 'libs', include: ['*.jar'])
   compile 'com.android.support:appcompat-v7:25.1.1'
   compile 'com.android.support:support-v4:25.1.1'
   compile 'com.android.support:support-annotations:25.1.1'
   compile 'com.google.android.gms:play-services-ads:10.0.1'
   compile 'com.android.volley:volley:1.0.0'
   compile 'com.moloco.sdk:van-sdk:2.0.0'
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

### SendEvent
A user can send a pre-defined **Event** to the Moloco VAN server by calling `MolocoVAN.sendEvent()`.
If the MolocoVAN class is not yet initialized then this event will be queued for later dispatch, 

```java
MolocoVAN.sendEvent(Constants.EventType.PURCHASE, dataMap, apiCallback)
```

### SendCustomEvent
A user can send a **Custom Event** to VAN server by calling `MolocoVAN.sendCustomEvent()`. You may choose any one of the CUSTOM_XX (0~16) as a custom event type, along with `customEventName`. If the MolocoVAN class is not yet initialized, the **Custom Event** is queued for later dispatch.
    
```java
MolocoVAN.sendCustomEvent(Constants.CustomEventType.CUSTOM_00, "my_custom_event", dataMap, apiCallback)
```

### DataMap
Users may send additional information using a `Map<String, Object>` that can be used for user tracking and targeting.

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
        Log.d("com.moloco.sampleVanApp", String.format("Success with response : %s", response));
    }

    @Override
    public void onFailureResponse(String response) {
        Log.d("com.moloco.sampleVanApp", String.format("Error with response : %s", response));
    }
}
```

If there is any question regarding with SDK integration, please contact `support@molocoads.com`.

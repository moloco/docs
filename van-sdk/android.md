
# Moloco VAN SDK - Android
Moloco VAN SDK provides session and event tracking for mobile app advertisements.


### Prerequisites
Moloco VAN SDK 1.1.4 is compatible for devices running Android **API 9** and above. Integrating with Moloco SDK requires a `Product ID` and `Api Key` from Moloco. (Contact [Moloco](www.molocoads.com) if you do not have them)

### Installation

This section contains instructions for installing VAN SDK using Android Studio. Moloco VAN SDK is built with Android Studio 2.2.3 Build #AI-145.3537739. If you’re using a different version and have a problem using VAN SDK, please contact [Moloco](www.molocoads.com).
  
First, launch your Android studio, and click `Start a new Android project`. We will name it as `SampleVanApp`. On the `Target Android Devices` screen you can click next to choose the defaults. On the next screen, let’s choose `Blank Activity with Fragment` for this tutorial. 
  
Click Next, then click the Finish button when it is activated. When your project is generated, the project navigator defaults to ‘Android’ format.
  
Expand the project tree by clicking the triangle next to SampleVANApp (or whatever you named your project.)  In the directory structure are two important gradle files: 
- The first is the build.gradle file located in the project’s root folder. We’ll refer to this build.gradle file as `project level build.gradle` file. 
- The second is the build.gradle file that appears when you expand the app folder. We’ll refer to this file as the `application level build.gradle` file. 

Open the ‘project level build.gradle’ file (the lower one in the above image). Confirm that the code reads as follows:

```properties
// Top-level build file where you can add configuration options common to all sub-projects/modules.
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
       applicationId "com.moloco.vansample"
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
   compile 'com.moloco.sdk:van-sdk:2.0.0@aar'
}
```

### Integration
Integrating Moloco VAN SDK requires initializing `MolocoEntryPoint`. Initialization requires a product ID and an API Key provided by Moloco. Initialize MolocoEntryPoint with context by calling `MolocoEntryPoint.init()` on very first activity.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    MolocoEntryPoint.init(this, "your_product_id", "moloco_api_key");
]
```

##### Session
Once MolocoEntryPoint is initialized, `Session` event will automatically be sent to Moloco VAN server.

##### SendEvent
A user can send a pre-defined P-Event to VAN server by calling `MolocoEntryPoint.sendEvent()`
Event is stored into queue for later dispatch, if entry point is not yet initialized.

```java
MolocoEntryPoint.sendEvent(Constants.PEventType.P_PURCHASE, dataMap, apiCallback)
```

##### SendCustomEvent
A user can send a P-Custom Event to VAN server by calling `MolocoEntryPoint.sendCustomEvent()`. You may choose any one of the P_CUSTOM_XX (0~16) as a custom event type, along with `customEventName`. Event is stored into queue for later dispatch, if entry point is not yet initialized.
    
```java
MolocoEntryPoint.sendCustomEvent(Constants.PCustomEventType.P_CUSTOM_00, "my_custom_event", dataMap, apiCallback)
```

##### DataMap
User may send additional information using a `Map<String, Object>` that can be used for user tracking and targetting.

```java
Map<String, String> dataMap = new HashMap<String, String>();
dataMap.put("gender", "male")
```

##### ApiCallback
ApiCallback is an interface for handling http responses. Custom ApiCallback must override `handleResponse(E response)`. Response is a string representation of error (empty string if no error)

```java
new ApiCallback<String>() {
    @Override
    public void handleResponse(String response) {
        Log.d("com.moloco.vansample", String.format("Error : %s", response));
    }
}
```

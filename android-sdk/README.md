
# Moloco Android SDK
Moloco Android SDK provides ad serving for mobile applications to facilitate advertising for app publishers.


## Prerequisites
Moloco Android SDK 1.1.1 is compatible with all Android devices running Android **API 16** and above.

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

## SDK Modularization

With the modular SDK, you can choose to include specific formats to decrease overall SDK footprint in your app. To do so, include the line for any combination of components that you want in your build.gradle file as follows:

- [Banner](BANNER.md)
- [Native](NATIVE.md)

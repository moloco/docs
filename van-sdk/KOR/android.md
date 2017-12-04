
# Moloco VAN SDK 2.0 - Android
Moloco VAN SDK 2.0 은 모로코 서버를 통해 안드로이드앱에 유입된 설치, 실행, 인앱이벤트 및 리텐션 트래킹을 가능케합니다.

## 연동조건
Moloco VAN SDK 2.0 은 API 9 이상을 사용하는 안드로이드 기기에 적용 가능합니다. Moloco VAN SDK에 연동하기 위해서는, `Product ID` (앱 아이디) 와 `Api Key` (API 키) 를 모로코에서 부여받아야 합니다. 해당 정보가 없으시다면 [모로코](mailto:support@molocoads.com)로 연락주시기 바랍니다.

## 설치가이드
본 문서의 설치 가이드는 Android Studio를 기반으로 하였으며, Android Studio 2.3.3 버젼의 #AI-162.4069837 빌드를 사용하였습니다. 만일 다른 버젼과 빌드를 사용하여 문제가 있을 경우, [모로코](mailto:support@molocoads.com)로 연락주시기 바랍니다.
  
먼저, Android Studio 를 실행하고 `Start a new Android project`를 클릭합니다. 예시로, 프로젝트 이름을 `VanSampleApp`라고 명칭하겠습니다. `Target Android Devices` 스크린에서, **Next** 를 누를 후, `default` 설정을 클릭합니다. 다음 화면에서는 예시로 `Empty Activity`를 눌러줍니다.

![alt text](https://storage.googleapis.com/vansdk/android/1.png)

![alt text](https://storage.googleapis.com/vansdk/android/2.png)
  
다음, **Finish** 를 눌러 프로젝트를 만드신 후 좌측의 `project` 메뉴에서 `VanSampleApp` 프로젝트를 눌러줍니다. 해당 폭더 안에는, 두가지 파일이 생성 되어있을 것입니다.
- 프로젝트 단위의 `build.gradle` 파일.
- 어플리케이션 단위의 `build.gradle` 파일.

![alt text](https://storage.googleapis.com/vansdk/android/3.png)

프로젝트 단위의 `build.gradle` 파일을 여신 후, 다음과 같은 설정이 되어 있는지 확인해 주십시오.

```gradle
// Top level build file where you can add configuration options common to all sub-projects/modules.
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
  
다음으로, 어플리케이션 단위의 `build.gradle` 파일을 여신 후, 아래와 같은 `Maven` 프로젝트 dependency를 설정해 주십시오.

```gradle
apply plugin: "com.android.application"

android {
   compileSdkVersion 25
   buildToolsVersion "25.0.0"

   defaultConfig {
       applicationId "com.moloco.VanSampleApp"
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
   maven { url "http://artifacts.adsmoloco.com/artifactory/libs-release-local/" }
}

dependencies {
   compile fileTree(dir: 'libs', include: ['*.jar'])
   compile "com.android.support:appcompat-v7:25.1.1"
   compile "com.android.support:support-v4:25.1.1"
   compile "com.android.support:support-annotations:25.1.1"
   compile "com.google.android.gms:play-services-ads:10.0.1"
   compile "com.android.volley:volley:1.0.0"
   compile "com.moloco.sdk:van-sdk:2.0.0"
}
```

이로써, Moloco VAN SDK의 라이브러리를 사용하실 수 있습니다.

![alt text](https://storage.googleapis.com/vansdk/android/4.png)

### 연동가이드
앞서 말하였듯이, Moloco VAN SDK 와 연동하기 위해서는 모로코에서 앱마다 제공하는 `Product ID` 와 `API Key` 가 필요합니다. 해당 앱 아이디와 API 키를 받으신 경우, `MolocoVAN.init()` 함수를 앱 실행시 가장 처음으로 열리는 액티비티에서 불러주면 유저의 앱 설치 및 오픈 이벤트를 트래킹을 위한 세션이벤트가 기록됩니다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    MolocoVAN.init(this, "your_product_id", "moloco_api_key");
}
```

### 이벤트 종류
특정 인앱 이벤트를 트래킹하기 위해서는, 기존 이벤트를 기록하는 `sendEvent()` 함수 혹은 사용자가 직접 명시하는 이벤트를 기록하는 `sendCustomEvent()` 함수를 호출해야 합니다. `sendEvent()` 함수가 받는 기존 이벤트 종류 (`EventType` Enum) 는 다음과 같습니다.

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

### 추가 데이터
사용자는 이벤트에 기록되는 기본 정보 이외에, 사용자 별로 효율이나 성과측정을 위한 추가적인 데이터를 보낼 수 있습니다. `dataMap` 이라는 변수로 불리는 `Map<String, Object>` 자료구조를 사용하여 어떤 데이터도 전송이 가능한데, 예시로 사용자의 성별 혹은 나이 등을 추가적으로 보낼 수 있습니다.

```java
Map<String, String> dataMap = new HashMap<String, String>();
dataMap.put("gender", "male")
```

### API 콜백
**ApiCallback** 은 HTTP response 결과에 대한 추가적인 행동을 위한 장치입니다. 사용자가 직접 설정하는 ApiCallback은, `onSuccessResponse(String response)` 함수와 `onFailureResponse(String response)` 함수를 override 해야합니다. Response 는 에러메세지에 관한 문자열입니다 (에러가 없는경우 빈 문자열을 리턴합니다).

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

### SendEvent 함수
유저의 이벤트를 기록해주는 `sendEvent()` 함수는, 앞서 말한 3가지의 변수를 받습니다. `MolocoVAN.sendEvent()` 함수에는 이벤트 종류 (`EventType` enum), 추가 데이터 (`HashMap<String, Object`), 그리고 API 콜백이 변수로 들어갑니다.

```java
MolocoVAN.sendEvent(Constants.EventType.PURCHASE, dataMap, apiCallback)
```

### SendCustomEvent 함수
유저의 이벤트를 사용자 임의의 이벤트 명(custom event name) 으로 보내기를 원하시는 경우, `MolocoVAN.sendCustomEvent()`에 앞서말한 변수에 더하여 임의의 이벤트명을 추가시켜주면 됩니다. 사용자는 총 16개의 각기 다른 임의 이벤트명를 지정 할 수 있고, 그중 하나의 `CustomEventType` enum 을 통하여 이름을 지정할 수 있습니다. `sendCustomEvent()` 함수에는 기본 이벤트 종류 대신 `CustomEventType`과 `CustomEventName`가 추가적으로 들어갑니다.
    
```java
MolocoVAN.sendCustomEvent(Constants.CustomEventType.CUSTOM_00, "my_custom_event", dataMap, apiCallback)
```

이로써 모든 Android 앱 이벤트 트래킹을 위한 연동이 끝났습니다!

연동이 완료된 경우, Moloco팀에 해당 어플리케이션의 연동이 정상적으로 되었는지 확인 요청을 해주시길 바랍니다.

해당 문서에 대한 질문이나 설치/연동 과정에서의 오류가 발생하였다면, [모로코](mailto:support@molocoads.com)팀으로 연락 주시길 바랍니다.

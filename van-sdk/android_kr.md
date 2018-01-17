
# Moloco VAN SDK - Android
Moloco VAN SDK 는 모로코 서버를 통하여 안드로이드앱에 유입된 설치, 실행, 인앱이벤트 및 리텐션 트래킹 기능을 제공합니다.

## 연동조건
Moloco VAN SDK 2.1.1 은 API 9 이상의 안드로이드 기기에 적용할 수 있습니다. Moloco VAN SDK를 사용하여 App과 Moloco 서버를 연동시키려면 모로코에서 `Product ID` (앱 아이디) 와 `Api Key`를 부여받아야 합니다. 해당 정보가 필요한 경우 [모로코](mailto:support@molocoads.com)로 문의하십시오.

## 설치가이드
본 설치 가이드는 Android Studio에 기반하며, Android Studio 2.3.3 버젼의 #AI-162.4069837 빌드에서 동작을 확인하였습니다. 만일 다른 버젼과 빌드를 사용하여 문제가 있을 경우, [모로코](mailto:support@molocoads.com)로 문의하십시오.

먼저, Android Studio 를 실행하고 `Start a new Android project`를 선택하십시오. 본 문서에서는 프로젝트 이름을 `VanSampleApp`이라고 명칭하겠습니다. `Target Android Devices` 스크린에서, **Next** 를 누를 후, `default` 설정을 선택하십시오.

![alt text](https://storage.googleapis.com/vansdk/android/1.png)

다음 화면에서 `Empty Activity`를 선택하십시오.

![alt text](https://storage.googleapis.com/vansdk/android/2.png)
 
다음, **Finish** 를 눌러 프로젝트를 생성한 후, 좌측의 `project` 메뉴에서 `VanSampleApp` 프로젝트를 선택하십시오. 해당 폴더에는 다음 두 개의 파일이 생성되어 있습니다.
- 프로젝트 단위의 `build.gradle` 파일
- 어플리케이션 단위의 `build.gradle` 파일

![alt text](https://storage.googleapis.com/vansdk/android/3.png)

프로젝트 단위의 `build.gradle` 파일을 열고, 다음과 같이 설정되어 있는지 확인하십시오.

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
  
다음으로, 어플리케이션 단위의 `build.gradle` 파일을 열어후, 아래와 같이 `Maven` 프로젝트 dependency를 설정하십시오.

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
   maven { url "https://maven.google.com" }
}

dependencies {
   compile fileTree(dir: 'libs', include: ['*.jar'])
   compile "com.google.android.gms:play-services-ads:11.0.4"
   compile 'com.android.installreferrer:installreferrer:1.0'
   compile "com.android.volley:volley:1.0.0"
   compile "com.moloco.sdk:van-sdk:2.1.1"
}

```

이로써, Moloco VAN SDK의 라이브러리를 앱에서 사용할 수 있습니다.

![alt text](https://storage.googleapis.com/vansdk/android/4.png)

### 연동가이드
Moloco VAN SDK 와 연동하기 위해서는 모로코에서 각 앱마다 제공하는 `Product ID` 와 `API Key` 가 필요합니다. 해당 정보가 필요한 경우 [모로코](mailto:support@molocoads.com)로 문의하십시오. 해당 앱 아이디와 API 키를 인자로 `MolocoVAN.init()` 함수를 앱 실행시 가장 처음으로 열리는 액티비티에서 호출하면, 해당 유저의 앱 설치 및 오픈 이벤트를 트래킹을 위한 세션이벤트를 모로코 서버에 기록합니다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    MolocoVAN.init(this, "your_product_id", "moloco_api_key");
}
```

### 이벤트 종류
앱 이벤트 트래킹에는 이벤트 종류에 따라 두 가지 방법이 있습니다. 일반적으로 널리 사용되는 기본 이벤트 트래킹을 위하여 `sendEvent()` 함수를 제공합니다. 여기에 명시되지 않는 앱 고유의 이벤트들은 `sendCustomEvent()` 함수를 이용하여 트래킹할 수 있습니다. `sendEvent()` 함수가 받는 기본 이벤트 종류 (`EventType` Enum) 는 다음과 같습니다.

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
이벤트에 기록하는 기본 정보 이외에, 사용자 별 효율이나 성과 측정을 위한 추가 데이터를 첨부할 수 있습니다. `Map<String, Object>` 타입의 `dataMap` 인자를 이용하여 임의의 타입의 데이터를 전송할 수 있습니다. 예시로, 아래와 같이 사용자의 성별 정보를 추가로 보낼 수 있습니다.

```java
Map<String, String> dataMap = new HashMap<String, String>();
dataMap.put("gender", "male")
```

### API 콜백
**ApiCallback** 을 사용하여 HTTP response 결과에 대한 콜백을 등록할 수 있습니다. 사용자가 직접 설정하는 ApiCallback은, `onSuccessResponse(String response)` 함수와 `onFailureResponse(String response)` 함수를 override 해야합니다. Response 는 에러메세지에 관한 문자열입니다. (에러가 없는 경우 빈 문자열을 반환합니다).

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
유저의 이벤트를 기록해주는 `MolocoVAN.sendEvent()` 함수는 앞서 설명한 3가지 인자, 즉 이벤트 종류 (`EventType` enum), 추가 데이터 (`HashMap<String, Object`), 그리고 API 콜백을 인자로 받습니다.

```java
MolocoVAN.sendEvent(Constants.EventType.PURCHASE, dataMap, apiCallback)
```

### SendCustomEvent 함수
앱 고유의 이벤트 이름으로 (custom event name) 유저의 이벤트를 보내고자 하는 경우, `MolocoVAN.sendCustomEvent()`에 해당 이벤트명을 추가하여 호출할 수 있습니다. 사용자는 최대 16개의 고유 이벤트를 각 `CustomEventType` enum 타입에 지정할 수 있습니다. `sendCustomEvent()` 함수는 기본 이벤트 종류 대신 `CustomEventType`과 `CustomEventName`를 추가로 받습니다.
    
```java
MolocoVAN.sendCustomEvent(Constants.CustomEventType.CUSTOM_00, "my_custom_event", dataMap, apiCallback)
```

이로써 모든 Android앱 이벤트 트래킹을 위한 구현이 끝났습니다!

연동에 필요한 구현이 완료된 경우, Moloco팀에 해당 어플리케이션의 연동이 정상적으로 동작하고 있는지 확인을 요청하십시오.

해당 문서에 대한 질문 및 설치/연동 과정에서의 오류에 대한 문의는 [모로코](mailto:support@molocoads.com)팀으로 연락하십시오.

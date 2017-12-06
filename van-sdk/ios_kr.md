# Moloco VAN SDK - iOS
Moloco VAN SDK 2.0 은 모로코 서버를 통하여 IOS앱에 유입된 설치, 실행, 인앱이벤트 및 리텐션 트래킹 기능을 제공합니다.

## 연동조건
Moloco VAN SDK 2.0 는 iOS 버젼 **8.0** (target version) 이상 기기에 적용 가능합니다. 본 SDK는 `Object-C` 와 `Swift` 두 언어를 지원합니다.

Moloco VAN SDK를 사용하여 App과 Moloco 서버를 연동시키려면 모로코에서 `Product ID` (앱 아이디) 와 `Api Key`를 부여받아야 합니다. 해당 정보가 필요한 경우 [모로코](mailto:support@molocoads.com)로 문의하십시오.

Moloco VAN SDK 2.0 은 아래의 주소에서 받을 수 있습니다.
https://github.com/moloco/VANIosLibrary/archive/2.0.zip (191KB)


## 설치가이드
본 설치 가이드는 Xcode를 기반하며, Xcode `8.3.3 (8E3004b)` 버젼에서 동작을 확인하였습니다. 만일 다른 버젼과 빌드를 사용하여 문제가 있을 경우, [모로코](mailto:support@molocoads.com)로 문의하십시오.

> 중요: 어플리케이션을 앱스토어에 등록하기 전에 반드시 IDFA 사용 허가에 대한 승인을 받아주십시오: `Attribute this app installation to a previously served advertisement`.


## 연동가이드 - Objective C
연동을 위해서는 가장 먼저 위에서 다운로드 받은 `MolocoVANSDK.framework` 파일의 압축을 풀고, 해당 폴더의 하위 파일들을 IOS 어플리케이션 프로젝트의 폴더 안으로 이동시켜야 합니다.

위 파일들을 Moloco VAN SDK 2.0 라이브러리를 연동하고자 하는 어플리케이션 프로젝트에 다음 두 방법 중 하나로 추가하십시오.

- Xcode 에서 해당 프로젝트애 대하여 오른쪽 클릭 후, Add Files to (프로젝트 이름) 을 선택한 후  MolocoVANSDK.framework 를 선택
- 드래그 앤 드롭으로 MolocoVANSDK.framework 폴더를 Xcode 프로젝트에 추가

![alt text](https://storage.googleapis.com/vansdk/ios/objc/1.png)
![alt text](https://storage.googleapis.com/vansdk/ios/objc/2.png)
![alt text](https://storage.googleapis.com/vansdk/ios/objc/3.png)

좌측 Project Navigator 에서 프로젝트 이름을 클릭 후, `General` 탭의 `target` 섹션을 선택하여 `Build Phases` 로 들어가십시오. `Link Binary With Libraries` 드롭다운 메뉴를 선택하여, 다음 프레임워크 및 라이브러리를 프로젝트에 추가하십시오.

- `MolocoVANSDK.framework` (imported manually)
- `AdSupport.framework` (in main list)
- `CoreTelephony.framework` (in main list)
- `SystemConfiguration.framework` (in main list)

![alt text](https://storage.googleapis.com/vansdk/ios/objc/4.png)

다음으로, 앱 실행시 가장 먼저 실행되는 `ViewController`의 헤더 파일에 (`.h 파일`) 다음 dependency 코드를 추가하십시오.

```objc
#import <MolocoVANSDK/MolocoVAN.h>
```

이어서 위의 헤더파일에 해당하는 `ViewController`에 `MolocoApiCallback`이 명시하는 인터페이스 (interface) 를 다음과 같이 추가해야 합니다.

```objc
@interface ViewController : UIViewController<MolocoApiCallback> 
```

![alt text](https://storage.googleapis.com/vansdk/ios/objc/5.png)

위의 `MolocoApiCallback` 인터페이스는 다음과 같은 두 `delegate` 메소드를 필요로 합니다.

```objc
- (void)onSuccessResponse:(NSString *)response;
- (void)onFailureResponse:(NSString *)response;
```

예시로, `onSuccessResponse` 와 `onFailureResponse` 메소드를 다음과 같이 설정할 수 있습니다.

```objc
- (void)onSuccessResponse:(NSString *)response {
  NSLog(@"Successfully sent event with response : %@", response);
  [textView setText:[NSString stringWithFormat:@"Success response: %@", response]];
}

- (void)onFailureResponse:(NSString *)response {
  NSLog(@"Failed to send event with response : %@", response);
  [textView setText:[NSString stringWithFormat:@"Fail response: %@", response]];
}
```

마지막으로, Moloco VAN SDK를 연동하기 위해서는 `MolocoVAN` 객체를 생성해야 합니다. 객체를 생성하기 위해서는 모로코에서 제공하는 `Product ID` (앱 아이디) 와 `API Key` (API 키) 가 필요합니다. 해당 정보를 부여받은 뒤, 앱에서 가장 먼저 실행되는 액티비티에서 `[MolocoVAN setProductId(ProductId, Api-Key)]` 함수를 호출하십시오.

```objc
- (void)viewDidLoad {
  [super viewDidLoad];
  // Do any additional setup after loading the view, typically from a nib.
  [MolocoVAN setProductId:@"your_product_id" apiKey:@"moloco_api_key"];
}
```

![alt text](https://storage.googleapis.com/vansdk/ios/objc/6.png)


## 연동가이드 - Swift
연동을 위해서는 가장 먼저 위에서 다운로드 받은 `MolocoVANSDK.framework` 파일의 압축을 풀고, 해당 폴더의 하위 파일들을 IOS 어플리케이션 프로젝트의 폴더 안으로 이동시켜야 합니다.

위 파일들을 Moloco VAN SDK 2.0 라이브러리를 연동하고자 하는 어플리케이션 프로젝트에 다음 두 방법 중 하나로 추가하십시오.

- Xcode 에서 해당 프로젝트애 대하여 오른쪽 클릭 후, Add Files to (프로젝트 이름) 을 선택한 후  MolocoVANSDK.framework 를 선택
- 드래그 앤 드롭으로 MolocoVANSDK.framework 폴더를 Xcode 프로젝트에 추가

![alt text](https://storage.googleapis.com/vansdk/ios/swift/1.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/2.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/3.png)

좌측 Project Navigator 에서 프로젝트 이름을 클릭 후, 사용자의 프로젝트의 `General` 탭의 `target` 섹션을 눌러 `Build Phases` 로 들어갑니다. `Link Binary With Libraries` 라는 드롭다운 메뉴를 클릭하여, 아래의 리스트 중 추가되지 않은 프레임워크와 라이브러리를 추가합니다.

- `MolocoVANSDK.framework` (imported manually)
- `AdSupport.framework` (in main list)
- `CoreTelephony.framework` (in main list)
- `SystemConfiguration.framework` (in main list)

![alt text](https://storage.googleapis.com/vansdk/ios/swift/4.png)

Moloco VAN SDK 프레임워크는 Objective-C 언어로 작성되었기 때문에, `bridge header` 파일을 프로젝트에 추가시켜 주어야 합니다.

`bridge header` 파일을 생성하기 위해서는, 어플리케이션 폴더 하위에 `(사용자 프로젝트이름)-Bridging-Header.h` 이라는 이름의 파일을 만들고, 파일 내용을 다음과 같은 예제로 작성합니다.


```objc
//
//  VanSampleAppSwift-Bridging-Header.h
//  VanSampleAppSwift
//
//  Copyright © 2017 Molocoads. All rights reserved.
//

#ifndef VanSampleAppSwift_Bridging_Header_h
#define VanSampleAppSwift_Bridging_Header_h


#endif /* VanSampleAppSwift_Bridging_Header_h */

#import "MolocoVAN.h"
#import "Constants.h"

```

![alt text](https://storage.googleapis.com/vansdk/ios/swift/5.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/6.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/7.png)

위에서 작성한 헤더파일을 링크시켜주기 위해서는, 다음과 같은 절차를 밟아야 합니다.

1. framework 검색 경로 수정
- 프로젝트에서 `Build Settings` 탭을 선택 후, `Header Search Paths` 라는 필드를 찾아 `$(PROJECT_DIR)/MolocoVANSDK.framework/Headers` 경로를 추가합니다.

![alt text](https://storage.googleapis.com/vansdk/ios/swift/8.png)

2. bridging header 검색 경로 수정
- 프로젝트에서 `Build Settings` 탭을 선택 후, `Objective-C Bridging Header` 라는 필드를 찾아 `$(PROJECT_DIR)/ProjectName-Bridging-Header.h` 경로를 추가합니다.

![alt text](https://storage.googleapis.com/vansdk/ios/swift/9.png)

다음으로, 앱 실행시 가장 먼저 실행되는 `ViewController`가 `MolocoApiCallback` 인터페이스를 적용하도록 변경해 주어야 합니다. `MolocoApiCallback` 인터페이스는 두가지 `delegate` 메소드를 필요로 하는데, 이 함수들은 HTTP 프로토콜 response 결과에 따라 실행됩니다. 예제로, 다음과 같이 설정할 수 있습니다.

```swift
class ViewController: UIViewController, MolocoApiCallback {

    ... 
    
    func onSuccessResponse(_ response: String!) {
        // Handle success response from VAN server.
        textView.text = "Success response = \(response)"        
        print("Success response = \(response)\n")
    }

    func onFailureResponse(_ response: String!) {
        // Handle failue response from VAN server.
        textView.text = "Failure response = \(response)"
        print("Failure response = \(response)\n")
    }
}
```

마지막으로, Moloco VAN SDK를 연동하기 위해서는 `MolocoVAN` 객체를 생성해야 합니다. 객체를 생성하기 위해서는 모로코에서 제공하는 `Product ID` (앱 아이디) 와 `API Key` (API 키) 가 필요합니다. 해당 정보를 부여받은 뒤, 위와 같은 `ViewController` 에서 `[MolocoVAN setProductId(ProductId, Api-Key)]` 함수를 호출하십시오.

```swift
class ViewController: UIViewController, MolocoApiCallback {
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.        
        MolocoVAN.setProductId("your_product_id", apiKey: "your_api_key")
    }
}
```

![alt text](https://storage.googleapis.com/vansdk/ios/swift/10.png)


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
이벤트에 기록하는 기본 정보 이외에, 사용자 별 효율이나 성과 측정을 위한 추가 데이터를 첨부할 수 있습니다. `NSDictionary *` 타입의 `dataMap` 인자를 이용하여 임의의 타입의 데이터를 전송할 수 있습니다. 예시로, 아래와 같이 사용자의 성별 정보를 추가적으로 보낼 수 있습니다.

```objc
// ObjC
NSMutableDictionary *dict = [[NSMutableDictionary alloc] init];
[dict setValue:@"OS" forKey:@"iOS"];
[dict setValue:[NSNumber numberWithInt:31] forKey:@"age"];
```

```swift
// Swift
var dict:[String:String] = ["Moloco":"Van"];
```

#### SendEvent
유저의 이벤트를 기록해주는 `MolocoVAN.sendEvent()` 함수는 앞서 설명한 3가지 인자, 즉 이벤트 종류 (`EventType` enum), 추가 데이터 (`NSDictionary *`, 그리고 `Delegate` 인 `MolocoApiCallback`을 인자로 받습니다.

```objc
// ObjC: Send Event
[MolocoVAN sendEvent:PURCHASE dataMap:dict delegate:self];
```

```swift
// Swift: Send Event
MolocoVAN.send(PURCHASE, dataMap: dict, delegate: self);
```

#### SendCustomEvent
앱 고유의 이벤트 이름으로 (custom event name) 유저의 이벤트를 보내고자 하는 경우, `MolocoVAN.sendCustomEvent()`에 해당 이벤트명을 추가하여 호출할 수 있습니다. 사용자는 총 16개의 고유 이벤트를 각 `CustomEventType` enum 을 통하여 지정할 수 있습니다. `sendCustomEvent()` 함수는 기본 이벤트 종류 대신 `CustomEventType`과 `CustomEventName`를 추가로 받습니다.
    
```objc
// ObjC - Send Custom Event
[MolocoVAN sendCustomEvent:CUSTOM_00 customEventName:@"my_custom_ios_event" dataMap:dict delegate:self];
```

```swift
// Swift - Send Custom Event
MolocoVAN.send(CUSTOM_00, customEventName: "my_custom_event", dataMap: dict, delegate: self);
```

이로써 모든 IOS앱 이벤트 트래킹을 위한 구현이 끝났습니다!

연동에 필요한 구현이 완료된 경우, Moloco팀에 해당 어플리케이션의 연동이 정상적으로 동작하고 있는지 확인을 요청하십시오.

해당 문서에 대한 질문 및 설치/연동 과정에서의 오류에 대한 문의는 [모로코](mailto:support@molocoads.com)팀으로 연락하십시오.

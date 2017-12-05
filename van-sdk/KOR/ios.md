# Moloco VAN SDK - iOS
Moloco VAN SDK 2.0 은 모로코 서버를 통해 iOS 앱에 유입된 설치, 실행, 인앱이벤트 및 리텐션 트래킹을 가능케합니다.

## 연동조건
Moloco VAN SDK 2.0 를 사용하기 위해선, 먼저 아래의 라이브러리를 다운로드 해주셔야 합니다.

https://github.com/moloco/VANIosLibrary/archive/2.0.zip (191KB)

Moloco VAN SDK 2.0 는 iOS 버젼 **8.0** (target version) 이상 기기에 적용 가능합니다. 본 SDK는 `Object-C` 와 `Swift` 두 언어를 지원합니다.

Moloco VAN SDK에 연동하기 위해서는, `Product ID` (앱 아이디) 와 `Api Key` (API 키) 를 모로코에서 부여받아야 합니다. 해당 정보가 없으시다면 [모로코](mailto:support@molocoads.com)로 연락주시기 바랍니다.

## 설치가이드
본 문서의 설치 가이드는 Xcode를 기반으로 만들었으며, Xcode 버젼 `8.3.3 (8E3004b)` 를 사용하였습니다. 만일 다른 버젼과 빌드를 사용하여 문제가 발생하였을 경우, [모로코](mailto:support@molocoads.com)로 연락주시가 바랍니다.

SDK 연동 시, 사용자가 사용하길 원하는 언어에 해당하는 연동가이드를 참고해 주시길 바랍니다.

> 중요: 어플리케이션을 앱스토어에 발표하기 전, IDFA 사용 허가에 대한 승인을 받아주십시오: `Attribute this app installation to a previously served advertisement`.


## Objective C
먼저, 위에서 다운로드 받은 `MolocoVANSDK.framework` 파일의 압출을 풀고, 하위 파일들을 연동하고자 하는 어플리케이션 프로젝트의 폴더로 복사해주십시오.

다음으로, Xcode 에서 해당 프로젝트애 대하여 오른쪽 클릭 후, `Add Files to (프로젝트 이름)` 을 눌러주시고 `MolocoVANSDK.framework` 를 프로젝트에 추가해주십시오. 다른 방법으로는, 드래그 앤 드롭으로 `MolocoVANSDK.framework` 폴더를 Xcode 프로젝트에 추가 시킬 수 있습니다.

![alt text](https://storage.googleapis.com/vansdk/ios/objc/1.png)
![alt text](https://storage.googleapis.com/vansdk/ios/objc/2.png)
![alt text](https://storage.googleapis.com/vansdk/ios/objc/3.png)

좌측 Project Navigator 에서 프로젝트 이름을 클릭 후, 사용자의 프로젝트의 `General` 탭의 `target` 섹션을 눌러 `Build Phases` 로 들어갑니다. `Link Binary With Libraries` 라는 드롭다운 메뉴를 클릭하여, 아래의 리스트 중 추가되지 않은 프레임워크와 라이브러리를 추가해 주십시오.

- `MolocoVANSDK.framework` (imported manually)
- `AdSupport.framework` (in main list)
- `CoreTelephony.framework` (in main list)
- `SystemConfiguration.framework` (in main list)

![alt text](https://storage.googleapis.com/vansdk/ios/objc/4.png)

다음으로, Moloco VAN SDK API 를 사용하고자 하는 파일의 헤더 파일에 (`.h 파일`) 다음과 같은 dependency 코드를 삽입해 주십시오:

```objc
#import <MolocoVANSDK/MolocoVAN.h>
```

추가로, `MolocoApiCallback` 에서 명시하는 프로토콜 (interface) 을 사용해야 하는데, 예제로 다음과 같이 사용하십시오: 
```objc
@interface ViewController : UIViewController<MolocoApiCallback> 
```

![alt text](https://storage.googleapis.com/vansdk/ios/objc/5.png)

Moloco VAN SDK를 연동하기 위해서는 `MolocoVAN` 객체를 initialize 해야합니다. 객체를 만들기 위해서는 모로코에서 제공하는 `Product ID` (앱 아이디) 와 `API Key` (API 키) 가 필요합니다. 해당 정보를 부여받은 뒤, 앱에서 가장 먼저 실행되는 액티비티에서 `[MolocoVAN setProductId(ProductId, Api-Key)]` 함수를 호출해 주십시오.

```objc
- (void)viewDidLoad {
  [super viewDidLoad];
  // Do any additional setup after loading the view, typically from a nib.
  [MolocoVAN setProductId:@"your_product_id" apiKey:@"moloco_api_key"];
}
```

![alt text](https://storage.googleapis.com/vansdk/ios/objc/6.png)

객체화 후에는, `delegate` 메소드를 `MolocoApiCallback` 인터페이스에 따라 작성해 주시길 바랍니다.

```objc
- (void)onSuccessResponse:(NSString *)response;
- (void)onFailureResponse:(NSString *)response;
```

예시로, `onSuccessResponse` 와 `onFailureResponse` 메소드를 다음과 같이 설정해 주십시오.

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

## Swift
먼저, 위에서 다운로드 받은 `MolocoVANSDK.framework` 파일의 압출을 풀고, 하위 파일들을 연동하고자 하는 어플리케이션 프로젝트의 폴더로 복사해주십시오.

다음으로, Xcode 에서 해당 프로젝트애 대하여 오른쪽 클릭 후, `Add Files to (프로젝트 이름)` 을 눌러주시고 `MolocoVANSDK.framework` 를 프로젝트에 추가해주십시오. 다른 방법으로는, 드래그 앤 드롭으로 `MolocoVANSDK.framework` 폴더를 Xcode 프로젝트에 추가 시킬 수 있습니다.

![alt text](https://storage.googleapis.com/vansdk/ios/swift/1.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/2.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/3.png)

좌측 Project Navigator 에서 프로젝트 이름을 클릭 후, 사용자의 프로젝트의 `General` 탭의 `target` 섹션을 눌러 `Build Phases` 로 들어갑니다. `Link Binary With Libraries` 라는 드롭다운 메뉴를 클릭하여, 아래의 리스트 중 추가되지 않은 프레임워크와 라이브러리를 추가해 주십시오.

- `MolocoVANSDK.framework` (imported manually)
- `AdSupport.framework` (in main list)
- `CoreTelephony.framework` (in main list)
- `SystemConfiguration.framework` (in main list)

![alt text](https://storage.googleapis.com/vansdk/ios/swift/4.png)

Moloco VAN SDK 프레임워크는 Objective-C 언어로 작성되었기 때문에, `bridge header` 파일을 프로젝트에 추가시켜 주어야 합니다.

`bridge header` 파일을 생성하기 위해서는, 어플리케이션 폴더 하위에 `(사용자 프로젝트이름)-Bridging-Header.h` 이라는 파일을 만들고, 파일 내용을 다음과 같은 예제로 작성합니다.


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

마지막으로, `ViewController` 파일이 `MolocoVAN` 객체를 인스턴스화 시켜줌과 동시에 `MolocoApiCallback` 인터페이스를 적용하도록 변경해 주어야 하는데, 다음과 같은 방법으로 설정해주십시오.

```swift
class ViewController: UIViewController, MolocoApiCallback {
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.        
        MolocoVAN.setProductId("your_product_id", apiKey: "your_api_key")
    }

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

![alt text](https://storage.googleapis.com/vansdk/ios/swift/10.png)


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
사용자는 이벤트에 기록되는 기본 정보 이외에, 사용자 별로 효율이나 성과측정을 위한 추가적인 데이터를 보낼 수 있습니다. `dataMap` 이라는 변수로 불리는 `NSDictionary *` 자료구조를 사용하여 어떤 데이터도 전송이 가능한데, 예시로 사용자의 성별 혹은 나이 등을 추가적으로 보낼 수 있습니다.


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
유저의 이벤트를 기록해주는 `sendEvent()` 함수는, 앞서 말한 3가지의 변수를 받습니다. `MolocoVAN.sendEvent()` 함수에는 이벤트 종류 (`EventType` enum), 추가 데이터 (`NSDictionary *`, 그리고 `Delegate` 인 `MolocoApiCallback`이 변수로 들어갑니다.

```objc
// ObjC: Send Event
[MolocoVAN sendEvent:PURCHASE dataMap:dict delegate:self];
```

```swift
// Swift: Send Event
MolocoVAN.send(PURCHASE, dataMap: dict, delegate: self);
```

#### SendCustomEvent
유저의 이벤트를 사용자 임의의 이벤트 명(custom event name) 으로 보내기를 원하시는 경우, `MolocoVAN.sendCustomEvent()`에 앞서말한 변수에 더하여 임의의 이벤트명을 추가시켜주면 됩니다. 사용자는 총 16개의 각기 다른 임의 이벤트명를 지정 할 수 있고, 그중 하나의 `CustomEventType` enum 을 통하여 이름을 지정할 수 있습니다. `sendCustomEvent()` 함수에는 기본 이벤트 종류 대신 `CustomEventType`과 `CustomEventName`가 추가적으로 들어갑니다.
    
```objc
// ObjC - Send Custom Event
[MolocoVAN sendCustomEvent:CUSTOM_00 customEventName:@"my_custom_ios_event" dataMap:dict delegate:self];
```

```swift
// Swift - Send Custom Event
MolocoVAN.send(CUSTOM_00, customEventName: "my_custom_event", dataMap: dict, delegate: self);
```

이로써 모든 IOS 앱 이벤트 트래킹을 위한 연동이 끝났습니다!

연동이 완료된 경우, Moloco팀에 해당 어플리케이션의 연동이 정상적으로 되었는지 확인 요청을 해주시길 바랍니다.

해당 문서에 대한 질문이나 설치/연동 과정에서의 오류가 발생하였다면, [모로코](mailto:support@molocoads.com)팀으로 연락 주시길 바랍니다.

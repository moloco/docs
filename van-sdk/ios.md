# Moloco VAN SDK - iOS
Moloco VAN SDK provides session and event tracking for mobile applications to facilitate advertising for app publishers.

## Prerequisites
You will need to download the following library to install Moloco VAN SDK 2.1.2 for iOS:

[Download Moloco VAN SDK 2.1.2](https://github.com/moloco/VANIosLibrary/archive/master.zip)

Moloco VAN SDK 2.1.2 is compatible for devices running iOS **8.0** and above (target version). To use the Moloco VAN SDK with Object-C or Swift, you must set the deployment target version to at least 8.0.

Integrating with Moloco SDK requires a `Product ID` and `Api Key` from Moloco. Contact [Moloco](mailto:support@molocoads.com) if you do not have these credentials.

## Installation

This section contains instructions for installing Moloco VAN SDK using Xcode. Moloco VAN SDK is built and tested with Xcode Version `8.3.3 (8E3004b)`. If you’re using a different version and have a problem using VAN SDK, please contact [Moloco](mailto:support@molocoads.com).


Please follow appropriate sections below according to your choice of language (Objective-C or Swift).

> IMPORTANT: When submitting your app to the app store, make sure to select appropriate IDFA permissions: `Attribute this app installation to a previously served advertisement`.


## Objective C

First, unzip the library file and copy the `MolocoVANSDK.framework` bundle into the base directory of your project’s folder using Finder.

Then, right-click on your project in Xcode, select `Add Files to (project name)` and add `MolocoVANSDK.framework` to your project. Alternatively, you can just drag and drop `MolocoVANSDK.framework` folder into your project.

![alt text](https://storage.googleapis.com/vansdk/ios/objc/1.png)
![alt text](https://storage.googleapis.com/vansdk/ios/objc/2.png)
![alt text](https://storage.googleapis.com/vansdk/ios/objc/3.png)

Click on the project name at the top of the Project Navigator in Xcode, select your iOS project’s target on the pane to the right, then `Build Phases` across the top.

Click the drop-down titled `Link Binary With Libraries`. If not already listed, use the plus sign button to add the following frameworks and libraries.

- `MolocoVANSDK.framework` (imported manually)
- `AdSupport.framework` (in main list)
- `CoreTelephony.framework` (in main list)
- `SystemConfiguration.framework` (in main list)

![alt text](https://storage.googleapis.com/vansdk/ios/objc/4.png)

In the header file in which you want to call Moloco VAN API, add the following import:

```objc
#import <MolocoVANSDK/MolocoVAN.h>
```

Follow the `MolocoApiCallback` protocol.  For example, in the header file of your interface, the interface definition should look like this: 
```objc
@interface ViewController : UIViewController<MolocoApiCallback> 
```

![alt text](https://storage.googleapis.com/vansdk/ios/objc/5.png)

Integrating Moloco VAN SDK requires initializing `MolocoVAN`. Initialization requires a product ID and an API Key provided by Moloco. Initialize `MolocoVAN` with these values by calling `[MolocoVAN setProductId(ProductId, Api-Key)]` on the app's very first activity.

```objc
- (void)viewDidLoad {
  [super viewDidLoad];
  // Do any additional setup after loading the view, typically from a nib.
  [MolocoVAN setProductId:@"your_product_id" apiKey:@"moloco_api_key"];
}
```

![alt text](https://storage.googleapis.com/vansdk/ios/objc/6.png)

In the implementation (e.g., `ViewController.m`) file, implement the delegate method of `MolocoApiCallback` protocol. Add the following:

```objc
- (void)onSuccessResponse:(NSString *)response;
- (void)onFailureResponse:(NSString *)response;
```

For example, you may want to modify the `onSuccessResponse` and `onFailureResponse` method to look like:

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

Unzip the library file and copy the `MolocoVANSDK.framework` bundle into the base directory of your project’s folder using Finder.

Right-click on your project in Xcode, select `Add Files to (project name)` and add `MolocoVANSDK.framework` to your project.  Alternatively, you can just drag and drop `MolocoVANSDK.framework` folder into your project.

![alt text](https://storage.googleapis.com/vansdk/ios/swift/1.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/2.png)
![alt text](https://storage.googleapis.com/vansdk/ios/swift/3.png)

In the **General** tab of project settings page, scroll down to `Link Binary With Libraries` section. If not already listed, use the plus sign button to add the following frameworks and libraries.

- `MolocoVANSDK.framework` (imported manually)
- `AdSupport.framework` (in main list)
- `CoreTelephony.framework` (in main list)
- `SystemConfiguration.framework` (in main list)

![alt text](https://storage.googleapis.com/vansdk/ios/swift/4.png)

Since the Moloco VAN SDK framework is written in Objective-C, it is required to insert the bridge header file into the project. 

You can easily do this by creating a header file called `ProjectName-Bridging-Header.h` file under app level directory, where `ProjectName` is the name of your app's project name. Then, you can edit your header file to look as below.


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

In order to link these header files, you need to follow these instructions:

1. Modify framework search path
- Select `Build Settings` tab in your project, look for `Header Search Paths` fields, and add `$(PROJECT_DIR)/MolocoVANSDK.framework/Headers`.

![alt text](https://storage.googleapis.com/vansdk/ios/swift/8.png)

2. Modify bridging header search path
- Select `Build Settings` tab in your project, look for `Objective-C Bridging Header` fields, and add `$(PROJECT_DIR)/ProjectName-Bridging-Header.h`, where ProjectName is your app's project name.

![alt text](https://storage.googleapis.com/vansdk/ios/swift/9.png)

Finally, your `ViewController` needs to implement `MolocoApiCallback` interface, as well as `MolocoVAN` initialization.

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


#### Session
Once `MolocoVAN` is initialized, a `SESSION` event will automatically be sent to the Moloco VAN server.

### EventType
Following list of event types are pre-defined under `EventType` enum in `Constants.h` header file, to be used for `MolocoVAN.sendEvent()`.

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

#### DataMap
Users may send additional information using an `NSDictionary *` that can be used for user tracking and targeting. DataMap is used as an argument for both `MolocoVAN.sendEvent()` and `MolocoVAN.sendCustomEvent()` for providing more information about an event.

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
A user can send a pre-defined event to the Moloco VAN server by calling `MolocoVAN.sendEvent()` with an `EventType`, `DataMap` and `Delegate` (MolocoApiCallback).

```objc
// ObjC: Send Event
[MolocoVAN sendEvent:PURCHASE dataMap:dict delegate:self];
```

```swift
// Swift: Send Event
MolocoVAN.send(PURCHASE, dataMap: dict, delegate: self);
```

#### SendCustomEvent
A user can send a **Custom Event** to VAN server by calling `MolocoVAN.sendCustomEvent()`. You may choose any one of the CUSTOM_XX (CUSTOM_00 ~ CUSTOM_15) as a `CustomEventType`, along with `CustomEventName`, `DataMap` and `Delegate` (MolocoApiCallback).
    
```objc
// ObjC - Send Custom Event
[MolocoVAN sendCustomEvent:CUSTOM_00 customEventName:@"my_custom_ios_event" dataMap:dict delegate:self];
```

```swift
// Swift - Send Custom Event
MolocoVAN.send(CUSTOM_00, customEventName: "my_custom_event", dataMap: dict, delegate: self);
```

Now you are ready use **Moloco VAN** for iOS devices!

Once you complete integration, please work with your Moloco contact to verify receipt of the session and other events from VAN server.

If there is any question regarding with Moloco VAN iOS SDK integration, please contact [Moloco](mailto:support@molocoads.com).

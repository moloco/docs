
# Moloco VAN SDK - iOS
Moloco VAN SDK provides session and event tracking for mobile app advertisements.


### Prerequisites
You will need to download the following library to install VAN SDK for iOS:

https://github.com/moloco/VANIosLibrary/archive/master.zip (191KB)

Moloco VAN SDK 2.0.0 is compatible for devices running iOS **8.0** and above (target version). To use the Moloco VAN SDK with Object-C or Swift, you must set the deployment target version to at least 8.0.

Integrating with Moloco SDK requires a `Product ID` and `Api Key` from Moloco. (Contact [Moloco](www.molocoads.com) if you do not have them)

### Installation

This section contains instructions for installing VAN SDK using Xcode. Moloco VAN SDK is built and tested with Xcode Version 8.3.3 (8E3004b). If you’re using a different version and have a problem using VAN SDK, please contact [Moloco](www.molocoads.com).


Please follow appropriate sections below according to your choice of language (Objective-C and Swift).

IMPORTANT: When submitting your app to the app store, make sure to select appropriate IDFA permissions: “Attribute this app installation to a previously served advertisement”.


#### Objective C

First, unzip the library file and copy the MolocoVANSDK.framework bundle into the base directory of your project’s folder using Finder.

Then, right-click on your project in Xcode, select `Add Files to (project name)` and add MolocoVANSDK.framework to your project. Alternatively, you can just drag and drop MolocoVANSDK.framework folder into your project.

Click on the project name at the top of the Project Navigator in Xcode, select your iOS project’s target on the pane to the right, then `Build Phases` across the top.

Click the drop-down titled `Link Binary With Libraries`. If not already listed, use the plus sign button to add the following frameworks and libraries.

- MolocoVANSDK.framework (imported manually)
- AdSupport.framework (in main list)
- CoreTelephony.framework (in main list)
- SystemConfiguration.framework (in main list)


In the header file which you want to call Moloco VAN API, add the following import:

```objc
#import <MolocoVANSDK/MolocoEntryPoint.h>
```

Follow the MolocoApiCallback protocol.  For example, in the header file of your interface, the interface definition should look like this: 
```objc
@interface ViewController : UIViewController<MolocoApiCallback> 
```

In the implementation (e.g., `ViewController.m`) file, implement the delegate method of MolocoApiCallback protocol. Add the following:
```objc
- (void)handleVANResponse:(NSString *)response {
  // Your code to handle response.
  NSLog(@"Response: %@", response);
}

```

For example, you may want to modify the handleVANResponse method to look like:

```objc
- (void)handleVANResponse:(NSString *)response {
  // Your code to handle response.
  UserInfo *userInfo = [UserInfo sharedInstance];
  NSLog(@"Response: %@\nIDFA: %@\nModel: %@\nCarrier: %@", response, [userInfo idfa], [userInfo model], [userInfo carrier]);
}
```

#### Swift

Unzip the library file and copy the MolocoVANSDK.framework bundle into the base directory of your project’s folder using Finder.

Right-click on your project in Xcode, select `Add Files to (project name)` and add MolocoVANSDK.framework to your project.  Alternatively, you can just drag and drop MolocoVANSDK.framework folder into your project.

In the General tab of project settings page, scroll down to `Link Binary With Libraries` section. If not already listed, use the plus sign button to add the following frameworks and libraries.

- MolocoVANSDK.framework (imported manually)
- AdSupport.framework (in main list)
- CoreTelephony.framework (in main list)
- SystemConfiguration.framework (in main list)

Since MolocoVANSDK framework is written in Objective-C, it is required to insert the bridge header file into the project. You can easily do this by creating a dummy Objective-C (.m) file under app level directory, and clicking on `Create Bridge Header`. After so, delete the dummy file and add the following two lines to the bridging header file (`{project name}-Bridging-Header.h`)from the Navigator.

```objc
#import <MolocoVANSDK/Constants.h>
#import <MolocoVANSDK/MolocoEntryPoint.h>
```

Lastly, your ViewController needs to implement MolocoApiCallback interface.

```objc
class ViewController: UIViewController, MolocoApiCallback {
    ...
    func handleVANResponse(_ response: String!) {
        // Handle response from VAN server.
        textView.text = "Response = \(response)"
        
        // You can also access device and user information that VAN collected.
        let userInfo = UserInfo.sharedInstance() as AnyObject?
        print("Response = \(response)\nIDFA = \(userInfo?.idfa)\nOS = \(userInfo?.os)\nCarrier = \(userInfo?.carrier)\n")
    }
}
```

# Moloco VAN SDK - Android
Moloco VAN SDK provides session and event tracking for mobile app advertisements.

### Download
  - Please download Moloco VAN SDK from the following [GitHub Page](https://github.com/moloco/VANSample/tree/master/android/VanSampleApp)
  - Before integrating the SDK, please ask for **Product ID** and **Api Key** from Moloco.

### Prerequisites
  - Moloco VAN SDK 1.2.x is compatible for devices running Android **API 9** and above.
  - Following dependencies must be added to `build.gradle`

```sh
compile fileTree(dir: 'libs', include: ['*.jar'])
compile 'com.android.support:appcompat-v7:25.1.1'
compile 'com.android.support:support-v4:25.1.1'
compile 'com.android.support:support-annotations:25.1.1'
compile 'com.google.android.gms:play-services-ads:10.0.1'
compile 'com.android.volley:volley:1.0.0'
compile project(':van-sdk')
```

### Integration
- Integrating Moloco VAN SDK requires initializing `MolocoEntryPoint`.
- Initialization requires a product ID and an API Key provided by Moloco.
- Initialize MolocoEntryPoint with context by calling `MolocoEntryPoint.init()` on very first activity.

```sh
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    MolocoEntryPoint.init(this, "your_product_id", "moloco_api_key");
]
```

##### Session
- Once MolocoEntryPoint is initialized, `Session` event will automatically be sent to Moloco VAN server.

##### SendEvent
- A user can send a pre-defined P-Event to VAN server by calling `MolocoEntryPoint.sendEvent()`
- Event is stored into queue for later dispatch, if entry point is not yet initialized.

```sh
MolocoEntryPoint.sendEvent(Constants.PEventType.P_PURCHASE, dataMap, apiCallback)
```

##### SendCustomEvent
- A user can send a P-Custom Event to VAN server by calling `MolocoEntryPoint.sendCustomEvent()`
- May choose any one of the P_CUSTOM_XX (0~16) as a custom event type, along with `customEventName`.
- Event is stored into queue for later dispatch, if entry point is not yet initialized.
    
```sh
MolocoEntryPoint.sendCustomEvent(Constants.PCustomEventType.P_CUSTOM_00, "my_custom_event", dataMap, apiCallback)
```

##### DataMap
- User may send additional information using a `Map<String, Object>` that can be used for user tracking and targetting.

```sh
Map<String, String> dataMap = new HashMap<String, String>();
dataMap.put("gender", "male")
```

##### ApiCallback
- ApiCallback is an interface for handling http responses.
- Must override `handleResponse(E response)`
- Response is a string representation of error (empty string if no error)

```sh
new ApiCallback<String>() {
    @Override
    public void handleResponse(String response) {
        Log.d("com.moloco.vansample", String.format("Error : %s", response));
    }
}
```

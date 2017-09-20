# Moloco VAN SDK - Server 2 Server (S2S)
Moloco VAN SDK provides session and event tracking for mobile applications to facilitate advertising for app publishers.

## Description
For S2S VAN SDK 2.0, VAN user gathers relevant VAN information in their server and make requests to Moloco servers. This has several advantages over using Moloco SDK for native platforms (Android, iOS, Unity).

- No impact for end-users (e.g. app binary size, service latency, etc).
- More sophisticated control (e.g. HTTP protocols, optimization)

However, if you are a mobile app company, we suggest using our native SDK (Android, iOS, Unity) since a lot of work (parsing, optimizing, encoding, and HTTP handling) is already done along with simple interface.

## Prerequisites
Moloco S2S VAN SDK 2.0.0 is compatible for any device, as long as correctly used.

Integrating with Moloco SDK requires a `Product ID` and `Api Key` from Moloco. Contact [Moloco](mailto:support@molocoads.com) if you are missing either value.

## Instructions

Like many other RESTful APIs, VAN APIs use resource entities to construct a URL. Mainly we use two entities `O-Event` and `P-Event` and the final URL format looks like this:

O-Event
> https://tracker-us.adsmoloco.com/tracking/post_o?o=%s

P-Event
> https://tracker-us.adsmoloco.com/tracking/post_p?p=%s

Where `%s` part is an bas64 encoded journal event string value which will be described below.


#### O-Event
This event type is used for an (good) 'outcome' of ad-event. In our analogy, an event of this type is like registration of a company.

#### P-Event 
This event type is used for a 'post-outcome' of ad-event. In our analogy, an event of this type is like making a revenue.


## Supported Operation: POST

We currently accept `POST` operation for tracking events. For a valid `POST` operation, the target `Product Id` and `Api-Key` must be registered by Moloco.

In your `POST` operation, you must set a provided `Api-Key` string value (32-hexadecimal digits) to a Header with `Api-Key` key-value pair.

*HTTP HEADER*
```
Api-Key: "123e4567-e89b-12d3-a456-426655440000"
``` 
![alt text](https://storage.googleapis.com/vansdk/s2s/1.png)

## Data Structures


### JournalEvent

JournalEvent has the following data fields:

```
string event_id;	// a unique id (i.e. UUID)
string maid;		// an advertising ID with a OS (*i* or *a*) type prefix (i.e. i:123e4567-e89b-12d3-a456-426655440000 for iOS)
string product_id;	// a provided product id
EventData event_data;	// an EventData instance (see below)
EventType event_type;	// an EventType enum (see below)
long happen_at_ns;	// the time when the event occurred in nano seconds 
```

### EventData

EventData has the following data fields:

```
// IPv4 address of the device (e.g., 169.254.127.141).
string ip_address = 1;

// Type of the device on which the event happened.
enum DeviceType {
	UNKNOWN_DEVICE = 0;
	PHONE = 1; 		// Smart phone.
	TABLET = 2; 		// Smart device with larger screen than smart phone.
	PERSONAL_COMPUTER = 3; 	// Legacy personal computer in case we support full cross platform.
	CONNECTED_DEVICE = 4; 	// General device connected to the Internet.
	CONNECTED_TV = 5; 	// Smart TV.
}

DeviceType device_type = 2;

// Model name of the device.
string device_model = 3;

// Type of the device connectivity.
enum ConnectionType {
    UNKNOWN_CONNECTION = 0;
    ETHERNET = 1;	// Wired connection.
    WIFI = 2;		// Wireless connection.
    MOBILE = 3; 	// Wireless connection via cellar network.
}

ConnectionType connection_type = 4;

// Carrier name that provides the mobile service to the device (e.g., Verizon).
string carrier = 5;

// ISO-3166-1 Alpha-2 country code (e.g., kr). We use Alpha-2 in the tracker world.
string country_code = 6;

// ISO-639-1 Alpha-2 language code (e.g., ko).
string language = 7;

// Device operating systems version (e.g., 22 or 10.3.1).
string os_version = 8;

// App version code (e.g., 1.7.5).
string app_version = 9;

// VAN SDK version code (e.g., 1.1.3).
string sdk_version = 10;

// A string representation of a key-value dictionary like object,
// encoded to a json string, which is then encoded to standard base64 encoding.
string base64_json_map = 11;

// Use this field to give a meaningful name when the event type is P_CUSTOM_(00,16).
// This field will be ignored in case of non-custom event types.
string custom_event_name = 12;
}
```

#### O-EventType (enum):

```
enum OEventType {
    O_UNKNOWN = 0;
    O_INSTALL = 11;
    O_OPEN = 12;
    O_SESSION = 13;
}
```

#### P-EventType (enum):

```
enum PEventType {
    P_UNKNOWN = 0;
    P_PURCHASE = 21;
    P_REGISTER = 22;
    P_OPEN_COMMUNITY = 23;
    P_INVITE = 24;
    P_CALL = 25;
    P_DELIVERY = 26;
    P_AUTHORIZE = 27;
    P_ADD_TO_CART = 28;
    P_ADD_TO_WISHLIST = 29;
    P_VIEW_CONTENT = 30;
    P_LEVEL_UP = 31;
    P_LOGIN = 32;
    P_RATE = 33;
    P_RESERVE = 34;
    P_SEARCH = 35;
    P_SELL = 36;
    P_SHARE = 37;
    P_COMPLETE_TUTORIAL = 38;
    P_CUSTOM_00 = 100;
    P_CUSTOM_01 = 101;
    P_CUSTOM_02 = 102;
    P_CUSTOM_03 = 103;
    P_CUSTOM_04 = 104;
    P_CUSTOM_05 = 105;
    P_CUSTOM_06 = 106;
    P_CUSTOM_07 = 107;
    P_CUSTOM_08 = 108;
    P_CUSTOM_09 = 109;
    P_CUSTOM_10 = 110;
    P_CUSTOM_11 = 111;
    P_CUSTOM_12 = 112;
    P_CUSTOM_13 = 113;
    P_CUSTOM_14 = 114;
    P_CUSTOM_15 = 115;
}
```

## RESTful API URL Format

As mentioned above, a url format for O-JournalEvent looks like this:

> https://tracker-us.adsmoloco.com/tracking/post_o?o=%s

Indeed, a string value that is appended at the end is an encoded JournalEvent instance.

The following procedure guides the encoding of a finalized string value.


1. Pick an `EventType` (O-EventType or P-EventType) to track.

2. Pick a corresponding `URL endpoint` (post_o? or post_?)

3. Construct an instance of `EventData` with the following fields:
	- ip_address, device_type, connection_type, carrier, country_code, language, os_version, app_version, sdk_version

4. If you want to add additional information,
	1. construct a key-value paired `dictionary` object instance (i.e. `HashMap<Sting, String> dataMap`).
	2. insert data fields as `key-value` pairs (i.e. `dataMap["username"] = "alex"`)
	3. encode the instance object as `json` string value.
	4. encode the `json` string value with `standard base64` string value.
	5. add the finalized string value to `base64_json_string` field of the `EventData` instance at (3)

5. Construct an instance of `JournalEvent` with the following fields:
	- event_id, maid, product_id, event_data (3), event_type (1), happen_at_ns

6. Encode `JournalEvent` instance as the following:
	1. encode the instance object as `json` string value. (i.e. "{"product_id":"myProductId"}")
	2. encode the `json` string value with `standard base64` string value.

7. Append the finalized string value (6) to the end of `URL endpoint` (2)

8. POST finalized API url (7) with Header values including provided `Api-Key` value
	- ex) "Api-Key": "123e4567-e89b-12d3-a456-426655440000"

9. Receive response!


## Validation

You can verify whether your constructed url for RESTful API works using the following url endpoints (same POST method as above)

Verify O-Event
> "https://tracker-us.adsmoloco.com/tracking/post_o?o=%s";

Verify P-Event
> "https://tracker-us.adsmoloco.com/tracking/post_p?p=%s";

If all goes all, you will receive a `valid` response.

*EXAMPLE*
```
Request:
  Request URL: /tracking/validate_p?p=ewogICJtYWlkIiA6ICJpOkI5MDNGQzIwLUM1QTktNDc5Ri04RkZBLTg3MjczMjQ2RDk2NyIsCiAgInByb2R1Y3RfaWQiIDogIm1vbG9jb190ZXN0aW5nX2FwcF9pb3MiLAogICJoYXBwZW5fYXRfbnMiIDogIjE1MDQ0OTA4Njc3MDA3MTYwMDAiLAogICJldmVudF9kYXRhIiA6IHsKICAgICJjb3VudHJ5X2NvZGUiIDogImtyIiwKICAgICJjdXN0b21fZXZlbnRfbmFtZSIgOiAicmVxdWVzdF9hZF9mcm9tX2Fkc2Rpc3BsYXlfaW9zIiwKICAgICJhcHBfdmVyc2lvbiIgOiAiMS40KDEpIiwKICAgICJzZGtfdmVyc2lvbiIgOiAiMi4wLjAiLAogICAgImJhc2U2NF9qc29uX21hcCIgOiAiZXlJaU9pSWlmUT09IiwKICAgICJkZXZpY2VfdHlwZSIgOiAxLAogICAgImNvbm5lY3Rpb25fdHlwZSIgOiAyLAogICAgImxhbmd1YWdlIiA6ICJlbi1LUiIsCiAgICAib3NfdmVyc2lvbiIgOiAiMTAuMy4zIiwKICAgICJpcF9hZGRyZXNzIiA6ICIxNzIuMzAuMS41NSIsCiAgICAiZGV2aWNlX21vZGVsIiA6ICJpUGhvbmU5LDM7aU9TIDEwLjMuMyIsCiAgICAiY2FycmllciIgOiAiS1QiCiAgfSwKICAiZXZlbnRfaWQiIDogIjA1QjA3NzgxLUFEQjQtNEVCQi04MzdFLTQ0M0Y0RjFCQzI5QSIsCiAgImV2ZW50X3R5cGUiIDogMTAwCn0=
  Request Header:
    Via: 1.1 google
    X-Forwarded-For: 59.5.23.57, 35.190.3.234
    X-Forwarded-Proto: https
    Connection: Keep-Alive
    Postman-Token: 55f6ae6d-f65d-484e-8712-a075c70ad826
    Api-Key: 099ad4a40d75dd0b8151630486513e9z
    Accept: */*
    Content-Length: 0
    X-Cloud-Trace-Context: 2e2c895f6c0db487e38ce2617a5f4a06/1384854340696844375
    Cache-Control: no-cache
    User-Agent: PostmanRuntime/6.2.5
    Accept-Encoding: gzip, deflate
- Content Length: 0
  Contents: 
PJournalEvent: json-marshaled for readability
{
    "event_id": "05B07781-ADB4-4EBB-837E-443F4F1BC29A",
    "maid": "i:B903FC20-C5A9-479F-8FFA-87273246D967",
    "product_id": "moloco_testing_app_ios",
    "event_type": 100,
    "event_data": {
      "ip_address": "172.30.1.55",
      "device_type": 1,
      "device_model": "iPhone9,3;iOS 10.3.3",
      "connection_type": 2,
      "carrier": "KT",
      "country_code": "kr",
      "language": "en-KR",
      "os_version": "10.3.3",
      "app_version": "1.4(1)",
      "sdk_version": "2.0.0",
      "base64_json_map": "eyJsYXRpdHVkZSI6MC4xMjM5MjMsInVzZXJuYW1lIjoibW9sb2NvIiwibG9uZ2l0dWRlIjowLjUz\nNDYxMjN9\n",
      "custom_event_name": "custom_click_test_from_myMobileApp"
    },
    "happen_at_ns": 1504490867700716000
  }
Event-Data: base64-decoded for readability
{"latitude":0.123923,"username":"moloco","longitude":0.5346123}
Validation: valid
```

Now you are ready to synchronize your app with *S2S Moloco VAN SDK 2.0* !

Once you complete integration, please work with your Moloco contact to verify receipt of the session and other events from VAN server.

If there is any question regarding with Moloco VAN iOS SDK integration, please contact [Moloco](mailto:support@molocoads.com).

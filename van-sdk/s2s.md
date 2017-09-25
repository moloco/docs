# Moloco VAN API - Server to Server (S2S)
Moloco VAN API supports session and event tracking for mobile applications to facilitate advertising for app publishers.

## Description
For `VAN S2S API`, users gather relevant tracking information in their servers and make requests to Moloco VAN servers. If you are a mobile app company, we suggest using our native VAN SDK ([Android](android.md), [iOS](ios.md), Unity), since a lot of work (parsing, optimizing, encoding, and HTTP handling) is already done along with simple interface.

`VAN S2S API` is encouraged for the following cases:

- Occurrence of an event only handled by webview.
- Events only happening from the server side (e.g., status change of money wire transfer for a banking app, results of invitation to join a community).
- Manipulated events by some post-processing after the in-app activity (e.g., separate "first purchase" event after looking up the payment history from the server side).

`VAN S2S API` can have several advantages over using `VAN SDK` for native platforms:

- No impact for end-users (e.g. app binary size, service latency, etc).
- More sophisticated control (e.g., HTTP protocols, optimization).

## Prerequisites
Moloco `VAN S2S API` is compatible for any platform supporting HTTP protocols.

Integrating with Moloco API requires `Product ID` and `Api Key` from Moloco. Contact [Moloco](mailto:support@molocoads.com) if you are missing either value.

## Instructions

Like many other RESTful APIs, `VAN S2S API` uses resource entities to construct a URL. Mainly we use two entities `O-Event` and `P-Event` and the final URL format looks like this:

O-Event
> https://tracker-us.adsmoloco.com/tracking/post_o?o=%s

P-Event
> https://tracker-us.adsmoloco.com/tracking/post_p?p=%s

Where `%s` part is a *json-base64-url-encoded* string value of a `JournalEvent` instance, which will be described below.

#### O-Event
This event type is used for an (good) 'outcome' of ad-event. In our analogy, an event of this type is like registration of a company.

#### P-Event 
This event type is used for a 'post-outcome' of ad-event. In our analogy, an event of this type is like making a revenue.

## Supported Operation: HTTP POST

We currently accept HTTP `POST` operation for tracking events. For a valid `POST` operation, the target `Product Id` and `Api-Key` must be registered by Moloco.

In your `POST` operation, you must set a provided `Api-Key` string value (32-hexadecimal digits) to a Header with `Api-Key` key-value pair.

*HTTP HEADER*

![](https://storage.googleapis.com/vansdk/s2s/1.png)

## Data Structures

The following data structures are defined as `Protocol Buffers` format described [here](https://developers.google.com/protocol-buffers/).

### JournalEvent

JournalEvent has the following data fields:

```
string event_id;      // a unique id (e.g., UUID)
string maid;          // an advertising ID with a OS type prefix ("i:" or "a:"), e.g. i:123e4567-e89b-12d3-a456-426655440000 for iOS
string product_id;    // a provided product id
EventData event_data; // an EventData instance (see below)
EventType event_type; // an EventType enum (see below)
long happen_at_ns;    // the time when the event occurred in nano seconds 
```

### EventData

EventData has the following data fields:

```
// IPv4 address of the device (e.g., 169.254.127.141).
string ip_address = 1;

// Type of the device on which the event happened.
enum DeviceType {
  UNKNOWN_DEVICE = 0;
  PHONE = 1;              // Smart phone.
  TABLET = 2;             // Smart device with larger screen than smart phone.
  PERSONAL_COMPUTER = 3;  // Legacy personal computer in case we support full cross platform.
  CONNECTED_DEVICE = 4;   // General device connected to the Internet.
  CONNECTED_TV = 5;       // Smart TV.
}

DeviceType device_type = 2;

// Model name of the device.
string device_model = 3;

// Type of the device connectivity.
enum ConnectionType {
    UNKNOWN_CONNECTION = 0;
    ETHERNET = 1; // Wired connection.
    WIFI = 2;     // Wireless connection.
    MOBILE = 3;   // Wireless connection via cellar network.
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

// VAN sdk version code (e.g., 1.1.3).
string sdk_version = 10;

// A string representation of a key-value dictionary like object,
// encoded to a json string, which is then encoded to standard base64-encoding.
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

As mentioned above, a URL format for O-JournalEvent looks like this:

> https://tracker-us.adsmoloco.com/tracking/post_o?o=%s

`%s` must be replaced with a *json-base64-url-encoded* string value of a `JournalEvent` instance which contains event tracking information.

To build the string value above, please follow the following procedure:


1. Pick a target `EventType` (O-EventType or P-EventType) for tracking.

2. Pick a corresponding `URL endpoint` (post_o? or post_?)

3. Construct an instance of `EventData` with the following fields:
  - `ip_address`, `device_type`, `connection_type`, `carrier`, `country_code`, `language`, `os_version`, `app_version`, `sdk_version`

4. If you want to add additional information,
  - construct a key-value paired `dictionary` object instance (e.g., `HashMap<Sting, String> dataMap`).
  - insert data fields as `key-value` pairs (e.g., `dataMap["username"] = "alex"`)
  - encode the instance object as `json` string value.
  - encode the `json` string value with `standard base64` encoding.
  - add the finalized string value to `base64_json_string` field of the `EventData` instance built in (3).

5. Construct an instance of `JournalEvent` with the following fields:
  - `event_id`, `maid`, `product_id`, `event_data` (3), `event_type` (1), `happen_at_ns`.

6. Encode `JournalEvent` instance as the following:
  - encode the instance object as `json` string value. (e.g., "{"product_id":"myProductId"}")
  - encode the `json` string value above with `standard base64` encoding.
  - `URL encode` the `standard base64` string above.

7. Append the finalized string value (6) to the end of `URL endpoint` (2).

8. POST finalized API URL (7) with Header values including provided `Api-Key`.
  - e.g., Api-Key: "111ad1a11d11dd1b1111111111111e1z"

9. Receive response from the server!

## Final URL demonstration

### Go

```go
package main

import (
  b64 "encoding/base64"
  "encoding/json"
  "fmt"
  "net/url"
)


type JournalEvent struct {
  EventId     string    `json:"event_id"`
  Maid        string    `json:"maid"`
  ProductId   string    `json:"product_id"`
  EventData   EventData `json:"event_data"`
  EventType   int64     `json:"event_type"`
  HappenAtNs  int64     `json:"happen_at_ns"`
}

type EventData struct {
  IpAddress       string  `json:"ip_address"`
  DeviceType      int64   `json:"device_type"`
  DeviceModel     string  `json:"device_model"`
  ConnectionType  int64   `json:"connection_type"`
  Carrier         string  `json:"carrier"`
  CountryCode     string  `json:"country_code"`
  Language        string  `json:"language"`
  OsVersion       string  `json:"os_version"`
  AppVersion      string  `json:"app_version"`
  SdkVersion      string  `json:"sdk_version"`
  Base64JsonMap   string  `json:"base64_json_map"`
  CustomEventName string  `json:"custom_event_name"`
}

const (
  p_event_url_format = "https://tracker-us.adsmoloco.com/tracking/post_p?p=%s"
  p_login = 32
  // define other event types here
)

func main() {
    journalEvent := &JournalEvent{
      EventId:    "05B07781-ADB4-4EBB-837E-443F4F1BC29A",
      Maid:       "a:B903FC20-C5A9-479F-8FFA-87273246D96a",
      ProductId:  "moloco_van_s2s_testing_app",
      EventType:  p_login,
      EventData:  EventData{
        IpAddress:        "175.203.211.101",
        DeviceType:       1,
        DeviceModel:      "iPhone7",
        ConnectionType:   2,
        Carrier:          "kt",
        Language:         "EN",
        OsVersion:        "1.2.3",
        AppVersion:       "1.0.0",
        SdkVersion:       "2.0.0",
        Base64JsonMap:    "",
        CustomEventName:  "",
      },
      HappenAtNs: 1504490867700716000,
    }

    // Encode data map for Base64JsonMap field.
    dataMap := map[string]string{"user" : "alex"}
    dataMapString, _ := json.Marshal(dataMap)
    // dataMapString: {"user":"alex"}
    dataMapBase64 := b64.StdEncoding.EncodeToString([]byte(dataMapString))
    // dataMapBase64: eyJ1c2VyIjoiYWxleCJ9
    journalEvent.EventData.Base64JsonMap = dataMapBase64
    
    // Encode JournalEvent to Json-Base64-UrlEscaped string.
    jsonString, _ := json.Marshal(journalEvent)
    // jsonString: {"event_id":"05B07781-ADB4-4EBB-837E-443F4F1BC29A","maid":"i:B903FC20-C5A9-479F-8FFA-87273246D96a","product_id":"moloco_van_s2s_test_app","event_type":32,"event_data":{"ip_address":"175.203.211.101","device_type":1,"device_model":"iPhone7","connection_type":2,"carrier":"kt","country_code":"","language":"EN","os_version":"1.2.3","app_version":"1.0.0","sdk_version":"2.0.0","base64_json_map":"eyJ1c2VyIjoiYWxleCJ9","custom_event_name":""},"happen_at_ns":1504490867700716000}

    encodedString := b64.StdEncoding.EncodeToString([]byte(jsonString))
    // encodedString: eyJldmVudF9pZCI6IjA1QjA3NzgxLUFEQjQtNEVCQi04MzdFLTQ0M0Y0RjFCQzI5QSIsIm1haWQiOiJhOk...

    escapedString := url.QueryEscape(encodedString)
    // escapedString: eyJldmVudF9pZCI6IjA1QjA3NzgxLUFEQjQtNEVCQi04MzdFLTQ0M0Y0RjFCQzI5QSIsIm1haWQiOiJhOk...

    finalURL := fmt.Sprintf(p_event_url_format, escapedString)
    // finalURL: https://tracker-us.adsmoloco.com/tracking/post_p?p=eyJldmVudF9pZCI6IjA1QjA3NzgxLUFE...
}
```


## Validation

You can validate whether your constructed `URL` for `Moloco VAN S2S API` works using the following validation URL endpoints (same POST method as above)

Verify O-Event
> https://tracker-us.adsmoloco.com/tracking/validate_o?o=%s

Verify P-Event
> https://tracker-us.adsmoloco.com/tracking/validate_p?p=%s

If all goes well, you will receive a `valid` response.

**EXAMPLE**
```
Request:
  Request URL: /tracking/validate_p?p=ewogICJtYWlkIiA6ICJpOkI5MDNGQzIwLUM1QTktNDc5Ri04RkZBLTg3MjczMjQ2RDk2NyIsCiAgInByb2R1Y3RfaWQiIDogIm1vbG9jb190ZXN0aW5nX2FwcF9pb3MiLAogICJoYXBwZW5fYXRfbnMiIDogIjE1MDQ0OTA4Njc3MDA3MTYwMDAiLAogICJldmVudF9kYXRhIiA6IHsKICAgICJjb3VudHJ5X2NvZGUiIDogImtyIiwKICAgICJjdXN0b21fZXZlbnRfbmFtZSIgOiAicmVxdWVzdF9hZF9mcm9tX2Fkc2Rpc3BsYXlfaW9zIiwKICAgICJhcHBfdmVyc2lvbiIgOiAiMS40KDEpIiwKICAgICJzZGtfdmVyc2lvbiIgOiAiMi4wLjAiLAogICAgImJhc2U2NF9qc29uX21hcCIgOiAiZXlJaU9pSWlmUT09IiwKICAgICJkZXZpY2VfdHlwZSIgOiAxLAogICAgImNvbm5lY3Rpb25fdHlwZSIgOiAyLAogICAgImxhbmd1YWdlIiA6ICJlbi1LUiIsCiAgICAib3NfdmVyc2lvbiIgOiAiMTAuMy4zIiwKICAgICJpcF9hZGRyZXNzIiA6ICIxNzIuMzAuMS41NSIsCiAgICAiZGV2aWNlX21vZGVsIiA6ICJpUGhvbmU5LDM7aU9TIDEwLjMuMyIsCiAgICAiY2FycmllciIgOiAiS1QiCiAgfSwKICAiZXZlbnRfaWQiIDogIjA1QjA3NzgxLUFEQjQtNEVCQi04MzdFLTQ0M0Y0RjFCQzI5QSIsCiAgImV2ZW50X3R5cGUiIDogMTAwCn0=
  Request Header:
    Via: 1.1 google
    X-Forwarded-For: 59.5.23.57, 35.190.3.234
    X-Forwarded-Proto: https
    Connection: Keep-Alive
    Postman-Token: 55f6ae6d-f65d-484e-8712-a075c70ad826
    Api-Key: 111ad1a11d11dd1b1111111111111e1z
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
    "maid": "i:B903FC20-C5A9-479F-8FFA-87273246D96a",
    "product_id": "moloco_van_s2s_test_app",
    "event_type": 32,
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

Now you are ready to synchronize your app with **S2S Moloco VAN API 2.0** !

Once you complete integration, please work with your Moloco contact to verify receipt of the session and other events from VAN server.

If there is any question regarding with `Moloco VAN S2S API` integration, please contact [Moloco](mailto:support@molocoads.com).
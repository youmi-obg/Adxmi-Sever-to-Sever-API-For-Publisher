# Server To Server Offer API For Publisher

- The API is only accessible by HTTP GET request and returns JSON.
- All the offers in your feed is active for you, but we will remove in-active offers from time to time, so please make sure to request every 15 min - 30 min.
- Excessive requests in short period would result in HTTP response with status 429, and provide back-off seconds in 'X-'

Endpoint: http://ad.api.yyapi.net/v2/offline

## Request Parameter

| Parameter   | Type   | Mandatory | Description       |
|-------------|--------|-----------|-------------------------------------------------------------------------------------------------------------------|
| app_id      | string | Y         | Retrieve from our publisher website              |
| page_size   | int    | Y         | Define the number of offers per page, page_size would be 500 in maxmium in case of request timeout             |
| page        | int    | Y         | Define which page to fetch, starting with page 1      |
| os          | array  | N         | Filter offer by target os|
| country     | array  | N         | Filter offer by target country|
| payout_type | array  | N         | Filter offer payout_type|

## Response Parameter

| Parameter           | Type   | Description          |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                  | string | The id of the offer|
| name                | string | The name of the offer                 |
| package             | string | The package name of the offer         |
| adtxt               | string | The introduction of the offer         |
| payout              | double | The payout (in USD) of the offer, 0 if dynamic payout |
| cap                 | int    | Maximum allowed conversion, 0 if open cap                |
| trackinglink        | string | The link that is used to track conversion                |
| country             | array  | Target country, empty means global |
| os                  | array  | Target os: android / ios        |
| traffic             | string | incentive or non-incentive        |
| os_version          | string | The min os version of the offer (eg:4.2.2)                 |
| carrier             | array  | The target carrier of the offer       |
| preview_url         | string | The preview url of the offer          |
| icon_url            | string | The link to the icon of the offer     |
| creative            | array  | The image materials of the offer      |
| video               | array  | The video materials of the offer      |
| store_label         | array  | The store (AppStore/GooglePlay) label of the offer       |
| store_rating        | string | The store (AppStore/GooglePlay) rating of the offer      |
| size                | string | The size of the package               |
| conversion_flow     | string | Publisher can get a convertion only if a user completing this convertion flow.|
| payout_type         | string | CPI: This means the offer is from an app store.<br> CPA: This means user will be redirected to a web task         |
| mandatory_device    | map<string, bool>  | required device parameters if it's marked as "true", see tracking link macro for details, failing to pass required parameters would result in invalid click response |
| category            | string | The category of the offer: APP / ADULT / SMARTLINK / SUBSCRIPTION |

## Example

```
GET http://ad.api.yyapi.net/v2/offline?app_id=b3a3277b8fdd54bc&page=1&page_size=20&os=android&country=US

{
  "c": 0,
  "total": 1,
  "page": 1,
  "page_size": 1,
  "n": 1,
  "offers": [
    {
      "id": "864378489573216256",
      "name": "Space Manager",
      "package": "com.mobileartsme.spacemanager",
      "adtxt": "SPACE MANAGER mobile application allows you to effortlessly back up and organize data on your mobile deviceIt will automatically upload your data on cloud when limit set by you get reached, freeing up your device&#39;s internal memory and improving your phone&#39;s performance Space Manager is a utility app that provides numerous features: - View and organize your data on: Internal memory, SdCard memory and Online memory (Cloud storage) - Up to 500 GB of online space to backup your images, videos and audios- Get notified when internal phone storage is less than limit set in settings- Manage content automatically by choosing size of images/videos/audios to copy or move to cloud, then Space Manager will automatically select your oldest data from your internal memory- Run automatic free up space  Space Manager FREE version: - 2 GB of free online storage - All features provided by Space manager  Space Manager Premium: Subscribe once, and sit back and enjoy all FREE &amp; Premium features of Space Manage",
      "payout": 1.12,
      "cap": 0,
      "trackinglink": "http://t.api.yyapi.net/v1/tracking?ad=864378489573216256&app_id=b3a3277b8fdd54bc&pid=3",
      "country": ["AE","BH","QA"],
      "os": ["android"],
      "traffic": "incentive",
      "os_version": "",
      "carrier": [],
      "device": [],
      "preview_url": "https://play.google.com/store/apps/details?id=com.mobileartsme.spacemanager",
      "icon_url": "https://lh3.googleusercontent.com/JErLykXFTGx8E88SwMzDE4m2jEFSXlrbEGrXm9rO-q5kkyJIXl9vvkzh9979NPNdDb0=w96",
      "creative": [
        {
          "url": "https://lh3.googleusercontent.com/KyqDq7p4f_bKV5gJVpgayLVAXW8GXznwDYEpfVfI4nhHcf-nfwIl3WFXXnDcxk0kOKw",
          "mime": "image/png",
          "width": 288,
          "height": 512
        }
      ],
      "video": [
        {
          "url": "https://xxx.mp4",
          "mime": "video/mp4"
        }
      ],
      "store_label": ["Tools"],
      "store_rating": "4.2",
      "size": "16M",
      "conversion_flow": "",
      "payout_type": "CPI",
      "mandatory_device": {
        "imei": false,
        "mac": false,
        "andid": true,
        "advid": false,
        "idfa": false,
        "udid": false
      },
      "category": "APP"
    }
  ]
}

## Tracking link macro

| Parameter | Description |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| user_id   | For your click_id or transaction_id, to uniquely identify a single click |
| chn       | For your sub_source_id or placement_id (maximum 150) |
| imei      | user's IMEI, mandatory if "true" in "mandatory_device"                |
| mac       | user's MAC Address, mandatory if "true" in "mandatory_device"           |
| andid     | user's Android ID, mandatory if "true" in "mandatory_device"      |
| advid     | user's Google Advertising ID (GAID), mandatory if "true" in "mandatory_device"  |
| idfa      | user's IDFA, mandatory if "true" in "mandatory_device"          |
| udid      | user's UDID, mandatory if "true" in "mandatory_device"  |
| package   | package name of the app originating the click            |
| aff_sub1  | For your custom parameter (maximum 256)                  |
| aff_sub2  | For your custom parameter (maximum 256)                  |
| aff_sub3  | For your custom parameter (maximum 256)                  |

### Notes

- Failing to provide required device parameters would result in invalid click. It is advertised to pass device parameters whenever possible, since the info will better identify the origin of the click, and lead to better conversion ratio.
- Lengthy parameters would be truncated on our side, so please respect the maximum length requirement.

# Offer Callback Protocol

Normally we send callback immediately when conversion happens.
However, on rare scenarios like server failure, delayed conversion confirm, etc., delayed callbacks (up to 7 days) are expected.
We send callback request via HTTP GET method, retry on request failure (HTTP response code 5XX).
Multiple callbacks related to the same conversion is possible, so it's receiver's duty to make sure the callback endpoint is idempotent.

## Callback macro

| Macro         | Description|
|---------------|------------------------------------------------------------------------------------------------------------------------------|
| {user_id}     | Your click_id  or  transaction_id |
| {chn}         | Your sub_source_id or placement_id|
| {order_id}    | The unique id of this transactionIf developer receives the same order, that means the transaction is already sent.|
| {ad}          | Offer ID|
| {package}     | The package name of this offer|
| {revenue}     | The revenue($) that developer can get|
| {aff_sub1}    | Passed in tracking link|
| {aff_sub2}    | Passed in tracking link|
| {aff_sub3}    | Passed in tracking link|

## Callback IP Whitelist

Available on request.

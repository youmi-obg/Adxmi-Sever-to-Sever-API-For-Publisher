# Server To Server Offer API For Publisher

Endpoint: http://ad.api.yyapi.net/v2/offline

## Request Parameter

| Parameter   | Type   | Mandatory | Description       |
|-------------|--------|-----------|-------------------------------------------------------------------------------------------------------------------|
| app_id      | string | Y         | Identification key, available from our publisher website              |
| page_size   | int    | N | Define the number of offers per page, page_size should be no greater than 10000 in case of request timeout             |
| page        | int    | N | Define which page to fetch, starting from 1      |
| payout_type | string  | N         | Filter offer payout_type: CPA / CPI / CPL |
| os          | string  | N         | Filter offer by target OS: android / ios |
| country     | string  | N         | Filter offer by target country, use `,` to separate multiple countries |
| offer_ids     | string  | N         | Filter offer by target offers, use , to separate multiple offers |

## Response Parameter

| Parameter           | Type   | Description          |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                  | string | The id of the offer|
| name                | string | The name of the offer                 |
| package             | string | The package name of the offer         |
| kpi                 | string | The KPI description of the offer      |
| adtxt               | string | The introduction of the offer         |
| payout              | double | The payout (in USD) of the offer, 0 if dynamic payout. Not necessarily the settled payout, we recommend to add `{revenue}` macro in you postback link (see below) |
| cap                 | int    | Maximum allowed total conversion per day, 0 if open cap                |
| trackinglink        | string | The tracking link of the offer |
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
| conversion_flow     | string | Publisher can get a conversion only if the user complete this conversion flow.|
| payout_type         | string | CPI: This means the offer is from an app store.<br> CPA: This means user will be redirected to a web task.<br> CPL: This means the offer is paid for an explicit sign-up         |
| mandatory_device    | map<string, bool>  | required device parameters if it's marked as "true", see tracking link macro for details, failing to pass required parameters would result in invalid click response |
| stream_type            | string | The stream type of offer: APP / ADULT / SMARTLINK / SUBSCRIPTION |
| category            | string | Advertising classification of Youmi, similar to Google Play |
| task_description_for_user            | string | Task description for user |
## Example

```
GET http://ad.api.yyapi.net/v2/offline?app_id=c46b362886e42385d30c83d76abc3c51&page=1&page_size=20&os=android&country=IN&payout_type=CPI

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
            "kpi": "hard: CTIT less than 20 seconds will not be paid",
            "adtxt": "SPACE MANAGER mobile application allows you to effortlessly back up and organize data on your mobile deviceIt will automatically upload your data on cloud when limit set by you get reached, freeing up your device&#39;s internal memory and improving your phone&#39;s performance Space Manager is a utility app that provides numerous features: - View and organize your data on: Internal memory, SdCard memory and Online memory (Cloud storage) - Up to 500 GB of online space to backup your images, videos and audios- Get notified when internal phone storage is less than limit set in settings- Manage content automatically by choosing size of images/videos/audios to copy or move to cloud, then Space Manager will automatically select your oldest data from your internal memory- Run automatic free up space  Space Manager FREE version: - 2 GB of free online storage - All features provided by Space manager  Space Manager Premium: Subscribe once, and sit back and enjoy all FREE &amp; Premium features of Space Manage",
            "payout": 1.12,
            "cap": 0,
            "trackinglink": "http://t.api.yyapi.net/v1/tracking?ad=864378489573216256&app_id=b3a3277b8fdd54bc&pid=3",
            "country": [
                "AE",
                "BH",
                "QA"
            ],
            "os": [
                "android"
            ],
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
            "store_label": [
                "Tools"
            ],
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
            "category": "APP",
            "task_description_for_user": {
                "en": "1. Download and Install App\n2. Create an account\n3. Open the App and play 15s at least"
            }
        }
    ]
}
```

## Notes

- The API is only accessible by HTTP GET request and returns JSON.
- We ensure all the offer you get is alive at the time of you request, but we might offline offers at any time. To minimize invalid clicks on offline offers, we advise you to request every 15 min - 30 min, and disappeared offers should be deemed unaccessable to you.
- Excessive requests in short period might result in 429 (Too Many Requests) http response code, retry seconds is provided in 'Retry-After' response header.

## Tracking link parameter

| Parameter | Description |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| chn       | For your sub_source_id or placement_id (maximum 150) |
| imei      | user's IMEI, mandatory if "true" in "mandatory_device"                |
| mac       | user's MAC Address, mandatory if "true" in "mandatory_device"           |
| andid     | user's Android ID, mandatory if "true" in "mandatory_device"      |
| advid     | user's Google Advertising ID (GAID), mandatory if "true" in "mandatory_device"  |
| idfa      | user's IDFA, mandatory if "true" in "mandatory_device"          |
| udid      | user's UDID, mandatory if "true" in "mandatory_device"  |
| device_id      | user's Device ID(for example OAID)  |
| app_name   | package name of the app originating the click            |
| aff_sub  | For your click_id or transaction_id, to uniquely identify a single click(maximum 256)                  |
| aff_sub2  | For your custom parameter (maximum 256)                  |
| aff_sub3  | For your custom parameter (maximum 256)                  |
| ua        | Do not extra the CFNetwork user-agent,extract the other one(URL encoded)                   |
| language  | Provide the language and locale;Example,en-US                  |
| ip        | ip of the user                  |
| os_version   | The device operating system version. <br />For Example:<br />    Android: 12 <br />    iOS: 16.2                 |
| device_model | The device model.  <br />For Example: <br />    Android: Pixel 5 <br />    iOS:Either iphone or ipad (all lowercase)                 |

### Notes

- Failing to provide required device parameters would result in invalid click. It is advertised to pass device parameters whenever possible, since the info will better identify the origin of the click, and lead to better conversion ratio.
- Lengthy parameters would be truncated on our side, so please respect the maximum length requirement.

# Conversion Callback Protocol

Normally we send callback immediately when conversion happens.
However, on rare scenarios like server failure, delayed conversion confirm, etc., delayed callbacks (up to 7 days) are expected.
We send callback request via HTTP GET method, retry on request failure (HTTP response code 5XX).
Multiple callbacks related to the same conversion is possible, so it's receiver's duty to make sure the callback endpoint is idempotent.

## Callback macro

| Macro         | Description|
|---------------|------------------------------------------------------------------------------------------------------------------------------|
| {chn}         | Your sub_source_id or placement_id|
| {oid}        | Offer ID|
| {package}     | The package name of this offer|
| {revenue}     | The settled price (in USD) for this conversion |
| {aff_sub}    | Your click_id or transaction_id|
| {aff_sub2}    | Passed in tracking link|
| {aff_sub3}    | Passed in tracking link|

## Callback IP Whitelist

Available on request.

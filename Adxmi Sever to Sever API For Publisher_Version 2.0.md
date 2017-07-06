# Adxmi Sever to Sever API For Publisher_Version 2.0
****



## To Get Started
#### Step1: Adxmi Publisher Account
The API is supported only for partners who have Adxmi account. Please register on our website https://www.adxmi.com/account/signup before integrating Adxmi API.
#### Step2: Adxmi Offer Access Parameter
Please create an app, get your app_id from app details. You will need to contact your account manager to get your app approved to Adxmi API.
#### Step3:Requesting Adxmi Offers
Publisher can request offers with approved app_id. Your app_id is the identification of your account, please keep it confidential, since that anyone can use it on behalf of you.

## General Instructions
1. The API is only accessible by HTTP GET request and returns JSON.

2. All the offers in your feed is active for you, but Adxmi will remove in-active offers from time to time, so please make sure to request every 15 min - 30 min.

3. At most 50 requests in 10 min will be proceeded for every publisher. Once exceeded, you will get response code indicating exceed qps limit.


## API Request
#### API Request url

>  http://ad.api.yyapi.net/v2/offline

#### Request Parameter


| Parameter   | Type   | Mandatory | Example                 | Description                                                                                                      |
|-------------|--------|-----------|-------------------------|------------------------------------------------------------------------------------------------------------------|
| app_id      | string | Y         | app_id=b3a3277b8fdd54bc | Applied from www.adxmi.com to access API.                                                                        |
| page_size   | int    | Y         | page_size=500           | Define the number of offers per page, page_size would be 500 in maxmium in case of request timeout.              |
| page        | int    | Y         | page=1                  | Define which page to fetch, starting with page 1.                                                                |
| os          | array  | N         | os=android              | Feild: android、ios. Both Android and iOS offers would be responsed if publisher don't set this parameter.       |
| country     | array  | N         | country=US,CN,AU        | Used to get offers of specific countries.  All offers would be responsed if publisher don't set this parameter.  |
| payout_type | array  | N         | payout_type=CPI         | Used to get offers of specific payout_type. All offers would be responsed if publisher don't set this parameter. |



##### Example

>http://ad.api.yyapi.net/v2/offline?app_id=b3a3277b8fdd54bc&page=1&page_size=20&os=android&country=US

## Response Field
#### Response Parameter

| Parameter        | Description                                                                                                         | Type   |
|------------------|---------------------------------------------------------------------------------------------------------------------|--------|
| id               | The id of the offer.                                                                                                 | string |
| name             | The name of the offer.                                                                                               | string |
| package          | The package name of the offer.                                                                                       | string |
| adtxt            | The introduction of the offer.                                                                                      | string |
| payout           | The revenue(USD) of the offer.                                                                                         | double |
| `cap`             | `The maximum conversion allowed of the offer for all publishers` | int    |
| trackinglink     | The link that is used to track conversion.                                                                           | string |
| `country`        | `The target countries of the offer(empty array means global)`.                                                               | array  |
| `os`              | android <br> ios                                                        | array|
| `traffic`          | Feild:incentive or non-incentive                                                                 | string|
| os_version       | The min os version of the offer (eg:4.2.2)                                                                          | string |
| carrier          | The target carrier of the offer.                                                                                    | array|
| device           | The target hardware brand of the offer.                                                                              | array|
| nettype          | The target net type of the offer(`2g`、`2.5g`、`3g`、`4g`、`wifi`).                                                  | array|
| preview_url      | The preview url of the offer.                                                                                        | string |
| icon_url         | The link to the icon of the offer.                                                                                   | string |
| creative        | The image materials of the offer.                                                                                    | array  |
| video        | The video materials of the offer.                                                                                    | array  |
| store_label      | The store (AppStore/GooglePlay) label of the offer.                                                                  | array  |
| store_rating     | The store (AppStore/GooglePlay) rating of the offer.                                                                 | string  |
| size             | The size of the package.                                                                                             | string |
| conversion_flow             | Publisher can get a convertion only if a user completing this convertion flow.                                                                                           | string |
| payout_type            | Feild: <br> CPI: This means the offer is from an app store.<br> CPA: This means user will be redirected to a web task.                                                                                         | string |
| `mandatory_device` | `Publishers need to transmit these device parameters if it's marked as "true". ` <br>     "imei" stands for user's device IMEI <br>  "mac" stands for the mac address of user's device. <br> "andid" stands for the android_id of user's device.  <br> "advid" stands for the Google Advertising ID of user's device. <br>  "idfa" stands for the idfa of user's iOS device. <br> "udid" stands for user's UDID.                  | array  |
| subsource_blacklist | Adxmi will list the subsource that was blocked by the advertiser, maybe due to bad quality. Publisher should block by yourself or the click would be blocked by Adxmi. Adxmi wouldn't inform you by other forms specifically         | array  |
| subsource_whitelist | This offer was only opened to these subsources of you. Publisher should make sure that you will just target these subsources. Clicks from other subsources would be blocked by Adxmi. | array  |

#### Notice!!!
- "cap" will be 0 if the advertiser didn't set a cap for this offer.
- "country" will be an empty array if the offer is targeting for global.
- We suggest publishers transmit GAID/IDFA of all the users as most advertisors require it.
- Clicks in the wrong country, missing mandatory device parameters, wrong trageting os_version, would be blocked by the server, so please make sure to target users that meet the requirements of our offers.

#### Example

```javascript
{
  "c": 0,
  "total": 100,
  "page": 1,
  "page_size": 1,
  "n": 1,
  "offers": [
    {
      "id": "864378489573216256",
      "name": "Space Manager",
      "payout": 1.12,
      "cap": 0,
      "preview_url": "https://play.google.com/store/apps/details?id=com.mobileartsme.spacemanager",
      "country": [
        "AE",
        "BH",
        "QA"
      ],
      "store_label": [
        "Tools"
	  ],
      "store_rating": "4.2",
      "os": [
        "android"
      ],
      "traffic": "incentive",
      "os_version": "",
      "carrier": [],
      "nettype": [
        "wifi"
      ],
      "creative": [
        {
          "url": "https://lh3.googleusercontent.com/KyqDq7p4f_bKV5gJVpgayLVAXW8GXznwDYEpfVfI4nhHcf-nfwIl3WFXXnDcxk0kOKw",
          "mime": "image/png",
          "width": 288,
          "height": 512
        }
      ],
      "size": "16M",
      "device": [],
      "mandatory_device": {
        "imei": false,
        "mac": false,
        "andid": false,
        "advid": false,
        "idfa": false,
        "udid": false
      },
      "icon_url": "https://lh3.googleusercontent.com/JErLykXFTGx8E88SwMzDE4m2jEFSXlrbEGrXm9rO-q5kkyJIXl9vvkzh9979NPNdDb0=w96",
      "adtxt": "SPACE MANAGER mobile application allows you to effortlessly back up and organize data on your mobile device. It will automatically upload your data on cloud when limit set by you get reached, freeing up your device&#39;s internal memory and improving your phone&#39;s performance.  Space Manager is a utility app that provides numerous features: - View and organize your data on: Internal memory, SdCard memory and Online memory (Cloud storage) - Up to 500 GB of online space to backup your images, videos and audios. - Get notified when internal phone storage is less than limit set in settings. - Manage content automatically by choosing size of images/videos/audios to copy or move to cloud, then Space Manager will automatically select your oldest data from your internal memory. - Run automatic free up space  Space Manager FREE version: - 2 GB of free online storage - All features provided by Space manager  Space Manager Premium: Subscribe once, and sit back and enjoy all FREE &amp; Premium features of Space Manage",
      "category": "SDL",
      "package": "com.mobileartsme.spacemanager",
      "conversion_flow": "",
      "video": [
          "url": "https://xxx.mp4",
          "mime": "video/mp4",
          "width": 288,
          "height": 512
      ],
      "payout_type": "CPI",
      "subsource_blacklist": [],
      "subsource_whitelist": [],
      "trackinglink": "http://t.api.yyapi.net/v1/tracking?ad=864378489573216256&app_id=b3a3277b8fdd54bc&pid=3&user_id={user_id}"
    }
  ]
}
```


### Tracking Link

| Parameter | Description                                                                                                                                                                            |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| user_id   | For your  `click_id ` or  `transaction_id `                                                                |
| chn       | For your `sub_source_id` or `placement_id`(less than 150 chars)                                                                                                                                           |
| imei      | Publisher must transmit user's IMEI in trackinglink, if it's "true" in "mandatory_device"                |
| mac       | Publisher must transmit user's MAC Address in trackinglink, if it's "true" in "mandatory_device"                              |
| andid     | Publisher must transmit user's Android ID in trackinglink, if it's "true" in "mandatory_device"      |
| advid     | Publisher must transmit user's Google Advertising ID(GAID) in trackinglink, if it's "true" in "mandatory_device"  |
| idfa      | Publisher must transmit user's IDFA in trackinglink, if it's "true" in "mandatory_device"          |
| udid      | Publisher must transmit user's UDID in trackinglink, if it's "true" in "mandatory_device"                    |
| aff_sub   | For your custom parameter                  |
| aff_sub2  | For your custom parameter                  |
| aff_sub3  | For your custom parameter                  |

#### Example

http://ad.api.yyapi.net/v1/tracking?ad=782801020756430848&app_id=4b84c1788615000d&pid=3&user_id=6lnp2meG4jrcC42o0xRKkF9u7jamDCur&chn=subsource1

## Common Error Response

| Key         | Description                                                                                                                                               |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| {"c":-1002} | app_id not exists, please check your app_id.                                                                                                                                                                                                                                                                     |
| {"c":-1202} | Application unapproved, please reach for your account manager.                                                                                                                                                                                                                                                       |
| {"c":-1403} | Application didn't pass the verification or publisher didn't turn on the "Live" button for the application in "ADs Settings" - "Ad Units" of ADXMI Panel. |
| {"c":-2103} | Wrong offer id, please check the offer id in your tracking link.                                                                                                                                          |
| {"c":-2221} | Offer in-active, please switch for another offer.                                                                                                                                        |
| {"c":-3006} | Missing required device parameters, please check the "mandatory_device" feild of the offer.                                                                                                                              |
| {"c":-3212} | Country mismatch, please don't click in the wrong IP region.                                                                                                                                          |
| {"c":-3302} | Exceed qps limit, at most 50 requests in 10min will be proceeded for every publisher.                                                                                                                                          |

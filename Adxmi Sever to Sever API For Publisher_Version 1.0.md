# Adxmi Sever to Sever API For Publisher_Version 1.0
****

## DESCRIPITIONS
**Adxmi Sever to Sever Api** is a product that designs for our cooperators to get all information of Adxmi offers with a programmatic way.

Here is the general principle of Adxmi Sever to Sever API

1. The Api is only accessible by HTTP GET request and returns JSON.
2. The Api is supported only for the publishing partners who have Adxmi account and approved app.



In order to use this api, publishers need to go to our official website (www.adxmi.com) to create an application and get the app_id and app_secret, which are used for Adxmi Api request.

## API Request
### API Request url

>  http://ad.api.yyapi.net/v1/offline

### Request Parameter


| Parameter | Description                                  | Type   | Mandatory |
|-----------|----------------------------------------------|--------|-----------|
| app_id    | Apply from www.adxmi.com for the application | string | Y         |
| page_size | Define the number of offers per page         | int    | Y         |
| page      | Define which page to fetch(start with 1)     | int    | Y         |
| os        | enum: `android`, `ios`                       | string | N         |

#### Notice
- page_size would like be less than 500 in case of request timeout.
- every 10 mins, at most 50 requests will be proceeded for every publisher. Once exceeded, you will get response code indicating exceed qps limit.

#### Example

>http://ad.api.yyapi.net/v1/offline?app_id=b3a3277b8fdd54bc&page=1&page_size=20

## Response Field
### Response Parameter

| Parameter        | Description                                                                                                         | Type   |
|------------------|---------------------------------------------------------------------------------------------------------------------|--------|
| id               | The id of the offer                                                                                                 | string |
| name             | The name of the offer                                                                                               | string |
| package          | The package name of the offer                                                                                       | string |
| adtxt            | The introduction of the offer                                                                                       | string |
| task             | The introduction of how to complete the offer                                                                       | string |
| payout           | The revenue($) of the offer                                                                                         | double |
| point            | The amount of virtual currency that will be earned for completing the offer(exchange rate is set on www.adxmi.com ) | int    |
| cap              | The maximum conversion allowed of the offer                                                                         | int    |
| trackinglink     | The link that is used to track conversion                                                                           | string |
| countries        | The target countries of the offer(empty means global)                                                               | array  |
| os               | The target operating system of the offer(`ios` or `android`)                                                        | string |
| traffic          | The value is `incentive` or `non-incentive`                                                                         | string |
| os_version       | The min os version of the offer (eg:4.2.2)                                                                          | string |
| carrier          | The target carrier of the offer                                                                                     | string |
| device           | The target hardware brand of the offer                                                                              | string |
| nettype          | The target net type of the offer(`2g`、`2.5g`、`3g`、`4g`、`wifi`)                                                  | string |
| preview_url      | The preview url of the offer                                                                                        | string |
| icon_url         | The link to the icon of the offer                                                                                   | string |
| creatives        | The image materials of the offer                                                                                    | array  |
| category         | The category of the offer                                                                                           | string |
| store_label      | The store (AppStore/GooglePlay) label of the offer                                                                  | array  |
| store_rating     | The store (AppStore/GooglePlay) rating of the offer                                                                 | string  |
| size             | The size of the package                                                                                             | string |
| mandatory_device | The mandatory device that offer needs to finish a conversion .`true` means mandatory                                | array  |

#### Notice
- "cap" will be 0 if there is no limit.
- "countries" will be an empty array if the offer is global.

#### Example

```javascript
{
    "c": 0,
    "offers": [
        {
            "id": "783709938932256768",
            "name": "Clean Master (Boost & AppLock)",
            "payout": 0.13,
            "point": 0,
            "cap": 0,
            "preview_url": "https://play.google.com/store/apps/details?id=com.cleanmaster.mguard",
            "countries": [
                "UY"
            ],
            "store_label": [
                "Tools"
            ],
            "store_rating": "4.7",
            "os": "android",
            "task": "Install the application and play for a few minutes",
            "traffic": "non-incentive",
            "os_version": "",
            "carrier": "",
            "nettype": "",
            "creatives": [
                {
                    "mime": "image/jpeg",
                    "width": "307",
                    "height": "512",
                    "url": "http://img1.ymcdn.net/creative/1512/17/d87e19d19480ec6d-unnamed.jpg"
                },
                {
                    "height": "250",
                    "url": "http://img.ymcdn.net/creative/1511/05/34e2ee43c42997c8-1.jpg",
                    "mime": "image/jpeg",
                    "width": "300"
                },
                {
                    "url": "http://img1.ymcdn.net/creative/1511/05/94a9e59bd9ce8249-2.jpg",
                    "mime": "image/jpeg",
                    "width": "320",
                    "height": "480"
                },
                {
                    "mime": "image/jpeg",
                    "width": "300",
                    "height": "300",
                    "url": "http://img.ymcdn.net/creative/1511/05/0d469de1aade077e-3.jpg"
                },
                {
                    "width": "1200",
                    "height": "627",
                    "url": "http://img1.ymcdn.net/creative/1511/05/3e7ab4c71232015a-4.jpg",
                    "mime": "image/jpeg"
                },
                {
                    "url": "http://img.ymcdn.net/creative/1602/05/c55d7d495777c680-unnamed.jpg",
                    "mime": "image/jpeg",
                    "width": "512",
                    "height": "250"
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
            "icon_url": "http://img.ymcdn.net/creative/1512/17/02c8a93f16e522a4-unnamed.jpg",
            "adtxt": " The World&#39;s Most Trusted Android Optimizer, Speed Booster, Battery Saver and Free Anti-Virus app, Clean Master Helps Accelerate and Clean Up Over 600 Million Phones! It Also Provides Real-time Protection With the #1 Antivirus Engine, and Secures Your Private Data With the AppLock Function.   Have you ever encounter the following problems?  Your device has become laggy and freezes all the time You don&#39;t have enough space to take more pictures or install apps Your battery has started draining quicker than ever Your device overheats and needs to cool down You want to lock your photos, gallery and messages from prying eyes and nosy friends  Features   ► AppLock  AppLock can lock Facebook, SMS, Contacts, Gallery, or any other apps you choose. With AppLock, only you can see the photos you protect. Guarding your privacy is easier than ever!  ► Battery Saver  Battery Saver help to analyze battery status and hiberate running apps to save power. With Battery Saver, you can stop apps that waste lots of power an",
            "package": "com.cleanmaster.mguard",
            "category": "SDL",
            "trackinglink": "http://ad.api.yyapi.net/v1/tracking?ad=783709938932256768&app_id=b3a3277b8fdd54bc&pid=3&user_id={user_id}"
        }
    ],
    "total": 1,
    "page": 1,
    "page_size": 20,
    "n": 1
}
```
### Offer Category

| category | Description                                                    |
|----------|----------------------------------------------------------------|
| SDL      | The app that is listed publicly on store(AppStore/GooglePlay). |
| WEB      | Redirect to a website, then play web games or submit a form.   |
| APK      | Download an APK and install direct.                            |
| DDL      | Redirect to a website to download app direct.                  |

### Tracking Link

| Parameter | Description                                                                                                                                                                            |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| user_id   | Identify the user who complete a task. The server callback can return this value when user complete a task.                                                                            |
| imei      | The IMEI of user's device.If imei is mandatory in "mandatory_device"  parameter,publisher must pass it in  trackinglink.If not,imei will not appear in trackinglink.                   |
| mac       | The Mac Address of user's device.If mac is mandatory in "mandatory_device"  parameter,publisher must pass it in trackinglink.If not,mac will not appear in trackinglink.               |
| andid     | The Android ID of user's device.If androidid is mandatory in "mandatory_device"  parameter,publisher must pass it in trackinglink.If not,android will not appear in trackinglink.      |
| advid     | The Google Advertising ID of user's device.If advid is mandatory in "mandatory_device"  parameter,publisher must pass it in trackinglink.If not,advid will not appear in trackinglink. |
| idfa      | The IDFA on IOS of user's device.If idfa is mandatory in "mandatory_device"  parameter,publisher must pass it in trackinglink.If not,idfa will not appear in trackinglink.             |
| udid      | The udid of user's device.If udid is mandatory in "mandatory_device"  parameter,publisher must pass it in trackinglink.If not,idfa will not appear in trackinglink.                    |
| chn       | Identify the channel which completed a task.(less than 150 chars)                                                                                                                                           |

#### Example

[http://ad.api.yyapi.net/v1/tracking?ad=782801020756430848&app_id=4b84c1788615000d&pid=3&user_id=abc123&chn=abc456](http://ad.api.yyapi.net/v1/tracking?ad=782801020756430848&app_id=4b84c1788615000d&pid=3&user_id=abc123&chn=abc456)

## Common Error Response

| Key         | Description                                                                                                                                               |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| {"c":-1002} | app_id not exists                                                                                                                                         |
| {"c":-1004} | app not match device_type                                                                                                                                 |
| {"c":-1202} | app didn't pass the verification                                                                                                                          |
| {"c":-1300} | app_secret not match app_id                                                                                                                               |
| {"c":-1403} | Application didn't pass the verification or publisher didn't turn on the "Live" button for the application in "ADs Settings" - "Ad Units" of ADXMI Panel. |
| {"c":-2103} | offer not exists                                                                                                                                          |
| {"c":-2221} | offer not running                                                                                                                                         |
| {"c":-3003} | missing required parameters                                                                                                                               |
| {"c":-3006} | missing required device parameters                                                                                                                               |
| {"c":-3212} | country mismatch                                                                                                                                          |
| {"c":-3302} | exceed qps limit                                                                                                                                          |

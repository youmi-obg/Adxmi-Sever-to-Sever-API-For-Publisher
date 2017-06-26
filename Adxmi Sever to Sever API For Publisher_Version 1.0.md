# Adxmi Sever to Sever API For Publisher_Version 2.0
****



## To get start
#### Step1: Adxmi Publisher Account
The API is supported only for partners who have Adxmi account. Please register on our website https://www.adxmi.com/account/signup before integrating Adxmi API.
#### Step2: Adxmi Offer Access Parameter
Please create an app, get your app_id from app details. You will need to contact your account manager to get your app approved to Adxmi API.
#### Step3:Requesting Adxmi Offers
Publisher can request offers with approved app_id. Your app_id is the identification of your account, please keep it confidential, since that anyone can use it on behalf of you.

## General instructions
1. The API is only accessible by HTTP GET request and returns JSON.

2. All the offers in your feed is active for you, but Adxmi will remove in-active offers from time to time, so please make sure to request every 15 min - 30 min.

3. At most 50 requests in 10 min will be proceeded for every publisher. Once exceeded, you will get response code indicating exceed qps limit.


## API Request
#### API Request url

>  http://ad.api.yyapi.net/v1/offline

#### Request Parameter


| Parameter |  Type   | Mandatory | Example |Description                                  |
|-----------|--------|-----------|-----------|----------------------------------------------|
| app_id    |  string | Y         | app_id=b3a3277b8fdd54bc       |   Applied from www.adxmi.com to access API. |
| page_size | int    | Y         |  page_size=500        |Define the number of offers per page, page_size would be 500 in maxmium in case of request timeout.   |      
| page      | int    | Y         |  page=1    |Define which page to fetch, starting with page 1.     |
| os        |  string | N         | os=android       | Feild: android、ios. Both Android and iOS offers would be responsed if publisher don't set this parameter.          |
| country        |  string | N         | country=US,CN,AU       | Used to get offers of specific countries.  All offers would be responsed if publisher don't set this parameter.          |



##### Example

>http://ad.api.yyapi.net/v1/offline?app_id=b3a3277b8fdd54bc&page=1&page_size=20&os=android

## Response Field
#### Response Parameter

| Parameter        | Description                                                                                                         | Type   |
|------------------|---------------------------------------------------------------------------------------------------------------------|--------|
| id               | The id of the offer                                                                                                 | string |
| name             | The name of the offer                                                                                               | string |
| package          | The package name of the offer                                                                                       | string |
| adtxt            | The introduction of the offer                                                                                       | string |
| task             | The introduction of how to complete the offer                                                                       | string |
| payout           | The revenue($) of the offer                                                                                         | double |
| point            | The amount of virtual currency that users would earn for incent offer(exchange rate is set on www.adxmi.com ) | int    |
| `cap`             | `The maximum conversion allowed of the offer for all publishers` | int    |
| trackinglink     | The link that is used to track conversion                                                                           | string |
| `countries`        | `The target countries of the offer(empty means global)`                                                               | array  |
| `os`              | `The target operating system of the offer(ios or android)`                                                        | string |
| `traffic`          | `The value is incentive or non-incentive`                                                                        | string |
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
| `mandatory_device` | `Publishers need to transmit these device parameters if it's marked as "true". `                              | array  |

#### Notice!!!
- "cap" will be 0 if there is no cap for it.
- "countries" will be an empty array if the offer is targeting for global.
- We suggest publishers transmit GAID/IDFA of all the users as most advertisors require it.
- Clicks in the wrong country,missing mandatory device parameters, wrong trageting os_version, would be blocked by the server, so please make sure to target users that meet the requirements of our offers.

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
                "andid": true,//this means publisher need to transmit android_id
                "advid": true,//this means publisher need to transmit GAID
                "idfa": false,// this means publisher can choose to transmit IDFA or not
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


#### Example

http://ad.api.yyapi.net/v1/tracking?ad=782801020756430848&app_id=4b84c1788615000d&pid=3&user_id=6lnp2meG4jrcC42o0xRKkF9u7jamDCur&chn=adxmi1

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

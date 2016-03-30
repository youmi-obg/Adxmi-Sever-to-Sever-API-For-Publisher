#Adxmi Offer Callback Protocol
###Statement
*	The uniform encoding of interface: UTF-8
*	Once Adxmi gets the feedback data from advertisers, we will report back to developers' server immediately.
*	It will need developers to offer an interface to receive data. The method for interface to receive data: GET.
*	All parameters will process urlencode.
*	After setting callback url in [Developer Control Panel](https://www.adxmi.com/apps#/settings), Adxmi will allocate an individual secret key `callback_token` for signature using.

###Callback Parameters
| Key 		| Value Description |
| :-------- | :---------------- |
| order		| The unique id of the order. If developer receives the same order ID, that means the order is already existed. |
| app		| your application id |
| ad		| The name of the offer |
| adid		| The id of the offer |
| user		| User ID: Developers can set up their own `user_id` In Offers API, then user ID can be used to replace the CID identification which is offered by Adxmi. (Adxmi will generate an identification number for each device) Otherwise, Adxmi will use CID as user ID. |
| revenue	| The revenue($) that developer can earn |
| points    | The currency points that users can earn |
| time		| The time that Adxmi create this order |
| storeid   | Id from application store |
| pkg		| package name of campaign app |
| sign		| Parameters signature, used for verify the integrity of the above parameters, to prevent the third party to tamper with them. |

###Signature Algorithm
	
*	Use all parameters except `sign` in 【Callback Parameters】 list as key to compute MD5 value. Assume that the parameters participate in computing signature are k1,k2,k3, then the signature calculation method is below:

	Formatting the non-binary type request parameters to key=value format. For example: k1=v1,k2=v2,k3=v3 Sort the key-value in ascending order and connect them together. For example: k1=v1k2=v2k3=v3 Append `callback_token` after the conneted key-value string. MD5 of the above string is the signature value. 

*	`Notice`:
	*	Don't include the sign(signature) parameters when calculating the signature. 
	*	The parameters in signature procedure have not been processed by urlencode. 
	*	In order to ensure the signature won't be abnormal when changing the callback parameters, please make sure using the verification function we offer as the verification method. 
	*	If developer's callback url contains parameters not mentioned above, these parameters will also join in the signature. 

###Verification Function

#####For PHP

```php
function verifySignature($callback_url, $callback_token){
	$sign = null;
	$params = array();
	$url_parse = parse_url($callback_url);
	if (isset($url_parse['query'])){
	    $query_arr = explode('&', $url_parse['query']);
	    if (!empty($query_arr)){
	        foreach($query_arr as $p){
	            if (strpos($p, '=') !== false){
	                list($k, $v) = explode('=', $p);
	                if($k == "sign"){
	                    $sign = $v; 
	                }else{
	                    $params[$k] = urldecode($v);
	                }
	            }
	        }
	    }
	}
	
	$str = '';
	ksort($params);
	foreach ($params as $k => $v) {
	    $str .= "{$k}={$v}";
	}
	$str .= $callback_token;
	
	return $sign == md5($str);
}
```


#####For Java

```java
import java.io.IOException;
import java.io.UnsupportedEncodingException;

import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLDecoder;

import java.security.GeneralSecurityException;
import java.security.MessageDigest;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;


public class AdxmiSign {
    /**
     * Signature Generation Algorithm
     *
     * @param HashMap
     *            <String,String> params Request paramenters set, all parameters
     *            need to convert to string type before this
     * @param String
     *            callbackToken The secret key from Adxmi Developer Control
     *            Panel Setting
     * @return String
     * @throws IOException
     */
    public static String getSignature(HashMap<String, String> params,
        String callbackToken) throws IOException {
        // Sort the parameters in ascending order
        Map<String, String> sortedParams = new TreeMap<String, String>(params);

        Set<Map.Entry<String, String>> entrys = sortedParams.entrySet();

        // Traverse the set after sorting, connect all the parameters as
        // "key=value" format
        StringBuilder basestring = new StringBuilder();

        for (Map.Entry<String, String> param : entrys) {
            basestring.append(param.getKey()).append("=")
                      .append(param.getValue());
        }

        basestring.append(callbackToken);

        // System.out.println(basestring.toString());
        // Calculate signature using MD5
        byte[] bytes = null;

        try {
            MessageDigest md5 = MessageDigest.getInstance("MD5");
            bytes = md5.digest(basestring.toString().getBytes("UTF-8"));
        } catch (GeneralSecurityException ex) {
            throw new IOException(ex);
        }

        // Convert the MD5 output binary result to lowercase hexadecimal result.
        StringBuilder sign = new StringBuilder();

        for (int i = 0; i < bytes.length; i++) {
            String hex = Integer.toHexString(bytes[i] & 0xFF);

            if (hex.length() == 1) {
                sign.append("0");
            }

            sign.append(hex);
        }

        return sign.toString();
    }

    /**
     * Calculate signature with a completed unsigned URL, append the signature
     * at the end of this URL.
     *
     * @param String
     *            url The completed unsigned URL
     * @param String
     *            callbackToken Secret key for calculating signature
     * @return String
     * @throws IOException
     *             , MalformedURLException
     */
    public static String getUrlSignature(String url, String callbackToken)
        throws IOException, MalformedURLException {
        try {
            URL urlObj = new URL(url);
            String query = urlObj.getQuery();
            String[] params = query.split("&");
            Map<String, String> map = new HashMap<String, String>();

            for (String each : params) {
                String name = each.split("=")[0];
                String value;

                try {
                    value = URLDecoder.decode(each.split("=")[1], "UTF-8");
                } catch (UnsupportedEncodingException e) {
                    value = "";
                }

                map.put(name, value);
            }

            String signature = getSignature((HashMap<String, String>) map,
                    callbackToken);

            return url + "&sign=" + signature;
        } catch (MalformedURLException e) {
            throw e;
        } catch (IOException e) {
            throw e;
        }
    }

    /**
     * Check the completed URL with signature parameter. Check the signature is
     * correct or not.
     *
     * @param String
     *            url The completed URL with signature parameter
     * @param String
     *            callbackToken Secret key for calculating signature
     * @return boolean
     */
    public static boolean verifySignature(String signedUrl, String callbackToken) {
        String urlSign = "";

        try {
            URL urlObj = new URL(signedUrl);
            String query = urlObj.getQuery();
            String[] params = query.split("&");
            Map<String, String> map = new HashMap<String, String>();

            for (String each : params) {
                String name = each.split("=")[0];
                String value;

                try {
                    value = URLDecoder.decode(each.split("=")[1], "UTF-8");
                } catch (UnsupportedEncodingException e) {
                    value = "";
                }

                if ("sign".equals(name)) {
                    urlSign = value;
                } else {
                    map.put(name, value);
                }
            }

            if ("".equals(urlSign)) {
                return false;
            } else {
                String signature = getSignature((HashMap<String, String>) map,
                        callbackToken);

                return urlSign.equals(signature);
            }
        } catch (MalformedURLException e) {
            return false;
        } catch (IOException e) {
            return false;
        }
    }
}

```
#####For Python

```python
from urlparse import urlparse
from urllib import unquote 
from hashlib import md5

def verifySinature(callback_url, callback_token):
	sign = None
	params = dict()
	url_parse = urlparse(callback_url)
	query = url_parse.query
	query_array = query.split('&')
	for group in query_array:
	    k, v = group.split('=')
	    if k == 'sign':
	        sign = v
	    else:
	        params[k] = unquote(v)
	
	str = ''
	sorted_params = sorted(params.items(), key = lambda d:d[0])
	for k, v in sorted_params:
	    str += '%s=%s' % (k, v)
	str += callback_token
	
	m = md5()
	m.update(str)
	return sign == m.hexdigest()

```


###Return Value
*	Adxmi will proceed the next operation according to the returning http status code from developers' server. The normal http status code should be 200 or 403.
*	If the http status code is 200, that means developers already received and processed the message normally.
*	If the http status code is 403, that means developers refuse this request, which also means middle-tier server will not repeatedly make request to developers' server.
*	If timeout, or the http status code isn't one of 2xx, 301, 302, 303, 307, 400 and 403, the middle-tier server will make request to developers' server again in the next cycle.
*	There will be delays in the next cycle request to developers' server, the delay time will respectively be: 5s, 10s, 60s, 300s, 600s, 3600s(since last request). Which means, in the worst case, Adxmi will send seven *	requests. If all of the seven requests fail, the link will be discarded.
*	Because of network issues or other reasons, developers' server will receive multiple records with the same order ID. In this case, developers' server need to abandon the duplicate records, and output with http status code 403.

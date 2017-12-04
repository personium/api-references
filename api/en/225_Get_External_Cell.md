# ExtCell Acquire

### Overview

acquire existed ExtCell

### Required Privileges

auth-read

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


### Request

#### Request URL

```
/{CellName}/__ctl/ExtCell('http%3A%2F%2F{UnitFQDN}%2F{CellName}')
```

#### Request Method

GET

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

[$select  Query](406_Select_Query.html)

[$expand  Query](405_Expand_Query.html)

[$format  Query](404_Format_Query.html)

#### Request Header

|Header Name|Summary|Valid Value|Required|Remarks|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

200

#### Response Header

None

#### Response Body

|Object|Item Name|Data Type|Remarks|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

##### ExtCell specific response body

|Object|Item Name|Data Type|Remarks|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ExtCell|
|{2}|Url|string|URL of target Cell|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https://{UnitFQDN}/{ExtCellName}/')",
        "etag": "W/\"1-1486519006899\"",
        "type": "CellCtl.ExtCell"
      },
      "Url": "https://{UnitFQDN}/{ExtCellName}/",
      "__published": "/Date(1486519006899)/",
      "__updated": "/Date(1486519006899)/",
      "_Role": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https://{UnitFQDN}/{ExtCellName}/')/_Role"
        }
      },
      "_Relation": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https://{UnitFQDN}/{ExtCellName}/')/_Relation"
        }
      }
    }
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https%3A%2F%2F{UnitFQDN}%2F{CellName}%2F')" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED
# Get service collection source

### Overview

Acquire service source information

### Required Privileges

read

### Restrictions

* General Restrictions
    * None

* WebDAV Restrictions
    * Unpublished<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{CollectionName}/__src/{ResourceName}
```

|Path|Overview|Notes|
|:--|:--|:--|
|{CellName}|Cell Name||
|{BoxName}|Box Name||
|{CollectionName}|Service Collection Name|Valid values <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)|
|{ResourceName}|Resource name|Valid values(limit) <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)|

#### Request Method

GET

#### Request Query

##### Common Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|Supported in V 1.1.7 and later|

##### Service Collection Source Acquisition Specific Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|If-None-Match|Specify the value of ETag, if there is no change, 304, if it is changed return the latest resource|String|No||

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

200

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Resource's Content-Type||
|ETag|Resource version information||

#### Response Body

Return the contents of the file

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/__src/{ResourceName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED

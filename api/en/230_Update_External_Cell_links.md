# Extcell $links update

### Overview

Update OData resources associated with ExternalCell<br>You can specify the following OData resources

* Box
* ExtCell
* ExtRole
* Role

### Required Privileges

None

### OData Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


### Request

#### Request URL

##### $links with ExtCell

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtCell('{ExtCellURL}')
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtCell('{ExtCellURL}')
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtCell('{ExtCellURL}')
```

#### Request Method

PUT

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value} override} $: $ {value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Contents-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

#### Request Body

#### format

JSON

#### Description

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Url|URL to Cell|Number of digits: 1-1024<br>Follow URI format<br>scheme:http, https<br>Trailing slash (URL terminated /) Required|Yes||

#### Request Sample

```JSON
{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')"}
```


### Response

#### Response Code

204

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|Content-Type|Format of data to be returned||
|DataServiceVersion|OData version||

#### OData Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|DataServiceVersion|OData version||
|ETag|Resource version information||

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Box('{BoxName}')" -X PUT -i -H 'If-Match:*' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Box('update_{BoxName}')"}'
```


###### Copyright 2017 FUJITSU LIMITED

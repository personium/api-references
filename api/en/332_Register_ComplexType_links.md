# ComplexType $links registered

### Overview

Link OData resource specified by $links to ComplexType  
Get a list of OData resources linked with following

* Property

### Required Privileges

alter-schema

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

```
$links with Property
/{CellName}/{BoxName}/{CollectionName}/$metadata/ComplexType('{ComplextypeName}')/$links/_Property
```

#### Request Method

POST

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|Supported in V 1.1.7 and later|

##### OData Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

##### OData Create Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

##### Format

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|uri|URI of the OData resource to be linked|Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn|Yes||

#### Request Sample

```JSON
{"uri":"https://{UnitFQDN}/Cell/__ctl/Box('{BoxName}')"}
```


### Response

#### Response Code

204

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|

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

EntityType

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='{EntityTypeName}')/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='assocName_link',_EntityType.Name=null)"}'
```

AssociationEnd

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}2',_EntityType.Name=Entity)/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}_link',_EntityType.Name=Entity2)"}'
```


###### Copyright 2017 FUJITSU LIMITED

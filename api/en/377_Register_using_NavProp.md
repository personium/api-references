# Entity NavProp registration

### Overview

Create entity Entity via Navigation Property.

### Required Privileges

write

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* User data restrictions
    * Property scope of Edm.DateTime type is not properly checked
    * Array of Edm.DateTime type is not supported
    * If SYSUTCDATETIME () is specified as the property of Edm.DateTime type, the set system time may be different
    * When setting in request body and setting with DefaultValue (\_\_published, \_\_ updated is the latter timing)
    * For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty


### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}
```

|Path|Overview|
|:--|:--|
|{CellName}|Cell Name|
|{BoxName}|Box Name|
|{ODataCollecitonName}|Collection Name|
|{EntityTypeName}|EntityType name|
|{EntityID}|EntityID|
|{NavigationPropertyName}|NavigationProperty name|

NavigationProperty name that can be specified is limited to those having the following relation with EntitySet.

|From|To|
|:--|:--|
|0 .. 1|1|
|0 .. 1|*|
|1|1|
|1|*|
|*|*|

#### Request Method

POST<br>

#### Request Query

##### Common Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>Not tested|
|Accept|Specifies the response body format|application/json|No|[application/json] by default<br>Not compatible|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default<br>Not compatible|

#### Request Body

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|__id|EntityID|Number of digits: 1-200<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")|No|If not specified a unique ID will be assigned<br>Valid value check unimplemented|

* \_It is possible to register dynamic user data in addition to \_id
    * Only character string can be set
    * If a numeric value, boolean value, null is specified, it is treated as a character string
    * Can not specify Hash value
    * Up to 400 user data can be specified
    * \_published, \_updated is a reserved word, so it is not possible to specify the request body

#### Request Sample

```JSON
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt": "","outcome": "During treatment"}
```


### Response

#### Response Code

201

#### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Location|Resource URL of created Entity|Return only when Entity can be created successfully|
|DataServiceVersion|OData version information|Return only when Entity can be created successfully|
|ETag|Resource version information|Return only when Entity can be created successfully|

#### Response Body

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value is as follows

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{3}|type|string|EntityType name|
|{2}|__id|string|EntityID(__id)|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{2}|{NP name}|string|Object{4}<br>It is returned only when Link is connected. {NP name}: NavigationPropert name|
|{4}|__deferred|object|Object{5}|
|{5}|uri|string|uri of the resource that has the relationship<br>Not tested|
|{1}|__count|string|Get number of results in $inlinecount query|

Return items that were schema-set other than the above, or dynamic items specified at registration

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('100-1_20101108-111352093')",
        "etag": "W/\"1-1487929403469\"",
        "type": "UserData.{EntityTypeName}"
      },
      "__id": "100-1_20101108-111352093",
      "__published": "/Date(1487929403469)/",
      "__updated": "/Date(1487929403469)/",
      "PetName": null,
      "{NavigationPropertyName}": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('100-1_20101108-111352093')/{NavigationPropertyName}"
        }
      }
    }
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"__id": "100-1_20101108-111352093"}'
```


###### Copyright 2017 FUJITSU LIMITED

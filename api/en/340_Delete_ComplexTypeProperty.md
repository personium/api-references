# ComplexTypeProperty Delete

### Overview

Delete existing ComplexTypeProperty information

### Required Privileges

alter-schema

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* If atom or xml is specified as the $format query option, it will not be an error, but there is no guarantee of the data of the response body
    * Deletion will not be executed if the relationship setting exists in the deletion target.
    * When deleting ComplexTypeProperty, it can be deleted only when the user data of the EntityType using the ComplexTypeProperty to be deleted does not exist

        ```
        Example)
        EntityTypeA
           |
           + PropertyA Type-String
           + PropertyB Type-ComplexTypeA
              |
              + ComplexTypePropertyA
        When deleting ComplexTypeProperty A, it is possible only when EntityTypeA data does not exist
        ```


### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty(Name='{ComplexTypePropertyName}',_ComplexType.Name='{ComplexTypeName}')
```

#### Request Method

DELETE

#### Request Query

##### Common Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

##### OData Common Request Query

None

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

##### ODataDelete request header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

204

#### Response Header

None

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/ComplexTypeProperty(Name='{ComplexTypePropertyName}',_ComplexType.Name='{ComplexTypeName}')" -X DELETE -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED

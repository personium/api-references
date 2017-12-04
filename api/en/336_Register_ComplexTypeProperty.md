# ComplexTypeProperty Registration

### Overview

Define the ComplexTypeProperty specified for user data

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* Individual restrictions

    * When user data of EntityType using the target ComplexType exists, it can be registered only when Nullable is true
    * Registerable only when ComplexType hierarchy number is within 5 hierarchies
    * Validity check of DefaultValue of Edm.DateTime is not properly done

* Restrictions stipulated as Personium specifications

    * For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty
    * Maximum number of registered types of ComplexTypeProperty of each hierarchy can be registered up to the following number of SimpleType
    * 2nd level: 50 pieces
    * 3rd level: 30 pieces
    * 4th level: 10 pieces
    * The maximum number of registered types of ComplexTypeProperty of each hierarchy can be registered up to the following number
    * 1st level: 20 pieces
    * 2nd level: 5 pieces
    * 3rd level: 2 pieces
    * 4th level: 0 pieces

### Required Privileges

alter-schema


### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty
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
|Content-Type|Specifies the request body format|application/json|No|When omitted, treat it as [application/json]|
|Accept|Specify the format of the response body|application/json|No|When omitted, treat it as [application/json]|

#### Request Body

##### Format

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Name|ComplexTypeProperty name|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")|Yes||
|_ComplexType.Name|ComplexType name attached|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")|Yes||
|Type|Type definition|Edm.Boolean / Edm.String / Edm.Int32 / Edm.Single / Edm.Double / Edm.DateTime / Registered ComplexType name|Yes||
|Nullable|Null value authorization|true / false<br>The default value is Null|No||
|DefaultValue|Default value|See the table below<br>The default value is Null|No||
|CollectionKind|Array type|None / List<br>The default value is "None"|No||

##### Valid values for DefaultValue

Valid values of DefaultValue differ depending on Type value (type definition), and also define items with different types of the following definitions with character strings

|Type Value|Effective Value|
|:--|:--|
|Edm.Boolean|true / false|
|Edm.String|Number of digits: 0-51200 byte<br>When "\\" is used, it must be specified with "\\\\"|
|Edm.Int32|-2147483648 - 2147483647|
|Edm.Single|Number of digits in integer part: 1-5 digits<br>Number of digits in decimal part: 1-5 digits|
|Edm.Double|Represents a floating point number with 15 digits precision.|
|Edm.DateTime|It is specified as a character string in the format of Date ([time of long type])<br> The valid value of [time of long type] is -6847804800000(1753-01-01T00:00:00.000Z)-253402300799999(9999-12-31T23:59:59.999Z)<br>In addition, you can specify the following as reserved words<br> SYSUTCDATETIME (): server time|

#### Request Sample

```JSON
{"Name": "{ComplexTypePropertyName}","_ComplexType.Name": "{ComplexTypeName}","Type": "Edm.String","Nullable": true,"DefaultValue": null,"CollectionKind": "None"}
```


### Response

#### Response Code

201

#### Response Header

##### Common Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

##### OData Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Location|URL to the resource that was created||
|DataServiceVersion|OData version||
|ETag|Resource version information||

#### Response Body

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

##### Property specific response body

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|{3}|type|string|ODataSvcSchema.ComplexTypeProperty|
|{2}|Name|string|ComplexTypeProperty name|
|{2}|Type|string|Type definition|
|{2}|Nullable|boolean|Null value authorization|
|{2}|DefaultValue|string|Default value|
|{2}|CollectionKind|string|Array type|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty(Name='{ComplexTypePropertyName}',_ComplexType.Name='{ComplexTypeName}')",
        "etag": "W/\"1-1487658277593\"",
        "type": "ODataSvcSchema.ComplexTypeProperty"
      },
      "Name": "{ComplexTypePropertyName}",
      "_ComplexType.Name": "{ComplexTypeName}",
      "Type": "Edm.String",
      "Nullable": true,
      "DefaultValue": null,
      "CollectionKind": "None",
      "__published": "/Date(1487658277593)/",
      "__updated": "/Date(1487658277593)/"
    }
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/ComplexTypeProperty" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name": "{ComplexTypePropertyName}","_ComplexType.Name": "{ComplexTypeName}","Type": "Edm.String","Nullable": true,"DefaultValue": null,"CollectionKind": "None"}'
```


###### Copyright 2017 FUJITSU LIMITED

# Create other objects via Role's Navigation Property

## Overview

Register via the Cell control object Navigation Property and register $links at the same time.

### Required Privileges

write

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


## Request

### Request URL

#### NavigationProperty to Account

```
{CellURL}__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_Account
```

or

```
{CellURL}__ctl/Role(Name='{RoleName}')/_Account
```

or

```
{CellURL}__ctl/Role('{RoleName}')/_Account
```

#### NavigationProperty to ExCel

```
{CellURL}__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_ExtCell
```

or

```
{CellURL}__ctl/Role(Name='{RoleName}')/_ExtCell
```

or

```
{CellURL}__ctl/Role('{RoleName}')/_ExtCell
```

#### NavigationProperty to ExtRole

```
{CellURL}__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_ExtRole
```

or

```
{CellURL}__ctl/Role(Name='{RoleName}')/_ExtRole
```

or

```
{CellURL}__ctl/Role('{RoleName}')/_ExtRole
```

#### NavigationProperty to Relation

```
{CellURL}__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_Relation
```

or

```
{CellURL}__ctl/Role(Name='{RoleName}')/_Relation
```

or

```
{CellURL}__ctl/Role('{RoleName}')/_Relation
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

### Request Method

POST

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${32 character string with UUID} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

### Request Body

#### When registering Role

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Name|Role Name|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")|Yes||
|_Box.Name|Box name to be related|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>Description: Specify Name of Box registered with Box registration API <br>Specify null if not associated with specific Box|No||

### Request Sample

```JSON
{
  "Name": "role1",
  "_Box.Name": "box1"
}
```


## Response

### Response Code

201

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Location|URL to the resource that was created||
|DataServiceVersion|OData version||
|ETag|Resource version information||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

### Response Body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

#### When registered Role

#### Role specific response body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Role|
|{2}|Name|string|Role Name|
|{2}|_Box.Name|string|Box name to be related|

### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')",
        "etag": "W/\"1-1486515294719\"",
        "type": "CellCtl.Role"
      },
      "Name": "role1",
      "__published": "/Date(1486515294719)/",
      "__updated": "/Date(1486515294719)/",
      "_Box.Name": "box1"
    }
  }
}
```

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)


## cURL Command

#### Relation registration via Navigation Property

```sh
curl "https://cell1.unit1.example/__ctl/Role('role1')/_Relation" -X POST -i \
-H 'Authorization: Bearer AA~PBDc...(snip)...FrTjA' \
-H 'Accept: application/json' -d '{"Name":"relation1"}'
```



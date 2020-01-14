# Retrieve Role

## Overview

Obtain existing Role information

### Required Privileges
auth-read
### Restrictions

* OData Restrictions
    * Accept in the request header is ignored
    * Always handles Content-Type in the request header as application/json
    * Only accepts the request body in the JSON format
    * Only application/json is supported for Content-Type in the request header and the JSON format for the response body
    * $formatQuery options ignored

## Request

### Request URL

```
{CellURL}__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')
```

or

```
{CellURL}__ctl/Role(Name='{RoleName}')
```

or

```
{CellURL}__ctl/Role('{RoleName}')
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

### Request Method

GET

### Request Query

The following query parameters are available

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

[$select  Query](406_Select_Query.md)

[$expand  Query](405_Expand_Query.md)

[$format  Query](404_Format_Query.md)

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|When not specified, default value given with ${4 digits}_${22 digits} Base64url characters format representing an UUID for each request|

### OData Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

### OData Create Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

### Request Body

None


## Response

### Response Code

200

### Response Header

None

### Response Body

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object|Item Name|Type|Notes|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

### Role specific response body

|Object|Item Name|Type|Notes|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Role|
|{2}|Name|string|Role Name|
|{2}|_Box.Name|string|Box name to be related|

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')",
        "etag": "W/\"1-1486349783744\"",
        "type": "CellCtl.Role"
      },
      "Name": "role1",
      "_Box.Name": "box1",
      "__published": "/Date(1486349783744)/",
      "__updated": "/Date(1486349783744)/",
      "_Box": {
        "__deferred": {
          "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')
/_Box"
        }
      },
      "_Account": {
        "__deferred": {
          "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')
/_Account"
        }
      },
      "_ExtCell": {
        "__deferred": {
          "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')
/_ExtCell"
        }
      },
      "_ExtRole": {
        "__deferred": {
          "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')
/_ExtRole"
        }
      },
      "_Relation": {
        "__deferred": {
          "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')
/_Relation"
        }
      }
    }
  }
}
```


## cURL Command

```sh
curl "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')" -X GET \
-i -H 'Authorization: Bearer AA~PBDc...(snip)...FrTjA' -H 'Accept: application/json'
```



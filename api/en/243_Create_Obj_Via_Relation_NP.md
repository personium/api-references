# Create other objects via Relation's Navigation Property

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

#### NavigationProperty to Box

```
{CellURL}__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Box
```

or

```
{CellURL}__ctl/Relation(Name='{RelationName}')/_Box
```

or

```
{CellURL}__ctl/Relation('{RelationName}')/_Box
```

#### NavigationProperty to ExCel

```
{CellURL}__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtCell
```

or

```
{CellURL}__ctl/Relation(Name='{RelationName}')/_ExtCell
```

or

```
{CellURL}__ctl/Relation('{RelationName}')/_ExtCell
```

#### NavigationProperty to ExtRole

```
{CellURL}__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtRole
```

or

```
{CellURL}__ctl/Relation(Name='{RelationName}')/_ExtRole
```

or

```
{CellURL}__ctl/Relation('{RelationName}')/_ExtRole
```

#### Navigation Property to Role

```
{CellURL}__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Role
```

or

```
{CellURL}__ctl/Relation(Name='{RelationName}')/_Role
```

or

```
{CellURL}__ctl/Relation('{RelationName}')/_Role
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

#### When registering Box

#### Format

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Name|Relation Name|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_"), plus, :<br>However, the string cannot start with a underscore ("\_") or colon (:)|Yes||
|_Box.Name|Box name to be related|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>Description: Specify Name of Box registered with Box registration API <br>Specify null if not associated with specific Box|No||

#### Request Sample

```JSON
{
  "Name": "relation2",
  "_Box.Name": "box2"
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

#### When registered Box

#### Box specific response body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Box|
|{2}|Name|string|Box Name|
|{2}|Schema|string|Schema Name|

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://cell1.unit1.example/__ctl/Box('box1')",
        "etag": "W/\"1-1486945452485\"",
        "type": "CellCtl.Box"
      },
      "Name": "box1",
      "Schema": null,
      "__published": "/Date(1486945452485)/",
      "__updated": "/Date(1486945452485)/"
    }
  }
}
```

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

## cURL Command

### When registered Box

```sh
curl "https://cell1.unit1.example/__ctl/Relation(Name='relation1',_Box.Name='box1')\
/_Box" -X POST -i -H 'Authorization: Bearer AA~PBDc...(snip)...FrTjA' \
-H 'Accept: application/json' -d '{"Name":"relation2"}'
```


# Acquisition via Relation\_NavProp

### Overview

Acquire cell control object via Navigation Property

### Required Privileges

* When acquire Role<br>auth-read
* When acquire Relation<br>social-read

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


### Request

#### Request URL

##### NavigationProperty to Box

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Box
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_Box
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/_Box
```

##### NavigationProperty to ExCel

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtCell
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_ExtCell
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/_ExtCell
```

##### NavigationPropert to ExtRole

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtRole
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_ExtRole
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/_ExtRole
```

##### navigationProperty to Role

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Role
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_Role
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/_Role
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

#### Request Method

GET

#### Request Query

The following query parameters are available

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

[$select  Query](406_Select_Query.html)

[$expand  Query](405_Expand_Query.html)

[$format  Query](404_Format_Query.html)

[$filter  Query](403_Filter_Query.html)

[$inlinecount  Query](407_Inlinecount_Query.html)

[$orderby  Query](400_Orderby_Query.html)

[$top  Query](401_Top_Query.html)

[$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value};override} $: $ {value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

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
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|Content-Type|Format of data to be returned||
|DataServiceVersion|OData version||

#### Response Body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{2}|__metadata|object|Object{3}|
|{3}|etag|string|Etag value|
|{3}|uri|string|URL to the resource that was created|
|{1}|__count|string|Get number of results in $inlinecount query|

##### When acquiring Relation

##### Relation specific response body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Relation|
|{3}|Name|string|Relation Name|
|{2}|_Box.Name|string|Box name to be related|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

##### Response Sample

```JSON
{
  "d": {
    "results": {
      "Name": "{RelationName}",
      "_Box.Name": "{BoxName}",
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')",
        "type": "CellCtl.relation"
      },
      "__published" : "\/Date(1339128525502)\/",
      "__updated"   : "\/Date(1339128525502)\/"
    }
  }
}
```


### cURL Command

None

###### Copyright 2017 FUJITSU LIMITED

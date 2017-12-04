# Registration via Account\_NavProp

### Overview

register Role via Account Navigation Property

### Required Privileges

write

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

##### navigationProperty to Role

```
/{CellName}/__ctl/Account(Name='{AccountName}')/_Role
```

or

```
/{CellName}/__ctl/Account('{AccountName}')/_Role
```

#### Request Method

POST

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

When registering Role

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Name|Account Name|Character type: Half size alphanumeric characters and following half-width symbol (-_!$*=^`{&#124;}~.@) <br>However, the first character can not be specified as the first character|Yes||

#### Request Sample

```JSON
{
  "Name": "{AccountName}"
}
```


### Response

#### Response Code

201

#### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|Content-Type|Format of data to be returned||
|Location|URL to the resource that was created||
|ETag|Resource version information||
|DataServiceVersion|OData version||

#### Response Body

|Item Name|Overview|Notes|
|:--|:--|:--|
|d|||
|d / results|||
|d / results / __published|Created date||
|d / results / __updated|Updated date||
|d / results / __metadata|||
|d / results / __metadata / etag|ETag value||
|d / results / __metadata / uri|URL to the resource that was created||
|d / results / __metadata / type|EntityType||
|d / results / Name|Role Name||
|d / results / _Box.Name|Box name to be related||

##### When registered Account

Account specific response body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|{2}|Name|string|Account name|
|{2}|Cell|string|null|
|{2}|Type|string|basic|
|{3}|Type|string|CellCtl.Account|

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "Name": "{AccountName}",
      "__published": "/Date(1349355810698)/",
      "Cell": null,
      "__updated": "/Date(1349355810698)/",
      "Type": "basic",
      "__metadata": {
        "etag": "1-1349355810698",
        "type": "CellCtl.Account",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')"
      }
    }
  }
}   
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

##### Register via Account and Role navigationProperty

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('acount_name')/_Role" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{RoleName}"}'
```


###### Copyright 2017 FUJITSU LIMITED

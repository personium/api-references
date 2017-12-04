# Change Properties

### Overview

change Cell properties

### Required Privileges

Only unit users permitted

### Restrictions

Unpublished


### Request

#### Request URL

```
/{CellName}
```

#### Request Method

PROPPATCH

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specify content format|application / xml|No||
|Accept|Specify acceptable media types in response|application / xml|No||

#### Request Body

|Item Name|Namespace|Overview|Required|Effective Value|Notes|
|:--|:--|:--|:--|:--|:--|
|DAV:||XML namespace setting|Yes|"DAV:"||
|urn: x-personium: xmlns||XML namespace setting|Yes|"Urn: x-personium: xmlns"||
|propertyupdate|DAV:|propertyupdate (Access Control List) root|Yes|<ELEMENT propertyupdate! (Set &#124; remove)>||
|set|DAV:|set property|No|<! ELEMENT set (prop *)>||
|remove|DAV:|remove property|No|<! ELEMENT set (prop *)>||
|prop|DAV:|remove property value|No|<! ELEMENT prop ANY>|Delete using the XML tag specified as ANY as a key|
|prop|DAV:|set property value|No|<! ELEMENT prop ANY>|The XML tag specified as ANY is the key|

#### Request Sample

```xml
<D:propertyupdate xmlns:D="DAV:"
    xmlns:p="urn:x-personium:xmlns">
    <D:set>
        <D:prop>
            <p:hoge>fuga</p:hoge>
        </D:prop>
    </D:set>
    <D:remove>
        <D:prop>
            <p:hoge/>
        </D:prop>
    </D:remove>
</D:propertyupdate>
```


### Response

#### Response Code

|Code|Message|Overview|
|:--|:--|:--|
|207|MULTI_STATUS|Success|

#### Response Header

None

#### Response Body

|Item Name|Namespace|Overview|Notes|
|:--|:--|:--|:--|
|multistatus|DAV:|Response Body route|<! ELEMENT multistatus (response *)>|
|response|DAV:|response route|<! ELEMENT response (href, propstat)>|
|href|DAV:|URL of the resource that executed PROPPATCH|URL of the resource that executed PROPPATCH|
|propstat|DAV:|rproperty setting result|<! ELEMENT propstat (prop, status)>|
|prop|DAV:|property setting contents|display resource setting result as following <br>success: setting key and value<br>delete success: deleted key|
|status|DAV:|Property setting status code|In the case of setting success 200 (OK) is returned|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/</href>
        <propstat>
            <prop>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:">${hoge}</p:hoge>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}" -X PROPPATCH -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?>
<D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop>
<p:hoge>${hoge}</p:hoge></D:prop></D:set><D:remove><D:prop><p:hoge/></D:prop></D:remove></D:propertyupdate>'
```


###### Copyright 2017 FUJITSU LIMITED

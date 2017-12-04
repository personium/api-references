# Cross-Domain Policy File

### Overview

Gets the (crossdomain.xml) cross-domain policy file.  
You can get the Cross-domain policy file of all users.

### Required Privileges

None

### Restrictions

None


### Request

#### Request URL

```
/crossdomain.xml
```

#### Request Method

GET

#### Request Query

None

#### Request Header

None

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

200

#### Response Header

None

#### Response Body

XML

|Item Name|Overview|
|:--|:--|
|/cross-domain-policy/site-control|Permission settings of meta-information policy. Return the "permitted-cross-domain-policies =" all "" attribute value is fixed|
|/cross-domain-policy/allow-access-from|Domain can be accessed in the current domain. Return the "domain =" * "" attribute value is fixed|
|/cross-domain-policy/allow-http-request-headers-from|And header information that can be sent, the source domain to the domain of the current header. Return the "domain =" * "headers =" * "" attribute value is fixed|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl "https://{UnitFQDN}/crossdomain.xml" -X GET -i
```


###### Copyright 2017 FUJITSU LIMITED

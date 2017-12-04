# $format Query

### Overview

When a $format query is specified, the response is returned with the media type specified by the query option.  
Query options for $format Valid values are shown in the table below.

### Request Query

|$format Option|Format of response data|
|:--|:--|
|atom|application/atom+xml|
|xml|application/xml|
|JSON|application/json|
|Other IANA-defined content formats|IANA definition content format|
|A service-specific value representing a format unique to a certain OData service|IANA definition content format|

### cURL Command

Example: When acquiring the cell list in JSON format:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$format=JSON" -X GET -i -H 'Authorization: Bearer {AccessToken}'
```


###### Copyright 2017 FUJITSU LIMITED

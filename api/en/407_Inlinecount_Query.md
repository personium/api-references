# $inlinecount  Query

### Overview

Use the $inlinecount query to specify the number of retrieved results when retrieving a list  
If omitted, do not include the number of acquisition results in response

### Effective Value

|Value|Overview|Example|Notes|
|:--|:--|:--|:--|
|allpages|Include number of results obtained|$inlinecount=allpages||
|none|Do not include the number of acquisition results|$inlinecount=none||

### cURL Command

Example: Obtaining the cell list When acquiring the cell count including the number of results:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$inlinecount=allpages" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED

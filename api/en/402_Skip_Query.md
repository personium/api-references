# $skip Query

### Overview

The $skip query is used to acquire information from the N + 1 th subject by omitting the number of natural numbers N specified in the collection.  
It is described with the following notation.

```
$skip={number}
```

### cURL Command

Example: When acquiring information from the 11th cell by omitting acquisition of 10 cells:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$skip=10" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED

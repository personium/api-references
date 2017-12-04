# $inlinecount クエリ
### 概要
一覧取得時に取得結果件数を指定する場合は$inlinecountクエリを使用する  
省略した場合は、レスポンスに取得結果件数を含まない
### 有効値
|Value|概要|Example|備考|
|:--|:--|:--|:--|
|allpages|取得結果件数を含める|$inlinecount=allpages||
|none|取得結果件数を含めない|$inlinecount=none||
### cURLサンプル
例：セル一覧を取得結果件数を含め取得する場合:
```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$inlinecount=allpages" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED

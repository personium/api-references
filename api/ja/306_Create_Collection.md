# コレクション作成
### 概要
コレクションを作成する
### 必要な権限
write
### 制限事項
共通制限
* なし

WebDAV制限
* 未稿


### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{CollectionName}
```
|パス|概要|備考|
|:--|:--|:--|
|{CellName}|セル名||
|{BoxName}|ボックス名||
|{CollectionName}|コレクション名|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)|
#### メソッド
MKCOL
#### リクエストクエリ
|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
##### 個別リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
#### リクエストボディ
##### 名前空間
|URI|概要|参考prefix|
|:--|:--|:--|
|DAV:|WebDAVの名前空間|D:|
|urn:x-personium:xmlns|Personiumの名前空間|p:|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名|名前空間|ノードタイプ|概要|備考|
|:--|:--|:--|:--|:--|
|mkcol|D:|要素|mkcolのルート要素を表し、setが子となる||
|set|D:|要素|プロパティ設定を表し、propが子となる||
|prop|D:|要素|プロパティ設定値を表し、resourcetypeが子となる||
|resourcetype|D:|要素|リソースタイプ設定を表し、collection・odata・serviceのいずれかが子となる||
|collection|D:|要素|コレクションを表す|collectionノードのみ指定されている場合WebDAVコレクション作成となる|
|odata|p:|要素|ODataコレクションを表す|collectionノードとodataノードが指定されている場合ODataコレクション作成となる|
|service|p:|要素|Serviceコレクションを表す|collectionノードとserviceノードが指定されている場合Serviceコレクション作成となる|
##### DTD表記
##### 名前空間D:
```dtd
<!ELEMENT mkcol (set) >
<!ELEMENT set (prop) >
<!ELEMENT prop (resourcetype) >
<!ELEMENT resourcetype (collection or odata or service) >
<!ELEMENT collection EMPTY>
```
##### 名前空間 p:
```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```
#### リクエストサンプル
WebDAVコレクション作成
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```
ODataコレクション作成
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
        <p:odata/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```
Serviceコレクション作成
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
        <p:service/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```


### レスポンス
#### ステータスコード
201
#### レスポンスヘッダ
##### 共通レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
##### WebDAV共通レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|ETag|リソースのバージョン情報|正常にコレクションが作成できた場合のみ返却する|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし


### cURLサンプル
WebDAVコレクション作成
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/></D:resourcetype></D:prop></D:set></D:mkcol>'
```
ODataコレクション作成
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/><p:odata/></D:resourcetype></D:prop></D:set></D:mkcol>'
```
Serviceコレクション作成
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/><p:service/></D:resourcetype></D:prop></D:set></D:mkcol>'
```

###### Copyright 2017 FUJITSU LIMITED

# コレクション設定変更
### 概要
コレクションの設定を変更する
### 必要な権限
write-properties
### 制限事項
未稿


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
PROPPATCH
#### リクエストクエリ
##### 共通リクエストクエリ
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

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Content-Type|コンテンツ形式を指定する|application/xml|×||
|Accept|レスポンスで受け入れ可能なメディアタイプを指定する|application/xml|×||
#### リクエストボディ
##### 名前空間
|URI|概要|参考prefix|
|:--|:--|:--|
|DAV:|WebDAVの名前空間|D:|
|urn:x-personium:xmlns|Personiumの名前空間|p:|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
|ノード名|名前空間|ノードタイプ|概要|備考|
|:--|:--|:--|:--|:--|
|propertyupdate|D:|要素|propertyupdateのルートを表し、setとremoveが子となる||
|set|D:|要素|プロパティ設定を表し、1つ以上複数のpropが子となる||
|remove|D:|要素|プロパティ削除設定を表し、1つ以上複数のpropが子となる||
|prop|D:|要素|プロパティ値を表し、1つ以上複数の任意の要素が子となる|set時：子のノード名がキーとなる<br>remove時：子のノードを名キーとして削除を行う|
##### DTD表記
```dtd
<!ELEMENT propertyupdate (set, remove) >
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```
#### リクエストサンプル
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


### レスポンス
#### ステータスコード
|コード|メッセージ|概要|
|:--|:--|:--|
|207|Multi-Status|成功|
#### レスポンスヘッダ
未稿
#### レスポンスボディ
##### 名前空間
|URI|概要|参考prefix|
|:--|:--|:--|
|DAV:|WebDAVの名前空間|D:|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名|名前空間|ノードタイプ|概要|備考|
|:--|:--|:--|:--|:--|
|multistatus|D:|要素|multistatusのルートを表し、1つ以上複数のresponseが子となる||
|response|D:|要素|multistatusの内容を表し、hrefとpropstatが子となる||
|href|D:|要素|PROPPATCHを実行したリソースのURL||
|propstat|D:|要素|プロパティ設定結果を表し、propとstatusが子となる||
|prop|D:|要素|プロパティ設定内容を表す|リソース設定の結果を以下のように表示する<br>設定成功：設定したキーと値<br>削除成功：削除したキー|
|status|D:|要素|プロパティ設定ステータスコード|設定成功の場合200(OK)が返る|
##### DTD表記
##### 名前空間 D:
```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (prop, status)>
<!ELEMENT prop ANY>
<!ELEMENT status (#PCDATA)   
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{CellName}/{BoxName}/{CollectionName}</href>
        <propstat>
            <prop>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:">foo</p:hoge>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}' -X PROPPATCH -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?><D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><p:hoge>${hoge}</p:hoge></D:prop></D:set><D:remove><D:prop><p:hoge/></D:prop></D:remove></D:propertyupdate>'
```

###### Copyright 2017 FUJITSU LIMITED

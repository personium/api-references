# Box_$links登録
### 概要
Boxに$linksで指定したODataリソースを紐付ける<Br>以下のODataリソースと紐付けることができる

* Role
* Relation

### 必要な権限
なし

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションは無視される


### リクエスト
#### リクエストURL
##### Roleとの紐付け
```
/{CellName}/__ctl/Box(Name='{BoxName}',Schema='{SchemaURL}')/$links/_Role
```
または、
```
/{CellName}/__ctl/Box(Name='{BoxName}')/$links/_Role
```
または、
```
/{CellName}/__ctl/Box('{BoxName}')/$links/_Role
```
##### Relationとの紐付け
```
/{CellName}/__ctl/Box(Name='{BoxName}',Schema='{SchemaURL}')/$links/_Relation
```
または、
```
/{CellName}/__ctl/Box(Name='{BoxName}')/$links/_Relation
```
または、
```
/{CellName}/__ctl/Box('{BoxName}')/$links/_Relation
```

※ Schemaパラメタを省略した場合は、nullが指定されたものとする

#### メソッド
POST

#### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|

#### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}override} $: $ {value}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
#### リクエストボディ
##### Format
JSON

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|uri|紐付けるODataリソースのURI|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn|○||

#### リクエストサンプル
```JSON
{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}')"}
```

### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|DataServiceVersion|ODataのバージョン||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/\$links/_Role" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d "{\"uri\":\"https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}')\"}"
```

###### Copyright 2017 FUJITSU LIMITED

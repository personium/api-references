# Cell取得
### 概要
既存のCell情報を取得する

### 必要な権限
ユニットユーザのみ可能

### 制限事項
* 共通制限
	* なし
* OData制限
	* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
	* リクエストボディはJSON形式のみ受け付ける
	* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
	* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない


### リクエスト
#### リクエストURL
```
/__ctl/Cell(Name='{CellName}')
```
または、
```
/__ctl/Cell('{CellName}')
```

#### メソッド
GET
#### リクエストクエリ
|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|_

[$select クエリ](406_Select_Query.html)

[$expand クエリ](405_Expand_Query.html)

[$format クエリ](404_Format_Query.html)

#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
##### OData共通リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
##### OData取得リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|If-None-Match|対象ETag値を指定する|ETag値|○|未対応|
#### リクエストボディ
なし
#### リクエストサンプル
なし


### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|DataServiceVersion|ODataのバージョン||
|ETag|リソースのバージョン情報||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|Personium APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ
##### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義される

|オブジェクト|名前（キー）|型|値|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

##### Cell固有レスポンスボディ
|オブジェクト|名前（キー）|型|値|
|:--|:--|:--|:--|
|{3}|type|string|UnitCtl.Cell|
|{2}|Name|string|Cellの名称|

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https:/{UnitFQDN}/__ctl/Cell(Name='{CellName}')",
        "etag": "W/\"1-1486427790039\"",
        "type": "UnitCtl.Cell"
      },
      "Name": "{CellName}",
      "__published": "/Date(1486427790039)/",
      "__updated": "/Date(1486427790039)/"
    }
  }
}
```


### cURLサンプル

```sh
curl "https://{UnitFQDN}/__ctl/Cell('{CellName}')" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

###### Copyright 2017 FUJITSU LIMITED

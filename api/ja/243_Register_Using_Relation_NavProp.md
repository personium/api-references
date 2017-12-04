# Relation_NavProp経由登録
### 概要
Cell制御オブジェクトNavigation Property経由で登録し、同時に$linksを登録する。

### 必要な権限
write

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションは無視される


### リクエスト
#### リクエストURL
##### BoxへのnavigationProperty
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Box
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_Box
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/_Box
```
##### ExtCellへのnavigationProperty
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtCell
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_ExtCell
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/_ExtCell
```
##### ExtRoleへのnavigationProperty
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtRole
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_ExtRole
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/_ExtRole
```
##### RoleへのnavigationProperty
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Role
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/_Role
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/_Role
```
※ \_Box.Nameパラメタを省略した場合は、nullが指定されたものとする

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
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
#### リクエストボディ
##### Boxを登録する場合
##### Format
JSON

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Name|Relation名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー)と:(コロン)は指定不可|○||
|_Box.Name|関係対象のBox名|桁数：1&#65374;128<br>文字種：半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>説明：Box登録APIにて登録済みのBoxのNameを指定<br>特定のBoxと関連付けない場合はnullを指定|×||

##### リクエストサンプル
```JSON
{
  "Name": "{RelationName}",
  "_Box.Name": "{BoxName}"
}
```

### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|Location|作成したリソースへのURL||
|DataServiceVersion|ODataのバージョン||
|ETag|リソースのバージョン情報||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ

|オブジェクト|項目名|Data Type|備考|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

##### Boxを登録した場合
##### Box固有レスポンスボディ

|オブジェクト|項目名|Data Type|備考|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Box|
|{2}|Name|string|Box名|
|{2}|Schema|string|Schema名|

##### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')",
        "etag": "W/\"1-1486945452485\"",
        "type": "CellCtl.Box"
      },
      "Name": "{BoxName}",
      "Schema": null,
      "__published": "/Date(1486945452485)/",
      "__updated": "/Date(1486945452485)/"
    }
  }
}
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル

##### Boxを登録した場合
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Box" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{BoxName}"}'
```

###### Copyright 2017 FUJITSU LIMITED

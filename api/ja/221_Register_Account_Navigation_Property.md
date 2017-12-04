# Account_NavProp経由登録
### 概要
RoleをAccountのNavigation Property経由で登録する
### 必要な権限
write
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない


### リクエスト
#### リクエストURL
##### RoleへのnavigationProperty
```
/{CellName}/__ctl/Account(Name='{AccountName}')/_Role
```
または、
```
/{CellName}/__ctl/Account('{AccountName}')/_Role
```
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
Roleを登録する場合

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Name|アカウント名|文字種:半角英数字と左記半角記号（-_!$*=^`{&#124;}~.@）<br>ただし、先頭文字に半角記号は指定不可|○||
#### リクエストサンプル
```JSON
{
  "Name": "{AccountName}"  
}
```


### レスポンス
#### ステータスコード
201
#### レスポンスヘッダ
|項目名|概要|備考|
|:--|:--|:--|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|Content-Type|返却されるデータの形式||
|Location|作成したリソースへのURL||
|ETag|リソースのバージョン情報||
|DataServiceVersion|ODataのバージョン||
#### レスポンスボディ
|項目名|概要|備考|
|:--|:--|:--|
|d|||
|d / results|||
|d / results / __published|作成日||
|d / results / __updated|更新日||
|d / results / __metadata|||
|d / results / __metadata / etag|ETag値||
|d / results / __metadata / uri|作成したリソースへのURL||
|d / results / __metadata / type|EntityType||
|d / results / Name|Role名||
|d / results / _Box.Name|関係対象のBox名||
##### Accountを登録した場合
Account固有レスポンスボディ

|オブジェクト|項目名|Data Type|備考|
|:--|:--|:--|:--|
|{2}|Name|string|Account名|
|{2}|Cell|string|null|
|{2}|Type|string|basic|
|{3}|Type|string|CellCtl.Account|

#### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "Name": "{AccountName}",
      "__published": "/Date(1349355810698)/",
      "Cell": null,
      "__updated": "/Date(1349355810698)/",
      "Type": "basic",
      "__metadata": {
        "etag": "1-1349355810698",
        "type": "CellCtl.Account",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')"
      }
    }
  }
}   
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル

##### AccountとRoleのnavigationProperty経由登録
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('acount_name')/_Role" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{RoleName}"}'
```

###### Copyright 2017 FUJITSU LIMITED

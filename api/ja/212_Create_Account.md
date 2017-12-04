# Account登録
### 概要
Accountを登録する

### 必要な権限
auth

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない


### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/Account
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
|X-Personium-Credential|パスワード|文字列|×|文字数：6&#65374;32文字<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)|
#### リクエストボディ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Name|アカウント名|桁数：1&#65374;128<br>文字種:半角英数字と下記半角記号<br>-_!$*=^`{&#124;}~.@<br>ただし、先頭文字に半角記号は指定不可|○||
|Type|アカウントタイプ|basic(ID/PWによる認証)<br>oidc:google(Google OpenID Connectによる認証)<br>または上記２つをスペースで区切る|×|デフォルト：basic|
|LastAuthenticated|最終認証日時|/Date(【long型の時刻】)/の形式で文字列で指定する<br>【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)|×|デフォルト：null|
#### リクエストサンプル
ID/PW認証用アカウント
```JSON
{
  "Name": "{AccountName}"
}
```
Google認証用アカウント
```JSON
{
  "Name": "{AccountName}","Type":"oidc:google"
}
```
ID/PW認証＋Google認証用アカウント
```JSON
{
  "Name": "{AccountName}","Type":"basic oidc:google"
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

|オブジェクト|項目名|型|備考|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

#### Account固有レスポンスボディ

|オブジェクト|項目名|型|備考|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Account|
|{2}|Name|string|Account名|
|{2}|LastAuthenticated|string|デフォルト：null|
|{2}|Type|string|デフォルト:"basic"|
|{2}|Cell|string|デフォルト：null|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
ID/PW認証用アカウント
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "basic",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```
Google認証用アカウント
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "oidc:google",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```
ID/PW認証＋Google認証用アカウント
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "basic oidc:google",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```


### cURLサンプル
ID/PW認証用アカウント
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}"}'
```
Google認証用アカウント
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"oidc:google"}'
```
ID/PW認証＋Google認証用アカウント
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"basic oidc:google"}'
```

###### Copyright 2017 FUJITSU LIMITED

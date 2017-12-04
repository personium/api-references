# ExtCell更新
### 概要
既存のExtCell情報を更新する

### 必要な権限
auth

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションは無視される


### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/ExtCell('http%3A%2F%2F{UnitFQDN}%2F{CellName}')
```
#### メソッド
PUT

#### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
&#160;

#### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
#### リクエストボディ

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Url|CellへのURL|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http, https<br>トレイリングスラッシュ(URL終端の/)必須|○||
#### リクエストサンプル
```JSON
{
  "Url": "https://{UnitFQDN}/{CellName}/"
}
```

### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ
なし

#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('http%3A%2F%2F{UnitFQDN}%2F{CellName}')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
 -H 'Accept: application/json' -d '{"Url":"http://{UnitFQDN}/{CellName}/"}'
```

###### Copyright 2017 FUJITSU LIMITED

# Account部分更新
## 概要
既存のAccount情報を部分更新する

### 必要な権限
auth

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない


## リクエスト
### リクエストURL
```
{CellURL}__ctl/Account(Name='{AccountName}')
```
または、
```
{CellURL}__ctl/Account('{AccountName}')
```
### メソッド
MERGE

### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合は、UUIDの独自短縮表現として${4桁}_${18桁}の形式のBase64url文字列を自動採番|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
|X-Personium-Credential|パスワード|文字列|×|※unitの設定のパスワード制限に従う<br>デフォルトは以下の通り<br>文字数：6&#65374;32文字<br>文字種:半角英数字と下記半角記号<br>-_!$\*=^\`{&#124;}~.@|
### リクエストボディ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Name|アカウント名|桁数：1&#65374;128<br>文字種:半角英数字と右記半角記号（-_!$\*=^\`{&#124;}~.@）<br>ただし、先頭文字に半角記号は指定不可|×||
|Type|アカウントタイプ|basic(ID/PWによる認証)<br>oidc:google(Google OpenID Connectによる認証)<br>または上記２つをスペースで区切る|×||
|IPAddressRange|IPアドレス帯|認証を許可するIPアドレス帯を指定する<br>カンマ区切りで複数指定、プレフィックス表記による範囲指定可<br>nullの場合は全てのIPアドレスで認証可とする|×||
|Status|ステータス|アカウントの状態を指定する<br>[Account登録](./212_Create_Account.md)の「Status」を参照|×||

### リクエストサンプル
アカウント名更新
```JSON
{
  "Name": "account2"
}
```
アカウント名+アカウントタイプ更新
```JSON
{
  "Name": "account2","Type":"oidc:google"
}
```

## レスポンス
### ステータスコード
204

### レスポンスヘッダ
なし

### レスポンスボディ
なし

### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

## cURLサンプル
アカウント名更新
```sh
curl "https://cell1.unit1.example/__ctl/Account('account1')" -X MERGE -i -H \
'If-Match: *' -H 'X-Personium-Credential:password' -H 'Authorization: Bearer AA~PBDc...(省略)...FrTjA' -H \
'Accept: application/json' -d '{"Name":"account2"}'
```
アカウント名+アカウントタイプ更新
```sh
curl "https://cell1.unit1.example/__ctl/Account('account1')" -X MERGE -i \
-H 'If-Match: *' -H 'X-Personium-Credential:password' \
-H 'Authorization: Bearer AA~PBDc...(省略)...FrTjA' \
-H 'Accept: application/json' -d '{"Name":"account2","Type":"oidc:google"}'
```

アカウント名+ステータス更新
```sh
curl "https://cell1.unit1.example/__ctl/Account('account1')" -X MERGE -i -H \
'If-Match: *' -H 'Authorization: Bearer AA~PBDc...(省略)...FrTjA' -H \
'Accept: application/json' -d '{"Name":"account2","Status":"deactivated"}'
```

パスワードの初期化（変更を強制）
```sh
curl "https://cell1.unit1.example/__ctl/Account('account1')" -X MERGE -i -H \
'If-Match: *' -H 'X-Personium-Credential:Initial_password' -H 'Authorization: Bearer AA~PBDc...(省略)...FrTjA' -H \
'Accept: application/json' -d '{"Status":"passwordChangeRequired"}'
```

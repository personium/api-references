# ファイル登録更新
### 概要
WebDav情報を登録・更新する。
### 必要な権限
write
### 制限事項
#### 共通制限
なし
#### WebDAV制限
未稿


### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{ResourcePath}
```
|パス|概要|備考|
|:--|:--|:--|
|{CellName}|セル名||
|{BoxName}|ボックス名||
|{ResourcePath}|リソースへのパス|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)|
#### メソッド
PUT
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
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
|Content-Type|登録・更新ファイルのコンテンツ形式を指定する|String|×|SWF形式で登録・更新する場合<br>Content-Type:application/x-shockwave-flash<br>PDF形式で登録・更新する場合<br>Content-Type:application/pdf<br>JPG形式で登録・更新する場合<br>Content-Type:image/jpeg<br>js形式で登録・更新する場合<br>Content-Type:application/x-javascript|
#### リクエストボディ
|概要|有効値|必須|備考|
|:--|:--|:--|:--|
|登録・更新するコンテキスト情報をバイナリでリクエストボディに指定する|Content-Typeヘッダで指定した方式|○||
#### リクエストサンプル
なし


### レスポンス
#### ステータスコード
|コード|メッセージ|概要|
|:--|:--|:--|
|201|Created|登録成功時|
|204|No Content|更新成功時|
#### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式|更新・作成時に失敗した場合のみ返却する|
|ETag|リソースのバージョン情報||
#### レスポンスボディ
更新・作成時に失敗した場合のみ返却する
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{【ファイル内容】}'
```

###### Copyright 2017 FUJITSU LIMITED

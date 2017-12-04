# Entity更新
### 概要
ユーザデータのEntityを更新します。
### 必要な権限
write
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
* ユーザデータ制限事項
	- Edm.DateTime型のプロパティの有効範囲のチェックが適切に行われない
	- Edm.DateTime型の配列は未サポート
	- Edm.DateTime型のプロパティにSYSUTCDATETIME()を指定した場合、設定されるシステム時間が異なる場合がある
      - リクエストボディでの設定時とDefaultValueでの設定時（\__published、\__updatedは後者のタイミング）
	- 1つのEntityTypeに対して作成出来るのは、DynamicProperty・DeclaredProperty・ComplexTypeProperty合わせて400個まで


### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}({EntityID})}
```
|パス|概要|
|:--|:--|
|{CellName}|セル名|
|{BoxName}|ボックス名|
|{ODataCollecitonName}|コレクション名|
|{EntityTypeName}|EntityType名|
|{EntityID}|更新するEntityのID|
#### メソッド
PUT
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
##### OData 共通リクエストクエリ
なし

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
##### OData更新リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Contents-Type|OAuth2.0形式で、認証情報を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
#### リクエストボディ
##### プロパティ
スキーマ定義済みのプロパティと動的（スキーマ未定義）プロパティ、合わせて最大で400個のプロパティを設定可能  
上記にはComplexTypeで定義されているプロパティ数を含む

##### スキーマ定義済みのプロパティ
|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|EntityTypeに紐づくProperty|ユーザ定義項目|デフォルト値 PropertyのDefaultValueに基づく|PropertyのNullableに基づく||
##### スキーマ定義済みプロパティのvalueの有効値  
|データ型|有効値|
|:--|:--|
|文字列|桁数：0&#65374;51200 byte<br>文字種: 文字列の値に制御コードを使用した場合、取得時にエスケープした状態で返却する<br>「\」を使用する場合、「\\\」で指定する必要がある<br>文字列型のプロパティに整数値、小数値、真偽値、日付型の値を設定した場合、文字列型に変換して登録される|
|整数値|-2147483648 &#65374; 2147483647|
|小数値|整数部分の桁数：1&#65374;5桁<br>小数部分の桁数：1&#65374;5桁|
|真偽値|true / false / null(nullを指定した場合はfalseとして扱う)|
|日付|/Date(【long型の時刻】)/の形式で文字列で指定する<br>【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>また、予約語として以下を指定可能<br>SYSUTCDATETIME()：サーバ時間|
##### 動的（スキーマ未定義）プロパティ
スキーマ定義済みのプロパティと動的（スキーマ未定義）プロパティ、合わせて最大で400個のプロパティを設定可能  
上記にはComplexTypeで定義されているプロパティ数を含む
##### 動的プロパティのkeyの有効値
|データ型|有効値|
|:--|:--|
|文字列|桁数：1&#65374;128 :<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>_ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可  <br>_published、_updatedは、予約語であるためリクエストボディの指定は不可|
##### 動的プロパティのvalueの有効値
スキーマ定義済みプロパティのvalueの有効値と同様  
配列、連想配列は指定不可

#### リクエストサンプル
```JSON
{"animalId": "100-2","name": "episode2","startedAt": "2016-02-21","episodeType": "care2","endedAt": "","outcome": "治療済"}
```


### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|ETag|リソースのバージョン情報||
|DataServiceVersion|ODataのバージョン||
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"animalId": "100-2","name": "episode2","startedAt":"2016-02-21","episodeType": "care2","endedAt": "","outcome": "治療済"}'
```

###### Copyright 2017 FUJITSU LIMITED

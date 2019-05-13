# Box再帰削除
## 概要
* 指定されたBox、Box配下のデータ、Boxとリンクしているデータを全て削除する。
* 削除対象のデータは下記
	- Role
	- Relation
	- ExtRole
	- ReceivedMessage
	- SentMessage
	- Collection
	- AssociationEnd
	- EntityType
	- ComplexType
	- Property
	- ComplexTypeProperty
	- $links
	- ユーザデータ

### 必要な権限
Box


## リクエスト
### リクエストURL
```
{CellURL}{BoxName}
```

### メソッド
DELETE

### リクエストクエリ
なし

### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UUIDで32文字の文字列}を設定する|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|○|※認証トークンは認証トークン取得APIで取得したトークン|
|X-Personium-Recursive|一括削除の指定|文字列|○|trueの場合は一括削除APIを実施<br>falseの場合およびヘッダの指定がなかった場合はエラーレスポンスコード412を返却|

### リクエストボディ
なし


## レスポンス
### ステータスコード
204

### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
### レスポンスボディ
なし

### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

## cURLサンプル

```sh
curl "https://cell1.unit1.example/box1" -X DELETE -i -H 'X-Personium-Recursive: true' -H \
'Authorization: Bearer AA~PBDc...(省略)...FrTjA' -H 'Accept: application/json'
```


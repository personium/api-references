# ExtRole更新
### 概要
ExtRoleを登録する
### 制限事項
* 共通制限
	* なし
* OData 制限
	* リクエストヘッダのAcceptは無視される
	* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
	* リクエストボディはJSON形式のみ受け付ける
	* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
	* $formatクエリオプションは無視される

### 必要な権限
auth


### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```
※ \_Relation.\_Box.Nameパラメタを省略した場合は、nullが指定されたものとする
#### メソッド
PUT
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
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
#### リクエストボディ
|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|ExtRole|外部ロール名(URL)|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn<br>Boxに紐付いている場合:/{アプリセル名}/&#95;&#95;role/&#95;&#95;/{Role名}<br>※ただし、BoxにSchema情報が登録されていない場合、Boxに紐付いていないとみなされる<br>Boxに紐付いていない場合:/{Cell名]/&#95;&#95;role/&#95;&#95;/{Role名}|○||
|_Relation.Name|関係対象のRelation名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー):(コロン)は指定不可<br>Relation登録APIにて登録済みのRelation<br>null|○||
|_Relation._Box.Name|関係対象のRelationが紐づくBox名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>Relation登録APIにて登録済みのRelationが紐づくBox名<br>null|×||
#### リクエストサンプル
```JSON
{
  "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/roletest",
  "_Relation.Name": "{RelationName}",
  "_Relation._Box.Name": "{BoxName}"  
}
```


### レスポンス
#### ステータスコード
204
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル

```sh
curl curl "https://{UnitFQDN}/{CellName}/__ctl/ExtRole(ExtRole='https%3A%2F%2F{UnitFQDN}%2F{CellName}%2F__role%2F__%2F{ExtRoleName}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{ "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/{ExtRoleName}", "_Relation.Name":"{RelationName}","_Relation._Box.Name": "{BoxName}"}'
```

###### Copyright 2017 FUJITSU LIMITED

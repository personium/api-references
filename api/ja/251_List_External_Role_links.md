# ExtRoleと他オブジェクトとのリンク一覧取得
## 概要
ExtRoleに紐付いたODataリソースを一覧取得する  
以下のODataリソースを指定することができる  

* Relation
* Role

### 必要な権限
|紐付け先|必要な権限|
|:-|:-|
|Relation|auth-read<br>social-read|
|Role|auth-read|

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポ  ートし、レスポンスボディはJSON形式とする


## リクエスト
### リクエストURL
#### Roleとの$links
```
{CellURL}__ctl/ExtRole(ExtRole='https%3A%2F%2F{CellName}.{UnitFQDN}%2F__role%2F__%2F{RoleName}',
_Relation.Name='{RelationName}')/$links/_Role
```
#### Relationとの$links
```
{CellURL}__ctl/ExtRole(ExtRole='https%3A%2F%2F{CellName}.{UnitFQDN}%2F__role%2F__%2F{RoleName}',
_Relation.Name='{RelationName}')/$links/_Relation
```

※ \_Box.Nameパラメタを省略した場合は、nullが指定されたものとする

### メソッド
GET

### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|_

<!---
[$select クエリ](406_Select_Query.md)

[$expand クエリ](405_Expand_Query.md)

[$format クエリ](404_Format_Query.md)

[$filter クエリ](403_Filter_Query.md)

[$inlinecount クエリ](407_Inlinecount_Query.md)

[$orderby クエリ](400_Orderby_Query.md)
-->

[$top クエリ](401_Top_Query.md)

[$skip クエリ](402_Skip_Query.md)

<!---
[全文検索(q)クエリ](408_Full_Text_Search_Query.md)
-->

### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合は、UUIDの独自短縮表現として${4桁}_${18桁}の形式のBase64url文字列を自動採番|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
### リクエストボディ
なし


## レスポンス
### ステータスコード
200

### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|DataServiceVersion|ODataのバージョン||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|

### レスポンスボディ

|オブジェクト|項目名|Data Type|備考|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|uri|string|紐付いているODataリソースへのURL|
### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

### レスポンスサンプル
```JSON
{
  "d": {
    "results": [
      {
        "uri": "https://cell1.unit1.example/__ctl/Role(Name='role1',_Box.Name='box1')"
      },
      {
        "uri": "https://cell2.unit1.example/__ctl/Role(Name='role2',_Box.Name=null)"
      }
    ]
  }
}
```

## cURLサンプル

```sh
curl "https://cell1.unit1.example/__ctl/ExtRole(ExtRole='https%3A%2F%2Fcell2.unit1.example%2F\
__role%2F__%2Frole1',_Relation.Name='relation1',_Relation._Box.Name='box1')/\$links/_Role" -X GET \
-i -H 'Authorization: Bearer AA~PBDc...(省略)...FrTjA' -H 'Accept: application/json'
```


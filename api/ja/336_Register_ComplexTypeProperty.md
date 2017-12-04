# ComplexTypeProperty登録
### 概要
ユーザーデータに指定するComplexTypePropertyを定義する

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
* 個別の制限
	- 関連対象のComplexTypeを使用したEntityTypeのユーザーデータが存在する場合は、Nullableがtrueの場合のみ登録可能
	- ComplexTypeの階層数が5階層以内となる場合のみ、登録可能
	- Edm.DateTimeのDefaultValueの有効範囲のチェックが適切に行われない
* Personiumの仕様として定めている制限
	- 1つのEntityTypeに対して作成出来るのは、DynamicProperty・DeclaredProperty・ComplexTypeProperty合わせて400個まで
	- 各階層のComplexTypePropertyのタイプがSimpleTypeの最大登録個数は下記個数まで登録可能
     - 2階層目：50個
     - 3階層目：30個
     - 4階層目：10個
	- 各階層のComplexTypePropertyのタイプがComplexTypeの最大登録個数は下記個数まで登録可能
     - 1階層目：20個
     - 2階層目：5個
     - 3階層目：2個
     - 4階層目：0個

### 必要な権限
alter-schema


### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty
```
#### メソッド
POST

#### リクエストクエリ

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

##### OData共通リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|

##### OData登録リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
#### リクエストボディ
##### Format
JSON

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Name|ComplexTypeProperty名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可|○||
|_ComplexType.Name|紐付くComplexType名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可|○||
|Type|型定義|Edm.Boolean / Edm.String / Edm.Int32 / Edm.Single / Edm.Double / Edm.DateTime / 登録済みComplexType名|○||
|Nullable|Null値許可|true / false<br>デフォルト値は Null|×||
|DefaultValue|デフォルト値|下記表を参照<br>デフォルト値は Null|×||
|CollectionKind|配列種別|None / List<br>デフォルト値は "None"|×||

##### Valid values &#8203;&#8203;for DefaultValue
DefaultValueの有効値はTypeの値（型定義）によって異なり、以下の定義となる型の異なる項目についても文字列で定義を行う  

|Type Value|有効値|
|:--|:--|
|Edm.Boolean|true / false|
|Edm.String|桁数：0&#65374;51200 byte<br>「\」を使用する場合、「\\\」で指定する必要がある|
|Edm.Int32|-2147483648　&#65374;　2147483647|
|Edm.Single|整数部分の桁数：1&#65374;5桁<br>小数部分の桁数：1&#65374;5桁|
|Edm.Double|15 桁の有効桁数を持つ浮動小数点数を表します。|
|Edm.DateTime|/Date(【long型の時刻】)/の形式で文字列で指定する<br>　【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>また、予約語として以下を指定可能<br>　SYSUTCDATETIME()：サーバ時間|

#### リクエストサンプル
```JSON
{"Name": "{ComplexTypePropertyName}","_ComplexType.Name": "{ComplexTypeName}","Type": "Edm.String","Nullable": true,"DefaultValue": null,"CollectionKind": "None"}
```

### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ
##### 共通レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|

##### ODataレスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|Location|作成したリソースへのURL||
|DataServiceVersion|ODataのバージョン||
|ETag|リソースのバージョン情報||

#### レスポンスボディ
##### 共通レスポンスボディ

レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト|名前（キー）|型|値|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

##### Property固有レスポンスボディ

|オブジェクト|名前（キー）|型|値|
|:--|:--|:--|:--|
|{3}|type|string|ODataSvcSchema.ComplexTypeProperty|
|{2}|Name|string|ComplexTypeProperty名|
|{2}|Type|string|型定義|
|{2}|Nullable|boolean|Null値許可|
|{2}|DefaultValue|string|デフォルト値|
|{2}|CollectionKind|string|配列種別|

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty(Name='{ComplexTypePropertyName}',_ComplexType.Name='{ComplexTypeName}')",
        "etag": "W/\"1-1487658277593\"",
        "type": "ODataSvcSchema.ComplexTypeProperty"
      },
      "Name": "{ComplexTypePropertyName}",
      "_ComplexType.Name": "{ComplexTypeName}",
      "Type": "Edm.String",
      "Nullable": true,
      "DefaultValue": null,
      "CollectionKind": "None",
      "__published": "/Date(1487658277593)/",
      "__updated": "/Date(1487658277593)/"
    }
  }
}
```


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/ComplexTypeProperty" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name": "{ComplexTypePropertyName}","_ComplexType.Name": "{ComplexTypeName}","Type": "Edm.String","Nullable": true,"DefaultValue": null,"CollectionKind": "None"}'
```

###### Copyright 2017 FUJITSU LIMITED

# クロスドメインポリシーファイル
### 概要
クロスドメインポリシーファイル(crossdomain.xml)を取得します。  
クロスドメインポリシーファイルはすべての利用者が取得できます。
### 必要な権限
なし
### 制限事項
特になし


### リクエスト
#### リクエストURL
```
/crossdomain.xml
```
#### メソッド
GET
#### リクエストクエリ
なし
#### リクエストヘッダ
なし
#### リクエストボディ
なし
#### リクエストサンプル
なし


### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
なし
#### レスポンスボディ
XML形式

|項目名|概要|
|:--|:--|
|/cross-domain-policy/site-control|メタポリシーの許可設定情報。固定で属性値「permitted-cross-domain-policies="all"」を返却する|
|/cross-domain-policy/allow-access-from|現在のドメインにアクセス可能なドメイン。固定で属性値「domain="*"」を返却する|
|/cross-domain-policy/allow-http-request-headers-from|現在のドメインに送信可能なヘッダ情報と、ヘッダ送信元ドメイン。固定で属性値「domain="\*" headers="*"」を返却する|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル
```sh
curl "https://{UnitFQDN}/crossdomain.xml" -X GET -i
```

###### Copyright 2017 FUJITSU LIMITED

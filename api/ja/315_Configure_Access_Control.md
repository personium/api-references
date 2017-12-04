# Box Level アクセス制御設定
### 概要
Box Level のアクセス制御機能を提供する

### 必要な権限
write-acl

### 制限事項
* ACL設定を行うと、既存のACL設定を上書きされる形で更新されます
* ACL設定を打ち消す機能（deny）
* ACLで設定出来るprivilegeの一覧取得


### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}
```
または、
```
/{CellName}/{BoxName}/{ResourcePath}
```

|パス|概要|備考|
|:--|:--|:--|
|{CellName}|セル名||
|{BoxName}|ボックス名||
|{ResourcePath}|リソースへのパス|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)|

#### メソッド
ACL

#### リクエストクエリ
なし

#### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|

#### リクエストボディ
名前空間

|URI|概要|参考prefix|
|:--|:--|:--|
|DAV:|WebDAVの名前空間|D:|
|urn:x-personium:xmlns|Personiumの名前空間|p:|

※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。


XMLの構造  
ボディはXMLで、以下のスキーマに従っています。  
privilegeタグ配下の権限設定の内容については、acl_model（[アクセス制御モデル](../../user_guide/002_Access_Control.html)）を参照。

|ノード名|名前空間|ノードタイプ|概要|備考|
|:--|:--|:--|:--|:--|
|acl|D:|要素|ACL（アクセス制御リスト）のルートを表し、1つ以上複数のaceが子となる||
|base|xml:|属性|hrefタグ内に記述するURLの基底を表し、任意の値を属性値とする。この属性は任意。||
|ace|D:|要素|ACE（アクセス制御エレメント）を表し、principalとgrantが一対で子となる|「invert」「deny」「protected」「inherited」はV1.1系未対応|
|principal|D:|要素|権限設定対象を表し、hrefまたはallが子となる||
|grant|D:|要素|権限付与設定を表し、1つ以上複数のprivilegeが子となる||
|href|D:|要素|権限設定対象ロール表し、ロールリソースURLを入力するテキストノード|権限設定対象ロールのリソースURLを指定する acl要素内のxml:base属性の設定によって、URLを短縮する事が出来る|
|all|D:|要素|全アクセス主体権限設定||
|privilege|D:|要素|権限設定を表し、以下の要素のいづれか一つが子となる||
|read|D:|要素|参照権限||
|write|D:|要素|編集権限||
|read-properties|D:|要素|プロパティ参照権限||
|write-properties|D:|要素|プロパティ編集権限||
|read-acl|D:|要素|ACL設定参照権限||
|write-acl|D:|要素|ACL設定編集権限||
|bind|D:|要素|未稿|V1.1系、V1.2系未対応|
|unbind|D:|要素|未稿|V1.1系、V1.2系未対応|
|exec|D:|要素|サービス実行権限||


DTD表記

名前空間 D:
```dtd
<!ELEMENT acl (ace*) >
<!ELEMENT ace ((principal or invert), (grant or deny), protected?,inherited?)>
<!ELEMENT principal (href or all)>
<!ELEMENT principal (privilege*)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT all EMPTY>
<!ELEMENT privilege (all or read or write or read-properties or write-properties or read-acl or write-acl or exec or bind or unbind)>
<!ELEMENT read EMPTY>
<!ELEMENT write EMPTY>
<!ELEMENT read-properties EMPTY>
<!ELEMENT write-properties EMPTY>
<!ELEMENT read-acl EMPTY>
<!ELEMENT write-acl EMPTY>
<!ELEMENT bind EMPTY>
<!ELEMENT unbind EMPTY>
<!ELEMENT exec EMPTY>
```

名前空間 p:
```dtd
<!ATTLIST acl requireSchemaAuthz (none or public or confidential) #IMPLIED>
<!ELEMENT exec EMPTY>   
```

名前空間 xml:
```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### リクエストサンプル
```xml
<?xml version="1.0" encoding="utf-8" ?>
<D:acl xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"
       xml:base="https://{UnitFQDN}/{CellName}/__role/{BoxName}/"
       p:requireSchemaAuthz="public">
    <D:ace>
        <D:principal>
            <D:all/>
        </D:principal>
        <D:grant>
            <D:privilege><D:read/></D:privilege>
        </D:grant>
    </D:ace>
    <D:ace>
        <D:principal>
            <D:href>role</D:href>
        </D:principal>
        <D:grant>
            <D:privilege><D:read/></D:privilege>
            <D:privilege><D:write/></D:privilege>
        </D:grant>
    </D:ace>
</D:acl>
```


### レスポンス
#### ステータスコード

|コード|メッセージ|概要|
|:--|:--|:--|
|200|OK|成功|
#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式|更新・作成時に失敗した場合のみ返却する|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X ACL -i
-H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d
'<?xml version="1.0" encoding="utf-8" ?>
 <D:acl xmlns:D="DAV:" xml:base="https://{UnitFQDN}/{CellName}/__role/{BoxName}/"　xmlns:p="urn:x-personium:xmlns" p:requireSchemaAuthz="none">
  <D:ace>
   <D:principal>
    <D:href>doctor</D:href>
   </D:principal>
   <D:grant>
    <D:privilege>
     <D:read/>
    </D:privilege>
    <D:privilege>
     <D:write/>
    </D:privilege>
   </D:grant>
  </D:ace>
 </D:acl>'
```

###### Copyright 2017 FUJITSU LIMITED

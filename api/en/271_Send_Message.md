# SendMessage

### Overview

send message

### Required Privileges

message

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

```
/{CellName}/__message/send
```

#### Request Method

POST

#### Request Query

None

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|BoxBound|Linking with Box, whether possible or not|true / false<br>The default value is false|No|Send a token you have schema authentication is set to true this item when you tie in Box (not implemented)|
|InReplyTo|Message ID of the reply message|Number of digits: 32<br>null|No||
|To|Destination cell URL|URL format<br>null|* 1|If you want to send message to multiple cells specified in CSV format,<br>*1 required either Relation or To<br>Maximum 1000 cell URL destination can be specified in Relation or To|
|ToRelation|Relationship names to be sent|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_"), plus, :<br>However, the string cannot start with a underscore ("\_") or colon (:)<br>null|* 1|*1 required either Relation or To<br>Maximum 1000 cell URL destination can be specified in Relation or To|
|Type|Message type|message<br>req.relation.build<br>req.relation.break<br>req.role.grant<br>req.role.revoke|No|The message is treated as a default|
|Title|Message Title|Number of digits: 256 characters or less|No|The default is treated as an empty string|
|Body|Message Body|Number of digits: 64Kbyte following|No|The default is treated as an empty string|
|Priority|Priority|1~5|No|The default is treated as 3|
|RequestRelation|Information of requested relation to register|URL format<br>null|* 2|*2 Required when message type is other than message<br>Relation name or relation class URL or role name or role class URL, the registration request<br>When only the relation name is specified, it is regarded as a relative URL from the following URL<br>BoxBound is true:[target Box schema URL]\_\_relation/\_\_/<br>BoxBound is false:[destination cell URL]\_\_relation/\_\_/<br>When only the role name is specified, it is regarded as a relative URL from the following URL<br>BoxBound is true:[target Box schema URL]\_\_role/\_\_/<br>BoxBound is false:[destination cell URL]\_\_role/\_\_/|
|RequestRelationTarget|Cell URL that connects the relationship|URL format<br>null|* 2|*2 Required when message type is other than message|

#### Request Sample

```JSON
{
  "BoxBound": true,
  "InReplyTo": "hnKXm44TTZCw-bfSEw4f0A",
  "To": "https://{UnitFQDN}/{TargetCellName}",
  "ToRelation": null,
  "Type": "req.relation.build",
  "Title": "Friend request",
  "Body": "Thank you for the friend approval",
  "Priority": 3,
  "RequestRelation": "https://{UnitFQDN}/{AppCellName}/__relation/__/{RelationName}",
  "RequestRelationTarget": "https://{UnitFQDN}/{CellName}"
}
```


### Response

#### Response Code

201

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Location|URL to the resource that was created||
|DataServiceVersion|OData version||
|ETag|Resource version information||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

#### Response Body

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object|Name (Key)|Type|Value|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

##### SentMessage specific response body

|Object|Name (Key)|Type|Value|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ReceivedMessage|
|{2}|__id|string|ReceivedMessage ID<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID|
|{2}|InReplyTo|string|ID message you are replying<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID|
|{2}|To|string|Destination cell URL|
|{2}|ToRelation|string|Relationship names to be sent|
|{2}|Type|string|Message type<br>message: message<br>relationship registration request(relation): req.relation.build<br>relationship deletion request(relation): req.relation.break<br>relationship registration request(role): req.role.grant<br>relationship deletion request(role): req.role.revoke|
|{2}|Title|string|Message Title|
|{2}|Body|string|Message Body|
|{2}|Priority|string|Priority<br>(high)1 - 5(low)|
|{2}|RequestRelation|string|Relation name or relation class URL or role name or role class URL, the registration request<br>Only when message type is other than message|
|{2}|RequestRelationTarget|string|CellURL of relationships<br>Only when message type is other than message|
|{2}|_Box.Name|string|BoxName for Relation|
|{2}|Result|array|Transmission result of each destination Cell<br>Array object {4}|
|{4}|To|string|Destination cell URL|
|{4}|Code|string|Response Code|
|{4}|Reason|string|Detailed message|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('3afcc60e35fc49ee9a4e4f6c1ebee426')",
        "etag": "W/\"1-1486638759524\"",
        "type": "CellCtl.SentMessage"
      },
      "__id": "3afcc60e35fc49ee9a4e4f6c1ebee426",
      "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
      "To": "https://{UnitFQDN}/{CellName}",
      "ToRelation": null,
      "Type": "message",
      "Title": "Message Sample Title",
      "Body": "Message Sample Body",
      "Priority": 3,
      "RequestRelation": null,
      "RequestRelationTarget": null,
      "_Box.Name": null,
      "Result": [
        {
          "To": "https://{UnitFQDN}/{CellName}/",
          "Code": "201",
          "Reason": "Created."
        }
      ],
      "__published": "/Date(1486638759524)/",
      "__updated": "/Date(1486638759524)/"
    }
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__message/send" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"BoxBound":false,"InReplyTo":"xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ","To":"https://{UnitFQDN}/{CellName}","Type":"message","Title":"Message Sample Title","Body":"Message Sample Body","Priority":3}'
```


###### Copyright 2017 FUJITSU LIMITED

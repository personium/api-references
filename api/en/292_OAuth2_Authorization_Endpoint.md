# OAuth2.0 Authorization Endpoint(\_\_authz)

### Overview

OAuth2 Authorization Endpoint  
This API is the OAuth2 authorization endpoint, for utilizing the Personium on JS application and native application.

### Required Privileges

None

### Precondition

In order to execute this API, it is necessary to prepare in advance Box which has the application cell URL in the schema.

### Restrictions

Request query, specification of "p\_target" parameter of request body is not supported

* When p_target is specified, an error occurs in nginx because the value of "Location" in the response header exceeds 4,096 characters.
* Internet Explorer can not redirect correctly because the maximum URL length is limited to 2048 characters.


### Request

#### Request URL

```
{CellName}/__authz
```

#### Request Method

GET : Authentication form request  
POST : Authentication form request, Authentication Token

#### Request Query

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|response_type|Response Type|String|Yes|Token|
|client_id|Application Cell URL|String|Yes|Application Cell URL of authentication form request|
|redirect_uri|Client redirect endpoint URL|String|Yes|URL of the redirect script registered under the default BOX of the application cell<br>Query parameters formatted with application/x-www-form-urlencoded can be included<br>It is not possible to include fragments<br>Effective digit length:512byte|
|state|Random value used to maintain state between request and callback|String|No|Random value<br>Effective digit length:512byte|
|p_target|Transcell token target|String|No|Cell URL using token to be paid out<br>When specified, pays out a transcell token|
|p_owner|ULUUT Execute promotion Query|String|No|Valid only for true<br>* When this query is set, authentication information is not returned as cookie|
|assertion|Access token|String|No|A valid TransCell token<br>In the case of no argument, it does not become token authentication|
|username|User name|String|No|Registered user name|
|password|Password|String|No|Registered password|

#### Request Header

None

#### Request Body

Same as request query

#### Request Sample

None


### Response

#### Forms Authentication Request

##### Response Code

200

##### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Content-Type of Resource||

##### Response Body

Return the following HTML form.<br>![Response body](image/OAuth2ResponseBody.png "Response body")

##### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

##### Response Sample

None

#### Request Token Authentication

##### Response Code

302  
The browser is redirected to redirect\_uri. A fragment indicated by "URL parameter" is stored in redirect\_uri.

```
{redirect_uri}#access_token={access_token}&token_type=Bearer&expires_in={expires_in}&state={state}
```

|Item Name|Overview|Notes|
|:--|:--|:--|
|redirect_uri|Client redirect endpoint URL|The value of "redirect_uri" in the request|
|access_token|Access token acquired in the authentication/authorization request form|Return cell local token or transcell token|
|token_type|Bearer||
|expires_in|Expiration date of "access_token"|1 hour (3600 seconds)|
|state|Value of state set at the time of request|Random value used to maintain state between request and callback|

##### Error Messages

|Item Name|Overview|Notes|
|:--|:--|:--|
|redirect_uri|Redirect URL|The URL of the client's redirect split<br>specified by the "redirect_uri" of the request<br>However, in the case of the following error contents, this value is set to "cell URL + __html/error"<br>"Redirect_uri is not in URL format" "cell in client_id and redirect_uri is different"|
|error|Code indicating error content|See "error"|
|error_description|Additional information on errors|Set exception message etc|
|error_uri|Web page URI of additional information on error|Return empty string<br>* Set for future enhancemen|
|state|Value of state set at the time of request||
|code|Personium error code||
|invalid_request|A required parameter is not specified in the request<br>Invalid request parameter format<br>Account locked||
|unauthorized_client|The client is not authorized<br>Cancel button pressed by user||
|access_denied|The cells of client_id and redirect_uri are different<br>When transcell token authentication fails||
|unsupported_response_type|Invalid value of response_type||
|server_error|Server Error||
|Please, input user ID and password.|"Username" or "password" has not been entered||
|User ID or password is incorrect.|When password authentication fails||
|Since the Expiration Date of the authentication passed,<br>You must be authorized again.|Cookie authentication failed||

##### Parameter Check Error

The browser is redirected to redirect\_uri.<br>
"Redirect\_uri is not in URL format" "cell in client\_id and redirect\_uri is different" "Authorization processing failure"

```
{redirect_uri}?code={code}
```

Other than those above

```
{redirect_uri}#error={error}&error_description={error_description}&state={state}&code={code}
```


### cURL Command

#### GET

```sh
curl "https://{UnitFQDN}/{CellName}/__authz?response_type=token&redirect_uri=https://{UnitFQDN}/{AppliCellName}/__/redirect.html&client_id=https://{UnitFQDN}/{AppliCellName}" -X GET -i
```

#### POST

```sh
curl "https://{UnitFQDN}/{CellName}/__authz" -X POST -i -d 'response_type=token&client_id=https://{UnitFQDN}/{AppliCellName}&redirect_uri=https://{UnitFQDN}/{AppliCellName}/__/redirect.html&state=0000000111&username={AccountUserName}&password={AccountUserPass}'
```


###### Copyright 2017 FUJITSU LIMITED

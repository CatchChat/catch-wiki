# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

#### 账号和密码认证

发送登录账号 (login) 和密码 (password)，可获取相应的 access_token。login 可以为邮箱或用户名。

```
POST /api/auth/token_by_login
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| phone\_code | 国家代码 |
| password | 密码 |
| expiring | access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认一年过期 |
| client | 用于推送, official=0, company=1, local=2 |

cURL 请求范例：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"12345678","password":"ruanwztest", "client": 2}'
park.catchchatchina.com/api/v1/auth/token_by_login'
```

返回范例：

```
{ 
  "login":"86-12345678",
  "access_token":"76si--vSz-hW-3vAsmRG1422601273.349557"
}
```

cURL 请求范例（1小时过期）：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"12345678","password":"ruanwztest", "client": 2, "expiring": 3600}'
park.catchchatchina.com/api/v1/auth/token_by_login'
```

如果用户名密码错误，则返回 HTTP 401.
{
  error: "用户名密码错误"
}

#### 手机号和验证码认证

**发送验证码到指定手机号。**

对于1个手机号，3600秒内只发送20次，24小时内只发送50次。

```
POST /api/auth/send_verify_code
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| phone\_code | 国家代码 |


cURL 请求范例：

```
curl -X POST   -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"12345678"}'
http://park.catchchatchina.com/api/v1/auth/send_verify_code
```

返回范例：

```
{
  "mobile":"12345678",
  "status":"sms sent"
}
```

如果发送次数过多，则返回 429 Too Many Requests 的错误。

```
HTTP/1.1 403 Too Many Requests
Date: Tue, 20 Aug 2013 14:50:41 GMT
Status: 429 Too Many Requests

{
  error: "发给手机号 xxxxxxxxxxx 的验证码数量已经超过限制"
}
```

**发送手机号 (mobile) 和验证码 (verify_code)，可获取相应的 access_token。**

```
POST /api/auth/token_by_mobile
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| verify_code | 验证码 |
| expiring | access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认7天过期 |
| client | 用于推送, official=0, company=1, local=2, 默认是official |

cURL 请求范例：

```
curl -X POST   -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"12345678", "verify_code": "23397"}'
http://park.catchchatchina.com/api/v1/auth/token_by_mobile
```

返回范例：

```
{
  "access_token":"Bs2H_1hVWLseG7rDy5hz1422602902.8925965",
  "user": {
    "id":1,
    "nickname":"ruanwz",
    "avatar_url":null,
    "mobile":"12345678",
    "phone_code":"86"
  }
}
```

cURL 请求范例（1小时过期）：

```

curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"12345678", "verify_code": "23397", "expiring": 3600}'
http://park.catchchatchina.com/api/v1/auth/token_by_mobile
```

如果手机号和验证码错误，或验证码已经过期，则返回 HTTP 401.
{
  error: "手机号或验证码错误"
}

### 发送用户名,手机号码，发起注册,等待接收手机验证码

```
POST   /api/v1/registration/create
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| nickname | 用户昵称 |
| phone_code | 国家码 |

cURL 请求范例：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"15626044835", "nickname": "testnick"}'
http://park.catchchatchina.com/api/v1/registration/create
```

返回范例：

```
{
  "nickname":"testnick",
  "mobile":"15626044835",
  "sent_sms": "{\"error\":0,\"msg\":\"ok\"}",
  "state":"blocked"
}
```

### 重发送用户名,手机号码，发起注册,等待接收语音验证码

```
POST   /api/v1/registration/resend_verify_code_by_voice
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| nickname | 用户昵称 |
| phone_code | 国家码 |

cURL 请求范例：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"15626044835", "nickname": "testnick"}'
http://park.catchchatchina.com/api/v1/registration/resend_verify_code_by_voice
```

返回范例：

```
{
  "nickname":"testnick",
  "mobile":"15626044835",
  "sent_sms": "{\"error\":0,\"msg\":\"ok\"}",
  "state":"blocked"
}
```

### 发手机验证码完成注册

```
PUT   /api/v1/registration/update
```
| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| phone\_code | 国家码 |
| token | 手机短信收到的验证码 |
| client |可选，用于推送, official=0, company=1, local=2 ，默认为0|
| expiring | 可选，access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认一年过期 |

cURL 请求范例：
```
 curl -X PUT -H "Content-Type: application/json" -d '{"phone_code":"86","mobile":"15626044835", "token": 70215}' http://park.catchchatchina.com/api/v1/registration/update

```
返回范例：
```
{
  "access_token":"DAo4Cs4dkaE-7ADS-63Q1422604371.509334",
  "user":{
    "id":12,
    "nickname":"testnick",
    "avatar_url":null,
    "mobile":"15626044835",
    "phone_code":"86",
    "state":"active"
  }
}
```
### 用 Access Token 调用其他 API

现在你可以通过 **access_token** 来调用其他 API 了，比如：

```
GET /api/v1/xxx
```

cURL 请求范例：

```
curl https://park.catchchatchina.com/api/v1/xxx  -H 'Authorization: Token token="p7DvqB4MoT5ux-B1xg"'
```

如果 access_token 不存在或过期了，则返回 HTTP 401

HTTP Token: Access denied.


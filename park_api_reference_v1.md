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
| longitude | longitude |
|latitude | latitude |

cURL 请求范例：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"15626044835", "nickname": "testnick", "latitude": 123.123, "longitude": 23.23}'
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

## Circle 公共群组

### 获取加入了的所有公共群组

```
GET /api/v1/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| page | Integer | 否 | 当然页码 |
| pre_page | Integer | 否 | 每页显示数量 |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v1/circles -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{
  "circles":[
    {
      "id":1,
      "name":"circle",
      "created_at":"2015-01-29T09:43:35Z",
      "created_at_string":"2015年01月29日 09:43:35",
      "updated_at":"2015-01-29T09:43:35Z",
      "updated_at_string":"2015年01月29日 09:43:35",
      "owner":{
        "id":1,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      },
      "members":[
        {
          "id":1,
          "username":"tumayun",
          "nickname":"tumayun",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        }
        .
        .
        .
      ]
    },
    {
      "id":2,
      "name":"公共群组",
      "created_at":"2015-01-29T09:44:39Z",
      "created_at_string":"2015年01月29日 09:44:39",
      "updated_at":"2015-01-29T09:50:49Z",
      "updated_at_string":"2015年01月29日 09:44:39",
      "owner":{
        "id":1,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      },
      "members":[
        {
          "id":1,
          "username":"tumayun",
          "nickname":"tumayun",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        }
        .
        .
        .
      ]
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":2
}
```

### 创建公共群组

```
POST /api/v1/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| members | String | 否 | 成员ID数组 |

#### 示例

```
curl -i -X POST https://park.catchchatchina.com/api/v1/circles -d '{ "members": [3,4,5], "name": "群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"群组",
  "created_at":"2015-01-29T09:44:38Z",
  "created_at_string":"2015年01月29日 09:44:38",
  "updated_at":"2015-01-29T09:44:38Z",
  "updated_at_string":"2015年01月29日 09:44:38",
  "owner":{
    "id":3,
    "username":"friend1"
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
  	.
  	.
  	.
  ]
}
```

### 更新公共群组

```
PUT /api/v1/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X PUT https://park.catchchatchina.com/api/v1/circles/2 -d '{ "name": "公共群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```

#### 获取单个公共群组

```
GET /api/v1/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v1/circles/2 -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"'
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```
I
#### 加入公共群组

```
POST /api/v1/circles/:id/join
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/circles/2/join -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":1,
    "username":"tumayun",
    "nickname":"tumayun",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":1,
      "username":"tumayun",
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg""
    },
    .
    .
    .
  ]
}
```

### 退出公共群组

```
DELETE /api/v1/circles/:id/leave
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v1/circles/2/leave -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{}
```

### 批量添加成员

```
POST /api/v1/circles/:id/batch_add
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要添加的成员ID数组 |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/circles/2/batch_add -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```

### 批量删除成员（只能是群 owner 才能删除成员）

```
DELETE /api/v1/circles/:id/batch_delete
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要删除的成员ID数组 |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v1/circles/2/batch_delete -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```

### 公共群组未读消息

```
GET /api/v1/circles/:id/unread_messages
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET http://park.catchchatchina.com/api/v1/circles/2/unread_messages -H 'Authorization: Token token="QVXyAg6mpi14YDje9RSr1421656248.3836942"'
```

#### 响应

```
{
  "messages":[
    {
      "id":4,
      "recipient_id":2,
      "recipient_type":"Circle",
      "text_content":"Hello~",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"image",
      "media_type_string":"一张照片",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-29T10:58:07Z",
      "created_at_string":"2015年01月29日 10:58:07",
      "updated_at":"2015-01-29T10:58:27Z",
      "updated_at_string":"2015年01月29日 10:58:07",
      "sender":{
        "id":1,
        "username":"tumayun"
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        "remarked_name":"",
        "contact_name":""
      },
      "circle":{
        "id":2,
        "name":"公共群组",
        "created_at":"2015-01-29T09:44:39Z",
        "created_at_string":"2015年01月29日 09:44:39",
        "updated_at":"2015-01-29T09:50:49Z",
        "updated_at_string":"2015年01月29日 09:44:39"
      },
      "attachments":[
        {
          "kind":"image",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        },
        {
          "kind":"thumbnail",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        }
      ]
    },
    {
      "id":5,
      "recipient_id":2,
      "recipient_type":"Circle",
      "text_content":"Hello~",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"image",
      "media_type_string":"一张照片",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-29T11:00:07Z",
      "created_at_string":"2015年01月29日 11:00:07",
      "updated_at":"2015-01-29T11:02:51Z",
      "updated_at_string":"2015年01月29日 11:00:07",
      "sender":{
        "id":1,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        "remarked_name":"",
        "contact_name":""
      },
      "circle":{
        "id":2,
        "name":"公共群组",
        "created_at":"2015-01-29T09:44:39Z",
        "created_at_string":"2015年01月29日 09:44:39",
        "updated_at":"2015-01-29T09:50:49Z",
        "updated_at_string":"2015年01月29日 09:44:39"
      },
      "attachments":[
        {
          "kind":"image",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        },
        {
          "kind":"thumbnail",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        }
      ]
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":2
}
```

## User 个人信息 API

### 可能认识的好友

```
GET /api/v1/user/may_know_friends
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v1/user/may_know_friends -H Authorization: Token token="kuH3PbRifgSATCanYwxd1418031570.162303"'
```

#### 响应

```
{
  "friends":[
    {
      "id":8,
      "username":"friend6",
      "nickname":"friend6",
      "phone_code":"86",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "common_friend_names":[
        "friend2",
        "friend3",
        "friend4",
        "friend5"
      ]
    },
    {
      "id":9,
      "username":"friend7",
      "nickname":"friend7",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "common_friend_names":[
        "friend3",
        "friend4",
        "friend5"
      ]
    },
    {
      "id":10,
      "username":"friend8",
      "nickname":"friend8",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "common_friend_names":[
        "friend4",
        "friend5"
      ]
    }
  ]
}
```

### 获取个人信息

```
GET /api/v1/user
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v1/user -H 'Authorization: Token oken="kuH3PbRifgSATCanYwxd1418031570.162303"'
```

#### 响应

```
{
  "id":"d2cfb112ffcec856ad34a3e933ed64b0",
  "username":"user5",
  "nickname":"user5",
  "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
  "phone_code":"86",
  "mobile":"15158160005",
  "pusher_id":"c0db0793fb939e96f2574cb4",
  "state":"active",
  "state_string":"激活状态",
  "latitude":28.2534,
  "longitude":117.068,
  "last_sign_in_at":"2015-03-24T06:01:18Z",
  "last_sign_in_at_string":"2015年03月24日 06:01:18",
  "learning_skills":[
    "ruby",
    "javascript",
    "ios",
    "linux",
    "css",
    "cooking",
    "boxing",
    "drawing"
  ],
  "master_skills":[
    {
      "name":"singing",
      "level":"beginner"
    },
    {
      "name":"piano",
      "level":"beginner"
    },
    {
      "name":"dancing",
      "level":"beginner"
    }
  ]
}
```

### 更新个人信息

```
PATCH /api/v1/user
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| nickname | String | 否 | 昵称 |
| avatar_url | String | 否 | 头像 URL |
| username | String | 否 | 用户名，必须唯一 |
| latitude | Float | 否 | 纬度 |
| longitude | Float | 否 | 经度 |

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/user -F username=tumayun -F latitude=26.331920 -F longitude=168.3097112 -F nickname=Tumayun -F avatar_url=http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{
  "nickname":"Tumayun",
  "username":"tumayun",
  "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
  "latitude":26.331920,
  "longitude":168.3097112
}
```

### 更新手机号

```
PATCH /api/v1/user/update_mobile
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 手机号国家码，详见 [http://countrycode.org/](ttp://countrycode.org/) |
| mobile | String | 是 | 手机号 |
| token | String | 是 | 短信验证码 |

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/user/update_mobile -F phone_ode=86 -F mobile=15158166372 -F token=131421 -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{
  "phone_code":"86",
  "mobile":"15158166372"
}
```

### Discover 发现

```
GET /api/:version/user/discover
```

如果传入`master_skills`和`learning_skills`参数，则按照传入参数匹配目标用户；
如果未传参数，则用当前用户想要学习的技能作为`master_skills`，当前用户已有的技能作为`learning_skills`，匹配出最近目标用户。
默认按照搜索引擎计算的 score 排序（越精准，越靠前），另外支持按照 distance（距离）和 last_sign_in_at（最后在线时间）排序。

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| master_skills | JSON Array | 否 | 匹配用户已有技能，如果不传，则默认为自己想学的技能 |
| learning_skills | JSON Array | 否 | 匹配用户想要学习的技能，如果不传，则默认为自己已有的技能 |
| sort | String | 否 | 排序字段，目前支持 distance 和 last_sign_in_at，默认按照搜索的 score 排序 |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v1/user/discover -d '{ "master_skills": ["ruby"], "learning_skills": ["singing"] }' -H 'Authorization: Token token="xfNa4ZYEQKLynYgbAfHB1427176878.549557"' -H 'Content-Type: application/json'
```

#### 响应

```
{
  "users":[
    {
      "id":"058555a359257b6d1b9e85ec425096b7",
      "username":"user4",
      "nickname":"user4",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "latitude":0.0,
      "longitude":0.0,
      "distance":"12646.761054269287",
      "last_sign_in_at":"2015-03-24T05:43:05Z",
      "last_sign_in_at_string":"2015年03月24日 05:43:05",
      "learning_skills":[
        "ruby",
        "javascript",
        "ios",
        "linux",
        "css",
        "cooking",
        "boxing",
        "drawing"
      ],
      "master_skills":[
        {
          "name":"singing",
          "level":"beginner"
        },
        {
          "name":"piano",
          "level":"beginner"
        },
        {
          "name":"dancing",
          "level":"beginner"
        }
      ]
    },
    {
      "id":"8d35d33fdd000da12ff35887daa7d428",
      "username":"user1",
      "nickname":"user1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "latitude":0.0,
      "longitude":0.0,
      "distance":"12646.761054269287",
      "last_sign_in_at":"2015-03-24T05:43:03Z",
      "last_sign_in_at_string":"2015年03月24日 05:43:03",
      "learning_skills":[
        "ruby",
        "javascript",
        "ios",
        "linux",
        "css",
        "cooking",
        "boxing",
        "drawing"
      ],
      "master_skills":[
        {
          "name":"singing",
          "level":"beginner"
        },
        {
          "name":"piano",
          "level":"beginner"
        },
        {
          "name":"dancing",
          "level":"beginner"
        }
      ]
    },
    {
      "id":"3cd8d8de0b3f732f4103cbd428f8ec92",
      "username":"tumayun",
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "latitude":28.3213,
      "longitude":117.001,
      "distance":"9.995235609898296",
      "last_sign_in_at":"2015-03-24T05:43:02Z",
      "last_sign_in_at_string":"2015年03月24日 05:43:02",
      "learning_skills":[
        "ruby",
        "javascript",
        "ios",
        "linux",
        "css",
        "cooking",
        "boxing",
        "drawing"
      ],
      "master_skills":[
        {
          "name":"singing",
          "level":"beginner"
        },
        {
          "name":"piano",
          "level":"beginner"
        },
        {
          "name":"dancing",
          "level":"beginner"
        }
      ]
    },
    {
      "id":"ff5405d3bdad237c7f572a58f65517a6",
      "username":"ruanwz",
      "nickname":"ruanwz",
      "avatar_url":null,
      "latitude":26.9211,
      "longitude":118.969,
      "distance":"238.93350644977318",
      "last_sign_in_at":"2015-03-24T05:43:01Z",
      "last_sign_in_at_string":"2015年03月24日 05:43:01",
      "learning_skills":[
        "ruby",
        "javascript",
        "ios",
        "linux",
        "css",
        "cooking",
        "boxing",
        "drawing"
      ],
      "master_skills":[
        {
          "name":"singing",
          "level":"beginner"
        },
        {
          "name":"piano",
          "level":"beginner"
        },
        {
          "name":"dancing",
          "level":"beginner"
        }
      ]
    },
    {
      "id":"6cf2c0fae5fb7048175fd80b8cc842a5",
      "username":"user3",
      "nickname":"user3",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "latitude":0.0,
      "longitude":0.0,
      "distance":"12646.761054269287",
      "last_sign_in_at":"2015-03-24T05:43:04Z",
      "last_sign_in_at_string":"2015年03月24日 05:43:04",
      "learning_skills":[
        "ruby",
        "javascript",
        "ios",
        "linux",
        "css",
        "cooking",
        "boxing",
        "drawing"
      ],
      "master_skills":[
        {
          "name":"singing",
          "level":"beginner"
        },
        {
          "name":"piano",
          "level":"beginner"
        },
        {
          "name":"dancing",
          "level":"beginner"
        }
      ]
    }
  ]
  "current_page":1,
  "per_page":30,
  "count":5
}
```

## Circle 公共群组

### 获取加入了的所有公共群组

```
GET /api/v1/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| page | Integer | 否 | 当然页码 |
| pre_page | Integer | 否 | 每页显示数量 |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v1/circles -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{
  "circles":[
    {
      "id":1,
      "name":"circle",
      "created_at":"2015-01-29T09:43:35Z",
      "created_at_string":"2015年01月29日 09:43:35",
      "updated_at":"2015-01-29T09:43:35Z",
      "updated_at_string":"2015年01月29日 09:43:35",
      "owner":{
        "id":1,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      },
      "members":[
        {
          "id":1,
          "username":"tumayun",
          "nickname":"tumayun",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        }
        .
        .
        .
      ]
    },
    {
      "id":2,
      "name":"公共群组",
      "created_at":"2015-01-29T09:44:39Z",
      "created_at_string":"2015年01月29日 09:44:39",
      "updated_at":"2015-01-29T09:50:49Z",
      "updated_at_string":"2015年01月29日 09:44:39",
      "owner":{
        "id":1,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      },
      "members":[
        {
          "id":1,
          "username":"tumayun",
          "nickname":"tumayun",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        }
        .
        .
        .
      ]
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":2
}
```

### 创建公共群组

```
POST /api/v1/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| members | String | 否 | 成员ID数组 |

#### 示例

```
curl -i -X POST https://park.catchchatchina.com/api/v1/circles -d '{ "members": [3,4,5], "name": "群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"群组",
  "created_at":"2015-01-29T09:44:38Z",
  "created_at_string":"2015年01月29日 09:44:38",
  "updated_at":"2015-01-29T09:44:38Z",
  "updated_at_string":"2015年01月29日 09:44:38",
  "owner":{
    "id":3,
    "username":"friend1"
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
  	.
  	.
  	.
  ]
}
```

### 更新公共群组

```
PUT /api/v1/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X PUT https://park.catchchatchina.com/api/v1/circles/2 -d '{ "name": "公共群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```

#### 获取单个公共群组

```
GET /api/v1/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v1/circles/2 -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"'
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```
I
#### 加入公共群组

```
POST /api/v1/circles/:id/join
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/circles/2/join -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":1,
    "username":"tumayun",
    "nickname":"tumayun",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":1,
      "username":"tumayun",
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg""
    },
    .
    .
    .
  ]
}
```

### 退出公共群组

```
DELETE /api/v1/circles/:id/leave
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v1/circles/2/leave -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{}
```

### 批量添加成员

```
POST /api/v1/circles/:id/batch_add
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要添加的成员ID数组 |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/circles/2/batch_add -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```

### 批量删除成员（只能是群 owner 才能删除成员）

```
DELETE /api/v1/circles/:id/batch_delete
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要删除的成员ID数组 |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v1/circles/2/batch_delete -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
```

#### 响应

```
{
  "id":2,
  "name":"公共群组",
  "created_at":"2015-01-29T09:44:39Z",
  "created_at_string":"2015年01月29日 09:44:39",
  "updated_at":"2015-01-29T09:50:49Z",
  "updated_at_string":"2015年01月29日 09:44:39",
  "owner":{
    "id":3,
    "username":"friend1",
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  },
  "members":[
    {
      "id":3,
      "username":"friend1",
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    },
    .
    .
    .
  ]
}
```

### 公共群组未读消息

```
GET /api/v1/circles/:id/unread_messages
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET http://park.catchchatchina.com/api/v1/circles/2/unread_messages -H 'Authorization: Token token="QVXyAg6mpi14YDje9RSr1421656248.3836942"'
```

#### 响应

```
{
  "messages":[
    {
      "id":4,
      "recipient_id":2,
      "recipient_type":"Circle",
      "text_content":"Hello~",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"image",
      "media_type_string":"一张照片",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-29T10:58:07Z",
      "created_at_string":"2015年01月29日 10:58:07",
      "updated_at":"2015-01-29T10:58:27Z",
      "updated_at_string":"2015年01月29日 10:58:07",
      "sender":{
        "id":1,
        "username":"tumayun"
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        "remarked_name":"",
        "contact_name":""
      },
      "circle":{
        "id":2,
        "name":"公共群组",
        "created_at":"2015-01-29T09:44:39Z",
        "created_at_string":"2015年01月29日 09:44:39",
        "updated_at":"2015-01-29T09:50:49Z",
        "updated_at_string":"2015年01月29日 09:44:39"
      },
      "attachments":[
        {
          "kind":"image",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        },
        {
          "kind":"thumbnail",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        }
      ]
    },
    {
      "id":5,
      "recipient_id":2,
      "recipient_type":"Circle",
      "text_content":"Hello~",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"image",
      "media_type_string":"一张照片",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-29T11:00:07Z",
      "created_at_string":"2015年01月29日 11:00:07",
      "updated_at":"2015-01-29T11:02:51Z",
      "updated_at_string":"2015年01月29日 11:00:07",
      "sender":{
        "id":1,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        "remarked_name":"",
        "contact_name":""
      },
      "circle":{
        "id":2,
        "name":"公共群组",
        "created_at":"2015-01-29T09:44:39Z",
        "created_at_string":"2015年01月29日 09:44:39",
        "updated_at":"2015-01-29T09:50:49Z",
        "updated_at_string":"2015年01月29日 09:44:39"
      },
      "attachments":[
        {
          "kind":"image",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        },
        {
          "kind":"thumbnail",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400, // 单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        }
      ]
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":2
}
```

## Contact 通讯录

### 上传通讯录并返回已注册的通讯录好友

**覆盖式上传，上传后会删除之前的通讯录，返回已注册的通讯录好友，并且自动添加好友**

```
POST /api/v1/contacts/upload
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| contacts | JSON | 是 | contacts JSON format, 如 "[{\"name\":\"abc\",\"number\":\"15158166372\"},{\"name\":\"bac\",\"number\":\"15158166723\"}]"|

#### 示例

```
curl htts://park.catchchatchina.com/api/v1/contacts/upload -F contacts="[{\"name\":\"abc\",\"number\":\"15158160004\"},{\"name\":\"bac\",\"number\":\"15158160005\"}]" -H 'Authorization: Token token="sVNxda9nywMLZkuzUqf31422601654.468095"'
```

#### 响应

```
{
  "registered_contacts":[
    {
      "id":1,
      "name":"abc",
      "user":{
        "id":13,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "nickname":"user4",
        "username":"user4",
      }
    },
    {
      "id":2,
      "name":"bac",
      "user":{
        "id":14,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "nickname":"user5",
        "username":"user5",
      }
    }
  ]
}
```

## Users 用户

### 搜索用户

```
GET api/v1/users/search
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| q | String | 是 | mobile 或者 username |

#### 示例

```
curl https://park.catchchatchina.com/api/v1/users/search\?q\=15158161111 -H 'Authorization: Token token="EtErCK18xN9pxakiCPp61418029033.582837"'
```

#### 响应

```
{
  "users":[
    {
      "id":8,
      "username":"tumayun",
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 校验手机号是否可用（无需登录）

```
GET api/v1/users/mobile_validate
```
** phone code 合法, mobile 合法，且具有唯一性    
在设置中修改手机号时，不能拿当前手机号去校验，否则会返回手机号已经被使用**

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 国家码 |
| mobile | String | 是 | 手机号 |

#### 示例

```
curl https://park.catchchatchina.com/api/v1/users/mobile_validate\?phone_code\=86\&mobile\=15158166372
```

#### 响应

* 手机号可用

```
{  
  "available":true
}
```

* 手机号不可用

```
{  
  "available":false,
  "message":"手机号已经被使用"
}
```

## Message 消息

### 获取所有未读消息

```
GET /api/v1/messages/unread
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v1/messages/unread -H 'Authorization: Token token="nH-CaGbGvS5tJRizTsiM1418019414.813717"' -k
```

#### 响应

```
{
  "messages":[
    {
      "id":3,
      "recipient_id":11,
      "recipient_type":"User",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"text",
      "media_type_string":"一段文字",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "sender":{
        "id":8,
        "username":"Tumayun",
        "nickname":"Tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
        "remarked_name":null,
        "contact_name":null
      },
      "attachments":[

      ]
    },
    {
      "id":4,
      "recipient_id":2,
      "recipient_type":"Circle",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"text",
      "media_type_string":"一段文字",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "sender":{
        "id":8,
        "username":"Tumayun",
        "nickname":"Tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
        "remarked_name":"",
        "contact_name":""
      },
      "circle":{
        "id":2,
        "name":"circle",
        "created_at":"2015-01-30T07:07:19Z",
        "created_at_string":"2015年01月30日 07:07:19",
        "updated_at":"2015-01-30T07:07:19Z",
        "updated_at_string":"2015年01月30日 07:07:19"
      },
      "attachments":[

      ]
    },
    {
      "id":5,
      "recipient_id":2,
      "recipient_type":"Circle",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":null,
      "latitude":null,
      "battery_level":50,
      "media_type":"image",
      "media_type_string":"一张照片",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-01-30T09:29:29Z",
      "created_at_string":"2015年01月30日 09:29:29",
      "updated_at":"2015-01-30T09:32:04Z",
      "updated_at_string":"2015年01月30日 09:29:29",
      "sender":{
        "id":8,
        "username":"Tumayun",
        "nickname":"Tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
        "remarked_name":"",
        "contact_name":""
      },
      "circle":{
        "id":2,
        "name":"circle",
        "created_at":"2015-01-30T07:07:19Z",
        "created_at_string":"2015年01月30日 07:07:19",
        "updated_at":"2015-01-30T07:07:19Z",
        "updated_at_string":"2015年01月30日 07:07:19"
      },
      "attachments":[
        {
          "kind":"thumbnail",
          "file":{
            "storage":"qiniu",
            "expires_in":86400,
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400,
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        },
        {
          "kind":"image",
          "file":{
            "storage":"qiniu",
            "expires_in":86400,
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          },
          "fallback_file":{
            "storage":"s3",
            "expires_in":86400,  //单位：秒
            "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
          }
        }
      ]
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":3
}
```

### 获取收到的单条消息

```
GET /api/v1/messages/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 消息 ID |

#### 示例

```
curl https://park.catchchatchina.com/api/v1/messages/4 -H 'Authorization: Token token="nH-CaGbGvS5tJRizTsiM1418019414.813717"'
```

#### 响应

```
{
  "id":5,
  "recipient_id":2,
  "recipient_type":"Circle",
  "text_content":"This is a test!",
  "parent_id":0,
  "longitude":null,
  "latitude":null,
  "battery_level":50,
  "media_type":"image",
  "media_type_string":"一张照片",
  "state":"unread",
  "state_string":"未读",
  "created_at":"2015-01-30T09:29:29Z",
  "created_at_string":"2015年01月30日 09:29:29",
  "updated_at":"2015-01-30T09:32:04Z",
  "updated_at_string":"2015年01月30日 09:29:29",
  "sender":{
    "id":8,
    "username":"Tumayun",
    "nickname":"Tumayun",
    "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
    "remarked_name":"",
    "contact_name":""
  },
  "circle":{
    "id":2,
    "name":"circle",
    "created_at":"2015-01-30T07:07:19Z",
    "created_at_string":"2015年01月30日 07:07:19",
    "updated_at":"2015-01-30T07:07:19Z",
    "updated_at_string":"2015年01月30日 07:07:19"
  },
  "attachments":[
    {
      "kind":"thumbnail",
      "file":{
        "storage":"qiniu",
        "expires_in":86400,
        "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
      },
      "fallback_file":{
        "storage":"s3",
        "expires_in":86400,
        "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
      }
    },
    {
      "kind":"image",
      "file":{
        "storage":"qiniu",
        "expires_in":86400,
        "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
      },
      "fallback_file":{
        "storage":"s3",
        "expires_in":86400,
        "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/test-key?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAOGBVMZAU5EZPGPIQ%2F20141208%2Fcn-north-1%2Fs3%2Faws4_request&X-Amz-Date=20141208T065428Z&X-Amz-Expires=86400&X-Amz-Signature=c2f80c4d07452ef937488139ef99aaec8ef00c77dd49e5464ab2609b9e1118f5&X-Amz-SignedHeaders=Host"
      }
    }
  ]
}
```

### 新建消息

```
POST /api/v1/messages
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
recipient_id | Integer | 是 | 接收者 ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
recipient_type | String | 是 | 接受者类型，只能是 User 或者 Circle
media_type | Integer | 否 | 消息类型，0 表示文字, 1 表示图片, 2表示视频, 3 表示语音, 4 表示贴纸, 5 表示位置, 默认是文字
text_content | String | 否 | 文字内容，**只有是文字消息时才是必填字段，其他情况都是选填字段**
parent_id | Integer | 否 | 回复的消息 ID，表示当前要发送的消息是回复哪条消息
longitude | Float | 否 | 经度
latitude | Float | 否 | 纬度
battery_level | Integer | 否 | 电量，0 - 100 之间的值，默认为 50

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/messages -F recipient_id=2 -F recipient_type=User -F text_content='This is a test!' -F battery_level=70 -H 'Authorization: Token token="NDccv1Yvdi9UKtwPToxx1416921006.674603"'
```

#### 响应

```
{
  "id":3,
  "battery_level":70,
  "state":"unread",
  "state_string":"未读",
  "media_type":"text",
  "media_type_string":"一段文字"
}
```

### 标记消息为已读

```
POST /api/v1/messages/:id/mark_as_read
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | 消息 ID

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/messages/3/mark_as_read -H 'Authorization: Token token="TKWsindneiDsFj3gUHs31416969554.7962759"'
```

#### 响应

```
{}
```


### 标记消息已收到

```
PATCH /api/v1/messages/:id/deliver
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | 消息 ID

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/messages/3/deliver -H 'Authorization: Token token="TKWsindneiDsFj3gUHs31416969554.7962759"'
```

#### 响应

```
{}
```

### 通知消息被截图保存

```
POST /api/v1/messages/:id/notify_screenshot
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | 消息 ID

#### 示例

curl -X POST https://park.catchchatchina.com/api/v1/messages/3/notify_screenshot -H 'Authorization: Token token="TKWsindneiDsFj3gUHs31416969554.7962759"'

#### 响应

```
{}
```

## Report 消息举报

### 举报

```
POST /api/v1/reports
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| message_id | Integer | 是 | 要举报的消息 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/reports -F message_id=1 -H 'Authorization: Token token="6Y62Cc4FQ1GVw3ZiLqzq1418393864.8241951"'
```

#### 响应

```
{}
```

## FriendRequest 好友请求

### 获取当前用户收到的好友请求

按创建时间倒序排

```
GET /api/friend_requests/received/:state
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
state | String | 是 | `:state` 是状态，可选：`pending`, `accepted`, `rejected`, `blocked`

#### 示例

```
curl https://catchchatserver.com/api/v4/friend_requests/received/accepted -H 'Authorization: Token token=DR-2ye_k9HzcMHZKEaHe1416646800.2446852'
```

#### 响应

```
{
  "friend_requests":[
    {
      "id":3,
      "user_id":1,
      "friend_id":3,
      "state":2,
      "state_string":"已接受",
      "created_at":"2014-11-22T17:08:07Z",
      "created_at_string":"2014年11月22日 17:08:07",
      "updated_at":"2014-11-22T17:09:42Z",
      "updated_at_string":"2014年11月22日 17:08:07",
      "friend":{
        "id":1,
        "username":"ruanwztest",
        "nickname":"ruanwztest",
        "name":"ruanwztest", // remarked_name || contact_name || nickname || username
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      }
    }
    .
    .
    .
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```


### 获取当前用户发送的好友请求

按创建时间倒序排

```
GET /api/friend_requests/sent/:state
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
state | String | 是 | `:state` 是状态，可选：`pending`, `accepted`, `rejected`, `blocked`

#### 示例

```
curl https://park.catchchatchina.com/api/v1/friend_requests/sent/accepted -H 'Authorization: Token token="s96vf4aWCz6i3nzwaS_Z1422855236.287788"'
```

#### 响应

```
{
  "friend_requests":[
    {
      "id":9,
      "user_id":8,
      "friend_id":12,
      "state":"accepted",
      "state_string":"已接受",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":12,
        "username":"user3",
        "nickname":"user3",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "contact_name":null
      }
    },
    {
      "id":8,
      "user_id":8,
      "friend_id":11,
      "state":"accepted",
      "state_string":"已接受",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":11,
        "username":"user2",
        "nickname":"user2",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "contact_name":null
      }
    },
    {
      "id":7,
      "user_id":8,
      "friend_id":10,
      "state":"accepted",
      "state_string":"已接受",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":10,
        "username":"user1",
        "nickname":"user1",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "contact_name":null
      }
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":3
}
```

### 新建好友请求

```
POST /api/friend_requests
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
friend_id | Integer | 是 | 要添加的好友ID

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/friend_requests -F friend_id=8 -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
```

#### 响应

```
{
  "id":7,
  "user_id":8,
  "friend_id":10,
  "state":"pending",
  "state_string":"等待中",
  "created_at":"2015-01-30T07:07:19Z",
  "created_at_string":"2015年01月30日 07:07:19",
  "updated_at":"2015-01-30T07:07:19Z",
  "updated_at_string":"2015年01月30日 07:07:19",
  "friend":{
    "id":10,
    "username":"user1",
    "nickname":"user1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "contact_name":null
  }
}
```

### 同意好友请求

```
PATCH /api/friend_requests/received/:id/accept
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friend_request ID

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/friend_requests/received/7/accept  -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
```

#### 响应

```
{
  "id":7,
  "user_id":8,
  "friend_id":10,
  "state":"accepted",
  "state_string":"已接受",
  "created_at":"2015-01-30T07:07:19Z",
  "created_at_string":"2015年01月30日 07:07:19",
  "updated_at":"2015-01-30T07:07:19Z",
  "updated_at_string":"2015年01月30日 07:07:19",
  "friend":{
    "id":10,
    "username":"user1",
    "nickname":"user1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "contact_name":null
  }
}
```

### 拒绝好友请求

```
PATCH /api/friend_requests/received/:id/reject
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friend_request ID

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/friend_requests/received/7/reject -H 'Authorization: Token token=XVVP8GpgezByNjhjWaEs1416648757.346672'
```

#### 响应

```
{
  "id":7,
  "user_id":8,
  "friend_id":10,
  "state":"rejected",
  "state_string":"已拒绝",
  "created_at":"2015-01-30T07:07:19Z",
  "created_at_string":"2015年01月30日 07:07:19",
  "updated_at":"2015-01-30T07:07:19Z",
  "updated_at_string":"2015年01月30日 07:07:19",
  "friend":{
    "id":10,
    "username":"user1",
    "nickname":"user1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "contact_name":null
  }
}
```

### 禁止好友请求

```
PATCH /api/friend_requests/received/:id/block
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friend_request ID

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/friend_requests/received/7/block -H 'Authorization: Token token=XVVP8GpgezByNjhjWaEs1416648757.346672'
```

#### 响应

```
{
  "id":7,
  "user_id":8,
  "friend_id":10,
  "state":"blocked",
  "state_string":"已禁止",
  "created_at":"2015-01-30T07:07:19Z",
  "created_at_string":"2015年01月30日 07:07:19",
  "updated_at":"2015-01-30T07:07:19Z",
  "updated_at_string":"2015年01月30日 07:07:19",
  "friend":{
    "id":10,
    "username":"user1",
    "nickname":"user1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "contact_name":null
  }
}
```

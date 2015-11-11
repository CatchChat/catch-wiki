# Yep API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

### 分页

默认返回30条记录，可以通过 `per_page` 参数指定最多每次返回100条记录，但是有些 API 因为一些技术原因，会忽略 `per_page` 参数。页码用 `?page` 参数指定，从1开始。
在`参数`中不再明确列出 `page` 和 `per_page` 参数，返回结果中包含 `current_page/per_page/count`，就表示支持分页功能。

cURL 请求范例：

```
curl https://www.catchchatserver.com/api/friendships?page=2&per_page=100
```

<!---
### 请求速率

1小时支持5000次请求，你能通过 HTTP Header 了解到请求速率的限制：

cURL 请求范例：

```
curl -i https://www.catchchatserver.com/api/friendships
```

返回范例：

| Header 字段 | 描述 |
|---|---|
| X-RateLimit-Limit | 1小时内最大请求数 |
| X-RateLimit-Remaining | 在时间窗口内，剩余的请求数 |
| X-RateLimit-Reset | 请求窗口重置的时间，UTC epoch seconds 表示 |

```
HTTP/1.1 200 OK
Date: Thu, 30 Oct 2014 16:27:06 GMT
Status: 200 OK
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4999
X-RateLimit-Reset: 1372700873
```

超过请求速率后，你会收到一个 429 Too Many Requests 的错误。

```
HTTP/1.1 403 Too Many Requests
Date: Tue, 20 Aug 2013 14:50:41 GMT
Status: 429 Too Many Requests
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1377013266

{
    error: "来自 xxx.xxx.xxx.xxx 的 API 请求已经超过限制"
}
```
--->

### Header

所有 API 请求都需要包含如下 headers:

| header | 说明 |
|--------|--------|
| Accept-Language | 当前客户端所使用的语言，目前只支持 `en-US` 和 `zh-CN`，无效的设置将默认为 `en-US` |

### Response

HTTP Code 大于等于 `200` 且小于 `300` 表示请求成功，反之则请求失败，失败会有错误信息，错误信息在 `error` 中。

### 字段模板

为了方便 API 文档的阅读，将会用模板重用一些重复的字段结构。

#### ID

所有返回和提交的 ID 都是加密后的字符串("516055075accc1e4067dd5ff6b2682cd")，在 API 中用到 ID 时，都将以 `<id>` 替代。

#### 技能字段结构

在 API 返回技能信息时，将会以 `<skill>` 替代如下结构：

```
"id":<id>,
"name":"Singing",        // 技能名
"name_string":"Singing", // 技能名翻译
"cover_url":null,        // 技能封面图片URL
```

在 API 返回技能信息时，将会以 `<skill_with_category>` 替代如下结构：

```
<skill>
"category":{
  "id":<id>,
  "name":"Art",          // 类别名
  "name_string":"Art"    // 类别翻译
}
```

#### 用户基本信息字段结构

在 API 返回基本的用户信息时，将会以 `<mini_user>` 替代如下结构：

```
"id":<id>,
"username":"asdaasd",
"nickname":"user9",
"avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
"latitude":0.0,
"longitude":0.0,
"introduction":"",
"badge":null,
"created_at":1441347808,
"updated_at":1441693888,
"last_sign_in_at":1441347780
```

#### 用户信息字段结构

在 API 返回用户信息时，将会以 `<user>` 替代如下结构：

```
"id":<id>,
"username":"tumayun",
"nickname":"tumayun",
"avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
"latitude":28.3213,
"longitude":117.001,
"introduction":"",
"badge":"apple",
"last_sign_in_at":1433930183, // UNIX 时间戳
"created_at":1433930183, // UNIX 时间戳
"updated_at":1433930183, // UNIX 时间戳
"providers":{ // 第三方平台绑定情况
  "github":false,
  "dribbble":false,
  "instagram":false
},
"master_skills":[
  {
    <skill_with_category>
  },
  ...
],
"learning_skills":[
  {
    <skill_with_category>
  },
  ...
]
```

#### 公共群组字段模板

在 API 返回公共群组信息时，将会以 `<circle>` 替代如下结构：

```
"id":<id>,
"topic_id":<id>
"name":"circle",
"active":true, // 是否允许发送消息
"created_at":1433930183, // UNIX 时间戳
"updated_at":1433930183, // UNIX 时间戳
```

在 API 返回公共群组信息时，将会以 `<circle_with_topic>` 替代如下结构：

```
<circle>
"topic":{
  <topic>
}
```

#### Topic 字段模板

在 API 返回 topic 信息时，将会以 `<topic>` 替代如下结构：

```
"id": <id>,
"allow_comment": true, // 是否允许评论
"kind": "normal" // 帖子类型，目前有 normal|apple_music|apple_movie|apple_ebook
"body": "test", // 帖子内容
"message_count": 0, // 评论消息数
"created_at": 1443278450.465,
"updated_at": 1443278450.465
"user":{ // 帖子创建者
  <mini_user>
},
"skill":{ // 注意：skill 可能为 null
  <skill>
},
"circle":{ // 注意：circle 可能为 null
  <circle>
},
"apple_media":{ // 注意：只有 kind 为 apple_music|apple_movie|apple_ebook 才会有 apple_media，并且 apple_media 的类型对应为 music|movie|ebook
  "title":"Upside Down",
  "description":"Sing-a-Longs and Lullabies for the Film Curious George",
  "poster":"https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewAlbum?i=120954025&id=120954021&s=143441",
  "media_url":"http://a1.itunes.apple.com/r10/Music/3b/6a/33/mzi.qzdqwsel.100x100-75.jpg",
  "preview_url":"http://a1099.itunes.apple.com/r10/Music/f9/54/43/mzi.gqvqlvcq.aac.p.m4p",
  "time_millis":210743
},
"attachments":[
  <attachment>,
  .
  .
  .
]
```

#### Attachment 字段模板

在 API 返回 attachment 信息时，将会以 `<attachment>` 替代如下结构：

```
"kind":"image",
"metadata":"metadata",
"file":{
  "storage":"qiniu",
  "expires_in":86400, // 单位：秒
  "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
}
```

#### Message 字段模板

在 API 返回 message 信息时，将会以 `<message>` 替代如下结构：

```
{
  "id":<id>,
  "recipient_id":<id>,
  "recipient_type":"Circle",
  "text_content":"Hello~",
  "latitude":113.033,
  "longitude":24.1231,
  "parent_id":0,
  "media_type":"image",
  "media_type_string":"一张照片",
  "state":"unread",
  "state_string":"未读",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "sender":{
    <mini_user>,
    "remarked_name":null,
    "contact_name":null
  },
  "attachments":[
    <attachment>,
    .
    .
    .
  ]
}
```

### 推送

推送 extras 说明

#### 新消息

**新消息推送已经支持 `category` 参数，参数值为 `YepMessageNotification`，方便用户直接在推送中回复消息。**

| key | value |
|--------|--------|
| type | message |
| subtype | 消息的 media_type 值，可能值为：`text`， `image`， `video`， `audio`， `sticker`， `location` |
| recipient_id | 接收者（聊天对象） ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
| recipient_type | 接受者（聊天对象）类型，只能是 User 或者 Circle |

#### 消息撤回

| key | value |
|--------|--------|
| type | message_deleted |
| message | { "id":<id>, "recipient_id":<id>, "recipient_type":"User", "sender":{ "id":<id>, "username":"tumayun", "nickname":"涂马云" } } |

#### 官方消息

| key | value |
|--------|--------|
| type | official_message |
| subtype | 消息的 media_type 值消息的 media_type 值，可能值为：`text`， `image`， `video`， `audio`， `sticker`， `location` |

#### 有通讯录好友加入

| key | value |
|--------|--------|
| type | contact_join |

#### 好友请求

| key | value |
|--------|--------|
| type | friend_request |
| subtype | 可能的值为 `pending`， `accepted`， `rejected`，分别表示有好友请求，好友请求被接受，好友请求被拒绝 |

----

## 注册登录

### 手机号和验证码认证

**发送验证码到指定手机号。**

对于1个手机号，1小时内只发送20次，24小时内只发送50次。

```
POST /api/v2/sms_verification_codes
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| phone_code | 国家代码 |
| method | 发送方式，`sms` 短信方式，`call` 语音方式 |

cURL 请求范例：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86","mobile":"12345678","method":"sms"}' http://park.catchchatchina.com/api/v2/sms_verification_codes
```

返回范例：

```
{}
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
| phone_code | 国家代码 |
| verify_code | 验证码 |
| expiring | access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认7天过期 |
| client | 用于推送, official=0, company=1, local=2, 默认是official |

cURL 请求范例：

```
curl -X POST   -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"12345678", "verify_code": "23397"}'
http://park.catchchatchina.com/api/v2/auth/token_by_mobile
```

返回范例：

```
{
  "access_token":"Bs2H_1hVWLseG7rDy5hz1422602902.8925965",
  "user": {
    "id":<id>,
    "username":"ruanwz",
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
http://park.catchchatchina.com/api/v2/auth/token_by_mobile
```

如果手机号和验证码错误，或验证码已经过期，则返回 HTTP 401.
{
  error: "手机号或验证码错误"
}

### 发送用户名,手机号码，发起注册,等待接收手机验证码

```
POST   /api/v2/registration/create
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| nickname | 用户昵称 |
| phone_code | 国家码 |
| longitude | longitude |
| latitude | latitude |

cURL 请求范例：

```
curl -X POST -H "Content-Type: application/json" -d '{"phone_code":"86",
"mobile":"15626044835", "nickname": "testnick", "latitude": 123.123, "longitude": 23.23}'
http://park.catchchatchina.com/api/v2/registration/create
```

返回范例：

```
{
  "username":"testnick",
  "nickname":"testnick",
  "mobile":"15626044835",
  "phone_code":"86"
  "state":"blocked"
  "sent_sms": true, // true 表示验证码发送正常，false 表示验证码发送失败
}
```

### 验证手机验证码完成注册

```
PUT   /api/v2/registration/update
```
| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| phone_code | 国家码 |
| token | 手机短信收到的验证码 |
| client |可选，用于推送, official=0, company=1, local=2 ，默认为0|
| expiring | 可选，access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认一年过期 |

cURL 请求范例：
```
 curl -X PUT -H "Content-Type: application/json" -d '{"phone_code":"86","mobile":"15626044835", "token": 70215}' http://park.catchchatchina.com/api/v2/registration/update

```
返回范例：
```
{
  "access_token":"DAo4Cs4dkaE-7ADS-63Q1422604371.509334",
  "user":{
    "id":<id>,
    "username":"testnick",
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
GET /api/v2/xxx
```

cURL 请求范例：

```
curl https://park.catchchatchina.com/api/v2/xxx  -H 'Authorization: Token token="p7DvqB4MoT5ux-B1xg"'
```

如果 access_token 不存在或过期了，则返回 HTTP 401

HTTP Token: Access denied.

## Circle 公共群组

### 获取加入了的所有公共群组

* 此 API 不会返回群组成员，客户端在用户进入某个群的聊天界面后，请求 `获取单个公共群组` API，可以拿到群组信息以及成员信息 *

```
GET /api/v2/circles
```

#### 参数

无

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v2/circles -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{
  "circles":[
    {
      <circle_with_topic>,
    }
    .
    .
    .
  ],
  "current_page":1,
  "per_page":30,
  "count":2
}
```

### 创建公共群组

```
POST /api/v2/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| members | JSON | 否 | 成员ID数组 |

#### 示例

```
curl -i -X POST https://park.catchchatchina.com/api/v2/circles -d '{ "members": [3,4,5], "name": "群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
```

#### 响应

```
{
  <circle_with_topic>,
  "owner":{
    <mini_user>
  },
  "members":[
    {
      <mini_user>
    },
    .
    .
    .
  ]
}
```

### 更新公共群组

```
PUT /api/v2/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X PUT https://park.catchchatchina.com/api/v2/circles/2 -d '{ "name": "公共群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
```

#### 响应

```
{
  <circle_with_topic>,
  "owner":{
    <mini_user>
  },
  "members":[
    {
      <mini_user>
    },
    .
    .
    .
  ]
}
```

### 获取单个公共群组

```
GET /api/v2/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v2/circles/2 -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"'
```

#### 响应

```
{
  <circle_with_topic>,
  "owner":{
    <mini_user>
  },
  "members":[
    {
      <mini_user>
    },
    .
    .
    .
  ]
}
```

### 加入公共群组

```
POST /api/v2/circles/:id/join
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/circles/2/join -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{
  <circle_with_topic>,
  "owner":{
    <mini_user>
  },
  "members":[
    {
      <mini_user>
    },
    .
    .
    .
  ]
}
```

### 退出公共群组

```
DELETE /api/v2/circles/:id/leave
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v2/circles/2/leave -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

不返回数据，只返回状态码

### 批量添加成员

```
POST /api/v2/circles/:id/batch_add
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要添加的成员ID数组 |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/circles/2/batch_add -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
```

#### 响应

```
{
  <circle_with_topic>,
  "owner":{
    <mini_user>
  },
  "members":[
    {
      <mini_user>
    },
    .
    .
    .
  ]
}
```

### 批量删除成员（只能是群 owner 才能删除成员）

```
DELETE /api/v2/circles/:id/batch_delete
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要删除的成员ID数组 |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v2/circles/2/batch_delete -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
```

#### 响应

```
{
  <circle_with_topic>,
  "owner":{
    <mini_user>
  },
  "members":[
    {
      <mini_user>
    },
    .
    .
    .
  ]
}
```

### 获取分享链接

```
POST /api/v2/circles/:id/share
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/circles/516055075accc1e4067dd5ff6b2682cd/share -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{ url: 'http://soyep.com/groups/share?token=qRLyR3jBQslCQlgADuevpwlEDLBTm28vI-t1eAioHvg=' }
```

### 获取分享的群组消息

**无需登录，获取分享时最近的 25 条消息，支持 jsonp**

```
GET /api/v2/circles/shared_messages
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| token | String | 是 | 分享链接带的 token 值 |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v2/circles/shared_messages?token=qRLyR3jBQslCQlgADuevpwlEDLBTm28vI-t1eAioHvg=
```

#### 响应

```
{
  "circle":{
    <circle>
  },
  "topic":{
    <topic>
  },
  "messages":[
    <message>
  ]
}
```

### 当前用户设置指定群组为免打扰

```
POST /api/v2/circles/:id/dnd
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -XPOST https://park.catchchatchina.com/api/v2/circles/2/dnd -H 'Authorization: Token token="test-token"'
```

#### 响应

只返回状态码

### 当前用户取消设置指定群组为免打扰

```
DELETE /api/v2/circles/:id/dnd
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -XDELETE https://park.catchchatchina.com/api/v2/circles/2/dnd -H 'Authorization: Token token="test-token"'
```

#### 响应

只返回状态码

### 获取当前用户对指定群组的免打扰设置

```
GET /api/v2/circles/:id/dnd
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -XGET https://park.catchchatchina.com/api/v2/circles/2/dnd -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{ "dnd": true } // dnd 为 true 表示已设置为免打扰，false 表示未设置为免打扰
```

## User 个人信息 API

### 可能认识的好友

```
GET /api/v2/user/may_know_friends
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/user/may_know_friends -H Authorization: Token token="kuH3PbRifgSATCanYwxd1418031570.162303"'
```

#### 响应

```
{
  "friends":[
    {
      <mini_user>,
      "common_friend_names":[
        "friend2",
        "friend3",
        "friend4",
        "friend5"
      ]
    },
    .
    .
    .
  ]
}
```

### 获取个人信息

```
GET /api/v2/user
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/user -H 'Authorization: Token oken="kuH3PbRifgSATCanYwxd1418031570.162303"'
```

#### 响应

```
{
  <user>,
  "push_content":true,
  "phone_code":"86",
  "mobile":"15158161111",
  "pusher_id":"439ee7d09180529d3442bd25",
  "state":"active",
  "state_string":"激活状态",
  "mute_started_at_string":"23:30", // 防打扰开始时间, UTC 时间，需要转换成客户端当前时区后再显示
  "mute_ended_at_string":"07:30" // 防打扰结束时间, UTC 时间，需要转换成客户端当前时区后再显示
}
```

### 更新个人信息

`mute_started_at_string` 和 `mute_ended_at_string` 都有值时，勿扰功能开启，都为空时，勿扰功能关闭。

```
PATCH /api/v2/user
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| nickname | String | 否 | 昵称 |
| avatar_url | String | 否 | 头像 URL |
| username | String | 否 | 用户名，必须唯一（忽略大小写），只能是由字母、数字、下划线组成，长度为4到16个字符 |
| latitude | Float | 否 | 纬度 |
| longitude | Float | 否 | 经度 |
| push_content | Boolean | 否 | 标识推送时是推送消息内容还是推送通知，true 推送消息内容，false 推送通知 |
| introduction | Text | 否 | 个人介绍 |
| badge | String | 否 | 徽章，有：android apple ball bubble camera game heart music palette pet plane star steve tech wine |
| mute_started_at_string | String | 否 | 防打扰开始时间，如：23:30，UTC 时间 |
| mute_ended_at_string | String | 否 | 防打扰结束时间，如：07:30，UTC 时间 |

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/user -F badge=apple -F username=tumayun -F latitude=26.331920 -F longitude=168.3097112 -F nickname=Tumayun -F avatar_url=http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg -F push_content=false -F mute_started_at_string=23:30 -F mute_ended_at_string=07:30 -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{
  <user>,
  "push_content":true,
  "phone_code":"86",
  "mobile":"15158161111",
  "pusher_id":"439ee7d09180529d3442bd25",
  "state":"active",
  "state_string":"激活状态",
  "mute_started_at_string":"23:30", // 防打扰开始时间，UTC 时间
  "mute_ended_at_string":"07:30" // 防打扰结束时间，UTC 时间
}
```

### 更新手机号流程

1. 发送当前手机号验证码 (POST /api/v2/sms_verification_codes)
2. 校验当前手机号验证码  (PATCH /api/v2/user/check_verify_code)
3. 发送新手机号验证码 (POST /api/v2/user/send_update_mobile_code)
4. 校验新手机号验证码，通关验证后更新手机号为新手机好 (PATCH /api/v2/user/update_mobile)

### 验证更新手机号请求的验证码

```
PATCH /api/v2/user/check_verify_code
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| token | String | 是 | 短信验证码 |

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/user/check_verify_code -F token=1234 -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{}
```

### 发送新手机号验证码

```
POST /api/v2/user/send_update_mobile_code
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 国家码 |
| mobile | String | 是 | 手机号 |
| method | String | 是 | 发送方式，`sms` 短信方式，`call` 语音方式 |

#### 示例

```
curl -X POST -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"' -H "Content-Type: application/json" -d '{"phone_code":"86","mobile":"12345678","method":"sms"}' https://park.catchchatchina.com/api/v2/user/send_update_mobile_code
```

#### 响应

```
{}
```

### 更新手机号

```
PATCH /api/v2/user/update_mobile
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 手机号国家码，详见 [http://countrycode.org/](ttp://countrycode.org/) |
| mobile | String | 是 | 手机号 |
| token | String | 是 | 短信验证码 |

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/user/update_mobile -F phone_ode=86 -F mobile=15158166372 -F token=131421 -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{}
```

### Discover 发现

```
GET /api/v2/user/discover
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
curl -X GET https://park.catchchatchina.com/api/v2/user/discover -d '{ "master_skills": [<id>], "learning_skills": [<id>] }' -H 'Authorization: Token token="xfNa4ZYEQKLynYgbAfHB1427176878.549557"' -H 'Content-Type: application/json'
```

#### 响应

```
{
  "users":[
    {
      <mini_user>,
      "distance":12646.8,
      "recently_updated_provider":"github", // 最近更新的 provider，值为 github | dribbble | instagram
      "master_skills":[
        <skill>,
        .
        .
        .
      ]
    },
    .
    .
    .
  ],
  "current_page":1,
  "per_page":30,
  "count":5
}
```

### 获取 provider 信息（github instagram dribbble）

```
GET /api/v2/user/:provider
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| provider | String | 是 | github instagram dribbble 中的一个 |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/user/github -H 'Authorization: Token token="test-token"'
```

#### 响应

github instagram dribbble 返回各不一样，从各平台拿到数据后原样返回，所以请参考各平台 API

## Contact 通讯录

### 上传通讯录并返回已注册的通讯录好友

**覆盖式上传，上传后会删除之前的通讯录，返回已注册的通讯录好友，并且自动添加好友**

```
POST /api/v2/contacts/upload
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| contacts | JSON | 是 | contacts JSON format, 如 "[{\"name\":\"abc\",\"number\":\"15158166372\"},{\"name\":\"bac\",\"number\":\"15158166723\"}]"|

#### 示例

```
curl htts://park.catchchatchina.com/api/v2/contacts/upload -F contacts="[{\"name\":\"abc\",\"number\":\"15158160004\"},{\"name\":\"bac\",\"number\":\"15158160005\"}]" -H 'Authorization: Token token="sVNxda9nywMLZkuzUqf31422601654.468095"'
```

#### 响应

```
{
  "registered_users":[
    {
      "contact_name":"bac",
      <mini_user>,
      distance: 1234.5
    },
    .
    .
    .
  ]
}
```

## Users 用户

### 搜索用户

```
GET api/v2/users/search
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| q | String | 是 | mobile|username|nickname，其中 username 和 nickname 是前缀匹配，mobile 则是全文匹配  |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/users/search\?q\=15158161111 -H 'Authorization: Token token="EtErCK18xN9pxakiCPp61418029033.582837"'
```

#### 响应

```
{
  "users":[
    {
      <mini_user>
      distance: 10.1 // 距离，单位 km
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 校验手机号是否可用（无需登录）

```
GET api/v2/users/mobile_validate
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
curl https://park.catchchatchina.com/api/v2/users/mobile_validate\?phone_code\=86\&mobile\=15158166372
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

### 获取指定用户的 provider 信息（github instagram dribbble）

```
GET /api/v2/users/:id/:provider
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | String | 是 | user id |
| provider | String | 是 | github instagram dribbble 中的一个 |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/users/90913b93738c8a627129e49db32eeec3/github -H 'Authorization: Token token="test-token"'
```

#### 响应

github instagram dribbble 返回各不一样，从各平台拿到数据后原样返回，所以请参考各平台 API

### 获取指定用户信息（by id）

```
GET /api/v2/users/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | String | 是 | user id |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/users/90913b93738c8a627129e49db32eeec3 -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  <user>
}
```

### 获取指定用户信息（by username）

**支持 JSONP，分享用户 profile 页专用，无需登录！**

```
GET /api/v2/users/:username/profile
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| username | String | 是 | username |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/users/tumayun/profile
```

#### 响应

```
{
  <user>,
  providers: {
    github: <provider_data>, // 格式同 provider_data API
    dribbble: <provider_data>, // 格式同 provider_data API
    instagram: <provider_data>  // 格式同 provider_data API
  }
}
```

### 获取指定用户与当前用户之间的设置

设置包括:
1. 当前用户是否屏蔽指定用户
2. 当前用户是否将指定用户设置为免打扰

```
GET /api/v2/users/:id/settings_with_current_user
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | String | 是 | user id |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/users/90913b93738c8a627129e49db32eeec3/settings_with_current_user -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  blocked: true,   // 当前用户是否 block 了 user<90913b93738c8a627129e49db32eeec3>
  dnd: true        // 当前用户是否将 user<90913b93738c8a627129e49db32eeec3> 设置为请勿打扰了
}
```

### 当前登录用户设置指定用户为免打扰

```
POST /api/v2/users/:id/dnd
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | String | 是 | user id |

#### 示例

```
curl -XPOST https://park.catchchatchina.com/api/v2/users/90913b93738c8a627129e49db32eeec3/dnd -H 'Authorization: Token token="test-token"'
```

#### 响应

只返回状态码

### 当前登录用户取消设置指定用户为免打扰

```
DELETE /api/v2/users/:id/dnd
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | String | 是 | user id |

#### 示例

```
curl -XDELETE https://park.catchchatchina.com/api/v2/users/90913b93738c8a627129e49db32eeec3/dnd -H 'Authorization: Token token="test-token"'
```

#### 响应

只返回状态码

### 获取擅长指定技能的用户

```
GET api/v2/master_skills/:skill_id/users
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/master_skills/90913b93738c8a627129e49db32eeec3/users -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  "users":[
    {
      <mini_user>
      distance: 10.1 // 距离，单位 km
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 获取正在学习指定技能的用户

```
GET api/v2/learning_skills/:skill_id/users
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/learning_skills/90913b93738c8a627129e49db32eeec3/users -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  "users":[
    {
      <mini_user>
      distance: 10.1 // 距离，单位 km
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

## Message 消息

### 获取最新未读消息

每个聊天窗口至多返回10条未读消息，进入聊天窗口后想要获取更多消息可以请求消息历史API

```
GET /api/v2/messages/unread
```

#### 参数

**无，且不支持分页**

#### 示例

```
curl https://park.catchchatchina.com/api/v2/messages/unread -H 'Authorization: Token token="nH-CaGbGvS5tJRizTsiM1418019414.813717"'
```

#### 响应

```
{
  "conversations":[
    {
      "conversation_type":"User" // 聊天窗口类型，值可能为 User 或者 Circle，分别表示单聊和群聊
      "conversation":{ // 根据 conversation_type 的不同，可能是 user 或者 circle
        <mini_user>|<circle_with_topic>,
      },
      "messages":[    // 未读消息
        <message>
      ],
      "count":100     // 未读总数
    }
  ]
}
```

### 发送消息

```
POST /api/v2/:recipient_type/:recipient_id/messages
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
recipient_id | String | 是 | 接收者（聊天对象）ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
recipient_type | String | 是 | 接受者（聊天对象）类型，只能是 users 或者 circles
media_type | String | 否 | 消息类型，text 表示文字, image 表示图片, video 表示视频, audio 表示语音, sticker 表示贴纸, location 表示位置, 默认是text
text_content | String | 否 | 文字内容，**只有是文字消息时才是必填字段，其他情况都是选填字段**
parent_id | Integer | 否 | 回复的消息 ID，表示当前要发送的消息是回复哪条消息
attachments | JSON | 否 | 如：{ "image": [{ "file": "3e1b14f1-ee42-471e-96c2-2c46459f13c4", "metadata": "metadata" }], "thumbnail": [{ "file": "99e3c1b0-adfe-4a35-b4e9-aee1117d9c6c", "metadata": "metadata" }] }
longitude | Float | 否 | 只有位置消息才需要
latitude | Float | 否 | 只有位置消息才需要

media_type 可选值：

可选值 | 解释
--- | --- |
text | 文字消息
image | 图片消息
video | 视频消息
audio | 语音消息
sticker | 贴纸消息
location | 位置消息

attachments 中 `file` 表示 S3 返回的文件 key，`metadata` 是附件的元数据。
attachments key 的可选值：

可选值 | 解释
--- | --- |
image | 附件一张图片
thumbnail | 附件一张缩略图
audio | 附件是一段声音
video | 附件是一段视频
sticker | 附件是一副贴纸

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/users/<id>/messages -d '{ "text_content": "This is a test!", "media_type": "image", "attachments": { "image": [{ "file": "3e1b14f1-ee42-471e-96c2-2c46459f13c4", "metadata": "metadata" }], "thumbnail": [{ "file": "99e3c1b0-adfe-4a35-b4e9-aee1117d9c6c", "metadata": "metadata" }] } }' -H 'Authorization: Token token="NDccv1Yvdi9UKtwPToxx1416921006.674603"' -H "Content-Type: application/json"
```

#### 响应

```
<message>
```

### 标记指定聊天窗口的多条消息为已读

将 max_id 提交上去后，会将此聊天窗口中在 max_id 之前的所有消息标记为已读。单聊群聊都适用，唯一的区别是单聊会通过 faye server 发送已读通知给消息的发送者，而群聊不会。

faye server 的已读确认消息结构如下：

```
// 客户端可以通过 recipient_id 和 recipient_type 确定聊天窗口，然后将聊天窗口我发送消息中的 `created_at` 小于等于 `last_read_at` 的都标记为已读。
{
  message_type: 'mark_as_read',
  message: {
    "last_read_at":1445596604.144, // max_id 对应消息的 created_at 值
    "recipient_id":<id>,
    "recipient_type":"User"
  }
}
```

```
PATCH /api/v2/:recipient_type/:recipient_id/messages/batch_mark_as_read
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
recipient_id | String | 是 | 接收者（聊天对象） ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID
recipient_type | String | 是 | 接受者（聊天对象）类型，只能是 users 或者 circles
max_id | String | 是 | 最新读取的消息 ID

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/users/<id>/messages/batch_mark_as_read -H 'Authorization: Token token="test-token"' -F max_id=<id>
```

#### 响应

{ "last_read_at":1445596604.144 }

### 撤回已发送消息

撤回成功会发送`message_deleted`类型的消息到 faye server，客户端收到消息后需要删除消息，faye server 消息体如下：

```
{
  message_type: 'message_deleted',
  message: {
    "id":<id>,
    "recipient_id":<id>,
    "recipient_type":"User",
    "sender":{
      "id":<id>,
      "username":"tumayun",
      "nickname":"涂马云"
    }
  }
}
```

并且有推送发出，客户端收到推送后需要删除消息，推送的`extras`为：

```
{
  type: 'message_deleted',
  message: {
    "id":<id>,
    "recipient_id":<id>,
    "recipient_type":"User",
    "sender":{
      "id":<id>,
      "username":"tumayun",
      "nickname":"涂马云"
    }
  }
}
```

```
DELETE /api/v2/messages/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 消息 ID |

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v2/messages/<id> -H 'Authorization: Token token="yChytb7mKMbs5EZPK8jp1435855393.5408268"'
```

#### 响应

```
{
  "id":<id>,
  "recipient_id":<id>,
  "recipient_type":"User",
  "sender":{
    "id":<id>,
    "username":"tumayun",
    "nickname":"涂马云"
  }
}
```

### 获取指定聊天窗口的消息历史

**按创建时间倒序返回**

```
GET /api/v2/:recipient_type/:recipient_id/messages
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| recipient_id | String | 是 | 接收者（聊天对象） ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
| recipient_type | String | 是 | 接受者（聊天对象）类型，只能是 users 或者 circles |
| max_id | String | 否 | 将返回 `id` 比 `max_id` 小的未读消息，**在向上翻页的时候必须带上 max_id** |
| min_id | String | 否 | 将返回 `id` 比 `min_id` 大的未读消息，**在向下翻页的时候必须带上 min_id** |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v2/users/<id>/messages -H 'Authorization: Token token="yChytb7mKMbs5EZPK8jp1435855393.5408268"'
```

#### 响应

同`GET /api/v2/messages/unread`

### 获取指定聊天窗口的未读消息

**按创建时间倒序返回**

```
GET /api/:version/:recipient_type/:recipient_id/messages/unread
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| recipient_id | String | 是 | 接收者（聊天对象） ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
| recipient_type | String | 是 | 接受者（聊天对象）类型，只能是 users 或者 circles |
| max_id | String | 否 | 将返回 `id` 比 `max_id` 小的未读消息，**在向上翻页的时候必须带上 max_id** |
| min_id | String | 否 | 将返回 `id` 比 `min_id` 大的未读消息，**在向下翻页的时候必须带上 min_id** |

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v2/users/<id>/messages/unread -H 'Authorization: Token token="yChytb7mKMbs5EZPK8jp1435855393.5408268"'
```

#### 响应

```
{
  "messages":[
    <message>
  ],
  "current_page": 1,
  "per_page": 25,
  "count": 100
}
```

### 获取我在指定聊天窗口发送的消息被对方最后读取的时间

用获取到的最后读取时间与发送消息的 `created_at` 对比，小于等于最后读取时间的消息全都标记为已读

```
GET /api/:version/:recipient_type/:recipient_id/messages/sent_last_read_at
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
| recipient_id | String | 是 | 接收者（聊天对象） ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
| recipient_type | String | 是 | 接受者（聊天对象）类型，只能是 users 或者 circles |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/users/<id>/messages/sent_last_read_at -H 'Authorization: Token token="nH-CaGbGvS5tJRizTsiM1418019414.813717"'
```

#### 响应

{ "last_read_at":1445596604.144 }

## FriendRequest 好友请求

### 获取当前用户收到的好友请求

按创建时间倒序排

```
GET /api/friend_requests/received/:state
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
state | String | 是 | `:state` 是状态，可选：`pending`, `accepted`, `rejected`

#### 示例

```
curl https://catchchatserver.com/api/v4/friend_requests/received/accepted -H 'Authorization: Token token=DR-2ye_k9HzcMHZKEaHe1416646800.2446852'
```

#### 响应

```
{
  "friend_requests":[
    {
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "state":2,
      "state_string":"已接受",
      "created_at":1433930183, // UNIX 时间戳
      "updated_at":1433930183, // UNIX 时间戳
      "friend":{
        <mini_user>,
        "contact_name":"contact_name"
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
state | String | 是 | `:state` 是状态，可选：`pending`, `accepted`, `rejected`

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friend_requests/sent/accepted -H 'Authorization: Token token="s96vf4aWCz6i3nzwaS_Z1422855236.287788"'
```

#### 响应

```
{
  "friend_requests":[
    {
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "state":2,
      "state_string":"已接受",
      "created_at":1433930183, // UNIX 时间戳
      "updated_at":1433930183, // UNIX 时间戳
      "friend":{
        <mini_user>,
        "contact_name":"contact_name"
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
curl -X POST https://park.catchchatchina.com/api/v2/friend_requests -F friend_id=8 -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
```

#### 响应

```
{
  "id":<id>,
  "user_id":<id>,
  "friend_id":<id>,
  "state":1,
  "state_string":"等待中",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "friend":{
    <mini_user>,
    "contact_name":"contact_name"
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
curl -X PATCH https://park.catchchatchina.com/api/v2/friend_requests/received/7/accept  -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
```

#### 响应

```
{
  "id":<id>,
  "user_id":<id>,
  "friend_id":<id>,
  "state":2,
  "state_string":"已接受",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "friend":{
    <mini_user>,
    "contact_name":"contact_name"
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
curl -X PATCH https://park.catchchatchina.com/api/v2/friend_requests/received/7/reject -H 'Authorization: Token token=XVVP8GpgezByNjhjWaEs1416648757.346672'
```

#### 响应

```
{
  "id":<id>,
  "user_id":<id>,
  "friend_id":<id>,
  "state":3,
  "state_string":"已拒绝",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "friend":{
    <mini_user>,
    "contact_name":"contact_name"
  }
}
```

### 查询当前用户是否发送或者接收过指定用户的好友请求

```
GET /api/v2/friend_requests/with_user/:user_id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
user_id | String | 是 | User ID

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friend_requests/with_user/ba994ac6dba5bc71489ab75fd5b8574c -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
   "current_user_blocked_by_specified_user":false, // 表示指定用户是否将当前用户列入黑名单
   "specified_user_blocked_by_current_user":false, // 表示当前用户是否将指定用户列入黑名单
   "friend":false,        // 表示当前用户与指定用户是否是好友关系
   "received":{           // 指定用户没有向当前用户发起过好友请求，则返回 null； 指定用户向当前用户发起过好友请求，则返回好友请求数据给客户端。
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "state":"pending",
      "state_string":"等待中",
      "updated_at":1439783441,
      "created_at":1439783441
   },
   "sent":"received":{    // 当前用户没有向指定用户发起过好友请求，则返回 null；指定用户向当前用户发起过好友请求，则返回好友请求数据给客户端。
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "state":"pending",
      "state_string":"等待中",
      "updated_at":1439783441,
      "created_at":1439783441
   }
}
```

## Friendship 好友关系

### 所有好友

按照 position 升序排列，最近联系过的好友越靠前 

```
GET /api/v2/friendships
```
#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friendships\?page\=1\&per_page\=10 -H 'Authorization: Token token=8ni89Uirfva2Zyyzq1ou1416506910.739525'
```

#### 响应

```
{
  "friendships":[
    {
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,           // 标示是否星组成员
      "position":1,
      "favored_position":null,
      "name":"user1",
      "created_at":1433930183, // UNIX 时间戳
      "updated_at":1433930183, // UNIX 时间戳
      "friend":{
        <mini_user>
      }
    },
    .
    .
    .
  ],
  "current_page":1,
  "per_page":10,
  "count":5
}
```

### 最近有联系的好友

最近三天有过联系的好友，包括发送消息给我的和我发过消息的好友

```
GET /api/v2/friendships/recent
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friendships/recent\?page\=1\&per_page\=10 -H 'Authorization: Token token=8ni89Uirfva2Zyyzq1ou1416506910.739525'
```

#### 响应

```
{
  "friendships":[
    {
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,           // 标示是否星组成员
      "position":1,
      "favored_position":null,
      "name":"user1",
      "created_at":1433930183, // UNIX 时间戳
      "updated_at":1433930183, // UNIX 时间戳
      "friend":{
        <mini_user>
      }
    },
    .
    .
    .
  ],
  "current_page":1,
  "per_page":10,
  "count":5
}
```

### 某一好友的详细信息(by friendship id)

```
GET /api/v2/friendships/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friendships/27 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":<id>,
  "user_id":<id>,
  "friend_id":<id>,
  "contact_name":"bac",
  "remarked_name":null,
  "favored":false,
  "position":5,
  "favored_position":null,
  "name":"bac",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "friend":{
    <mini_user>
  }
}
```

### 某一好友的详细信息(by friend id)

```
GET /api/v2/friendships/with/:friend_id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
friend_id | Integer | 是 | friend id

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friendships/with/14 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":<id>,
  "user_id":<id>,
  "friend_id":<id>,
  "contact_name":"bac",
  "remarked_name":null,
  "favored":false,
  "position":5,
  "favored_position":null,
  "name":"bac",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "friend":{
    <mini_user>
  }
}
```

### 更新好友信息

```
PATCH /api/v2/friendships/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id
remarked_name | String | 否 | 备注名
contact_name | String | 否 | 通讯录名

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/friendships/13 -F contact_name=contact_name -F remarked_name=remarked_name -H 'Authorization: Token token="sVNxda9nywMLZkuzUqf31422601654.468095"'
```

#### 响应

```
{
  "id":<id>,
  "user_id":<id>,
  "friend_id":<id>,
  "contact_name":"bac",
  "remarked_name":null,
  "favored":false,
  "position":5,
  "favored_position":null,
  "name":"bac",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "friend":{
    <mini_user>
  }
}
```

### 搜索好友

```
GET /api/friendships/search?q=keyword
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
q | String | 是 | remarked_name 或者 contact_name 或者 nickname 或者 mobile 或者 username

#### 示例

```
curl https://park.catchchatchina.com/api/v2/friendships/search\?q\=1515816 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "friendships":[
    {
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,           // 标示是否星组成员
      "position":1,
      "favored_position":null,
      "name":"user1",
      "created_at":1433930183, // UNIX 时间戳
      "updated_at":1433930183, // UNIX 时间戳
      "friend":{
        <mini_user>
      }
    },
    .
    .
    .
  ],
  "current_page":1,
  "per_page":10,
  "count":5
}
```

### 好友置顶

```
PATCH /api/v2/friendships/:id/move_to_top
```
#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/friendships/3/move_to_top -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{}
```
<!--
### 批量设置星组好友

```
PATCH /api/v2/friendships/batch_mark_as_favored
```

**按照提交的ID顺序覆盖式设置星组好友**

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
ids | Integer | 是 | friendship ids

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v2/friendships/batch_mark_as_favored -d '{ "ids": [3,5,1] }' -H "Content-Type: application/json" -H 'Authorization: Token token="wcdyPGTv3HqZ76vZt5VR1422869949.0730119"'
```

#### 响应

```
{
  "friendships":[
    {
      "id":<id>,
      "user_id":<id>,
      "friend_id":<id>,
      "contact_name":null,
      "remarked_name":null,
      "favored":true,           // 标示是否星组成员
      "position":1,
      "favored_position":1,
      "name":"user1",
      "created_at":1433930183, // UNIX 时间戳
      "updated_at":1433930183, // UNIX 时间戳
      "friend":{
        <mini_user>
      }
    },
    .
    .
    .
  ],
}
```
-->

## UnfriendRequest 解除好友请求

### 新建请求解除好友关系

```
POST /api/v2/unfriend_requests
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| friend_id | Integer | 是 | 好友 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/unfriend_requests -F friend_id=2 -H 'Authorization: Token token="yZp5UZMeCB8yKBUy_ae81416827431.104971"'
```

#### 响应

```
{}
```

## Attachment 附件

### 获取附件上传表单参数

获得S3上传所需的提交的form的参数

具体S3上传需要的格式可参考S3文档或Rails里的上传例子
[S3文档](http://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-post-example.html)

```
GET /api/v2/attachments/:kind/s3_upload_form_fields
```

kind 可能值为：

| kind | 说明 |
|---|---|
| message | 消息附件 |
| topic | feed 附件 |
| avatar | 头像 |

cURL 请求范例：
```
curl -X GET https://park.catchchatchina.com/api/v2/attachments/message/s3_upload_form_fields -H 'Authorization: Token token="DdPnWxQy6z5axwxZfsjs1427718553.7476008"'
```

返回范例：

```
{
  "provider":"s3",
  "options":{
    "bucket":"park-message-attachments",
    "key":"73d63ba7-d481-49b0-b78d-0a8f6e1dd031",
    "url":"https://park-message-attachments.s3.cn-north-1.amazonaws.com.cn/",
    "policy":{
      "expiration":"2015-03-30T13:30:44.000Z",
      "conditions":[
        {
          "bucket":"park-message-attachments"
        },
        {
          "key":"73d63ba7-d481-49b0-b78d-0a8f6e1dd031"
        },
        {
          "acl":"private"
        },
        {
          "x-amz-credential":"AKIAOGBVMZAU5EZPGPIQ/20150330/cn-north-1/s3/aws4_request"
        },
        {
          "x-amz-algorithm":"AWS4-HMAC-SHA256"
        },
        {
          "x-amz-date":"20150330T123044Z"
        }
      ]
    },
    "encoded_policy":"eyJleHBpcmF0aW9uIjoiMjAxNS0wMy0zMFQxMzozMDo0NC4wMDBaIiwiY29uZGl0aW9ucyI6W3siYnVja2V0IjoicGFyay1tZXNzYWdlLWF0dGFjaG1lbnRzIn0seyJrZXkiOiI3M2Q2M2JhNy1kNDgxLTQ5YjAtYjc4ZC0wYThmNmUxZGQwMzEifSx7ImFjbCI6InByaXZhdGUifSx7IngtYW16LWNyZWRlbnRpYWwiOiJBS0lBT0dCVk1aQVU1RVpQR1BJUS8yMDE1MDMzMC9jbi1ub3J0aC0xL3MzL2F3czRfcmVxdWVzdCJ9LHsieC1hbXotYWxnb3JpdGhtIjoiQVdTNC1ITUFDLVNIQTI1NiJ9LHsieC1hbXotZGF0ZSI6IjIwMTUwMzMwVDEyMzA0NFoifV19",
    "signature":"fd5fd80aabef5e59b60b0405726dd03958401d60da19cf135dcb9e9f307ac3fd"
  }
}
```

获取上传 form 所需参数后，上传到S3, 拿到 key，在创建消息的时候提交，详见 `POST messages` API

## Block User API（拒绝接收对方消息）


### 获取 Blocked Users

```
GET /api/v2/blocked_users
```

#### 参数

无

#### 示例

```
curl -X GET https://park.catchchatchina.com/api/v2/blocked_users -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{
  "blocked_users":[
    {
      <mini_user>
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### Block User

```
POST /api/v2/blocked_users
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
user_id | String | 是 | 想要 block 的用户 ID

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/blocked_users -F user_id=516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{}
```

### Unblock User

```
DELETE /api/v2/blocked_users/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | String | 是 | 想要 unblock 的用户 ID

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v2/blocked_users/516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{}
```

## Master Skills（已有的技能）

### 添加已有技能

```
POST /api/v2/master_skills
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
skill_id | String | 是 | 技能 ID

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/master_skills -F skill_id=516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{}
```

### 移除已有技能

```
DELETE /api/v2/master_skills/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | String | 是 | 技能 ID

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v2/master_skills/516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{}
```

## Learning Skills（想学的技能）

### 添加想学的技能

```
POST /api/v2/learning_skills
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
skill_id | String | 是 | 技能 ID

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/learning_skills -F skill_id=516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{}
```

### 移除想学的技能

```
DELETE /api/v2/master_skills/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | String | 是 | 技能 ID

#### 示例

```
curl -X DELETE https://park.catchchatchina.com/api/v2/learning_skills/516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{}
```

## 技能分类（Skill Category）

### 技能分类列表

```
GET /api/v2/skill_categories
```

#### 参数

无，且不支持分页，一次性返回所有分类以及技能

#### 示例

```
curl https://park.catchchatchina.com/api/v2/skill_categories -H 'Authorization: Token token="__6d1nbPEXM5-ycZdaHW1427949278.5644941"'
```

#### 响应

```
{
  "categories":[
    {
      "id":<id>,
      "name":"Art", // 分类名
      "name_string":"Art", // 分类名翻译
      "skills":[
        {
          "id":<id>,
          "name":"Drawing", // 技能名
          "name_string":"Drawing" // 技能名翻译
          "cover_url": "http://test.com" // 技能封面图片URL
        },
        .
        .
        .
      ]
    },
    .
    .
    .
  ]
}
```

## Skills（技能）

### 更新技能信息

```
PATCH /api/v2/skills/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | String | 是 | 技能 ID
cover_url | String | 否 | 技能封面图片地址

#### 示例

```
curl -X PATCH park-staging.catchchatchina.com/api/v2/skills/516055075accc1e4067dd5ff6b2682cd -F cover_url=https://s3.cn-north-1.amazonaws.com.cn/ruanwz-test-public/ruby-logo.png -H 'Authorization: Token token="test-token”'
```

#### 响应

```
{"id":<id>,"cover_url":"https://s3.cn-north-1.amazonaws.com.cn/ruanwz-test-public/ruby-logo.png"}
```
}

## 举报（reports）

### 举报用户

```
POST /api/v2/users/:id/reports
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | String | 是 | 要举报的用户 ID
report_type | Integer | 是 | 0 表示色情低俗, 1 表示广告骚扰, 2 表示诈骗, 3 表示其他
reason | Text | 否 | 举报原因，当 report_type 为 3 时，reason 为必填

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/users/bc93fe60a44cf376edeb98a9d68d85b9/reports -F report_type=1 -F reason=test -H 'Authorization: Token token="test-token"'
```

#### 响应

只返回 http code

### 举报话题

```
POST /api/v2/topics/:id/reports
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | String | 是 | 要举报的话题 ID
report_type | Integer | 是 | 0 表示色情低俗, 1 表示广告骚扰, 2 表示诈骗, 3 表示其他
reason | Text | 否 | 举报原因，当 report_type 为 3 时，reason 为必填

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/topics/bc93fe60a44cf376edeb98a9d68d85b9/reports -F report_type=1 -F reason=test -H 'Authorization: Token token="test-token"'
```

#### 响应

只返回 http code

## Oauth (绑定第三方平台账号)

### 流程

1. 客户端通过 WebView 发起请求`https://park.catchchatchina.com/auth/:provider?_tkn=test-token`，其中`_tkn`为`access_token`值
2. 客户端等待接收绑定是否成功的通知

### 判断绑定结果

在一系列跳转结束后

1. URL 为 `yep://auth/success`，则说明绑定成功，其余 URL 都表示绑定失败
2. URL 为 `yep://auth/failure`，绑定失败，且如果 URL 携带 `error` 参数，则表示失败原因

## OfficialMessage（官方消息）

### 流程

1. 进入后台 `https://park.catchchatchina.com/admin/official_message`
2. 创建一条官方消息
3. 创建成功会发送推送到所有客户端，推送的 `extras` 中 `type` 是 `official_message`，`sub_type` 是消息的 `media_type` 字段
4. 客户端收到推送后请求 `/api/v2/official_messages` API（注意：请求到的是最近两个月未读的官方消息）

### 获取官方消息

```
GET /api/v2/official_messages
```

#### 参数

无，且不支持分页

#### 示例

```
curl https://park.catchchatchina.com/api/v2/official_messages -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  "sender":{
    <mini_user>
  },
  "official_messages":[
    {
      "id":<id>,
      "media_type":"image",
      "media_type_string":"一张照片",
      "text_content":"Welcome~",
      "created_at":1434680681.435,
      "updated_at":1434680681.435,
      "attachments":[
        <attachment>,
        .
        .
        .
      ]
    }
  ]
}
```

## Feedback (用户反馈)

### 创建用户反馈

```
POST /api/v2/feedbacks
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
content | Text | 是 | 反馈内容
device_info | String | 是 | 设备信息

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v2/feedbacks -F content=test -F device_info=test -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{}
```

## Topics (Feeds)

### 获取所有我发的帖子

```
GET /api/v2/topics
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v2/topics -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  "count": 1,
  "current_page": 1,
  "per_page": 30,
  "topics": [
    {
      <topic>
    },
    .
    .
    .
  ]
}
```

### 发现帖子 (Feeds)

```
GET /api/v2/topics/discover
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| sort | String | 否 | 排序方式，目前支持 default|distance|time。default 表示按技能相关性排序，distance 表示按距离倒序展示，time 表示两周内的 topics 按发帖时间倒序展示。默认值为 default |
| skill_id | String | 否 | 技能ID，支持按技能过滤 |
| max_id | String | 否 | 仅当 sort 值为 time 时有效，表示 ID 获取小于 max_id 的话题。当带有 max_id 参数时，应该不传 page 参数或者始终置为 1 |

#### 示例

```
curl https://park.catchchatchina.com/api/v2/topics/discover -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  "count": 1,
  "current_page": 1,
  "per_page": 30,
  "topics": [
    {
      <topic>,
      distance: 245.3, // km
    },
    .
    .
    .
  ]
}
```

### 发布帖子

```
POST /api/v2/topics
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| kind | String | 是 | 帖子类型 |
| body | Text | 是 | 帖子内容 |
| latitude | Float | 是 | latitude |
| longitude | Float | 是 | longitude |
| allow_comment | Boolean | 否 | 是否允许评论，允许评论则会创建 circle，默认为 true |
| skill_id | String | 否 | 技能ID |
| attachments | JSON | 否 | 如：{ "image": [{ "file": "3e1b14f1-ee42-471e-96c2-2c46459f13c4", "metadata": "metadata" }], "thumbnail": [{ "file": "99e3c1b0-adfe-4a35-b4e9-aee1117d9c6c", "metadata": "metadata" }] }
| apple_media | JSON | 否 | {"title":"Upside Down","description":"Sing-a-Longs and Lullabies for the Film Curious George","poster":"http://a1.itunes.apple.com/r10/Music/3b/6a/33/mzi.qzdqwsel.100x100-75.jpg","media_url":"https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewAlbum?i=120954025&id=120954021&s=143441","preview_url":"http://a1099.itunes.apple.com/r10/Music/f9/54/43/mzi.gqvqlvcq.aac.p.m4p","time_millis":210743}

kind 可选值为：

可选值 | 解释
--- | --- |
normal | 普通的帖子，此时可以带上 attachments
apple_music | apple music 分享帖，此时 apple_media 为必填
apple_movie | apple movie 分享贴，此时 apple_media 为必填
apple_ebook | apple ebook 分享贴，此时 apple_media 为必填

attachments 中 `file` 表示 S3 返回的文件 key，`metadata` 是附件的元数据。
attachments key 的可选值：

可选值 | 解释
--- | --- |
image | 附件是一张图片
thumbnail | 附件是一张缩略图
audio | 附件是一段声音
video | 附件是一段视频

#### 示例

```
curl -XPOST 0.0.0.0:3000/api/v2/topics -F body=test -F latitude=11.11 -F longitude=22.22 -F allow_comment=true -F skill_id=cee89797007ab6db9f356f53edd0174a -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  <topic>
}
```

### 更新帖子

```
PUT /api/:version/topics/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| allow_comment | Boolean | 否 | 是否允许评论 |

#### 示例

```
curl -XPATCH 0.0.0.0:3000/api/v2/topics/516055075accc1e4067dd5ff6b2682cd -F allow_comment=true -H 'Authorization: Token token="test-token"'
```

#### 响应

```
{
  <topic>
}
```

### 删除帖子

```
DELETE /api/:version/topics/:id
```

#### 参数

无

#### 示例

```
curl -XDELETE https://park.catchchatchina.com/api/v2/topics/516055075accc1e4067dd5ff6b2682cd -H 'Authorization: Token token="test-token"'
```

#### 响应

不返回数据，只返回状态码
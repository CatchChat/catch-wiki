# CatchChat API Reference

* Author: Kgen (kgen@catch.la)
* Version: 0.5 (2014-11-21)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

### 基础 URL

```
https://www.catchchatserver.com/api
```

**注：为了安全，必须通过 https 请求所有 API。**

为了兼容性，采用 URL 来指定版本

```
https://www.catchchatserver.com/api/v4
```

### 响应

头部示例：

```
$ curl -i https://www.catchchatserver.com/api/v4/xxx

HTTP/1.1 200 OK
Server: nginx
Date: Thu, 30 Oct 2014 13:33:14 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Status: 200 OK
ETag: "a00049ba79152d03380c34652f2cb612"
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4987
X-RateLimit-Reset: 1350085394
Content-Length: 5
Cache-Control: max-age=0, private, must-revalidate
```

### 属性的格式

空字段，在返回时不会被忽略，而是以 `null` 返回。

时间字段采用 ISO 8601 格式：

```
YYYY-MM-DDTHH:MM:SSZ
```

一些值类型的属性，为了方便调用者直接显示，都有一个对应的格式化字符串一并返回，名称为<属性名>_string。比如：

created_at 字段，就有一个 created_at_string 字段一并返回。created_at 字段值为：'20140601T081030Z'，created_at_string 字段值为：'2014年6月1日 16:10:30'

### 参数

GET 请求中，路径中的片段和 query string 都可以作为参数使用：

```
$ curl -i "https://www.catchchatserver.com/api/v4/groups/233/friendships?state=accepted"
```

这个例子中，'233' 就是 :group_id 的参数，:state 则是 query string 中的参数。

POST, PATCH, PUT, DELETE 请求，除了路径片段意外的参数，以 form 方式提交，参数的 Content-Type 为 'application/x-www-form-urlencoded'

```
curl -X POST https://www.catchchatserver.com/api/auth/token_by_login -F login='your_login_name' -F password='yourpassword'
```

### 多语言

服务端根据 header 中的 `Accept-Language` 来确定 API 请求的语言，返回对应语言版本的字符串

### 时区

根据每个用户的 Timezone，返回对应 Timezone 的时间。

### 认证

服务端提供2种认证方式：

* 账号和密码认证
* 手机号和验证码认证

两种认证方式，获取的 Token 和使用权限没有任何区别。

#### 账号和密码认证

发送登录账号 (login) 和密码 (password)，可获取相应的 access_token。login 可以为邮箱或用户名。

```
POST /api/auth/token_by_login
```

| 参数 | 描述 |
|--------|--------|
| login | 用户名 / 邮箱 / 手机号 |
| password | 密码 |
| expiring | access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认7天过期 |

cURL 请求范例：

```
curl -X POST https://www.catchchatserver.com/api/auth/token_by_login -F login='your_login_name' -F password='yourpassword'
```

返回范例：

```
{
  login: "your_login_name",
  access_token: "p7DvqB4MoT5ux-B1xg"
}
```

cURL 请求范例（1小时过期）：

```
curl -X POST https://www.catchchatserver.com/api/auth/token_by_login -F login='yourname' -F password='yourpassword' -F expiring='3600'
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

cURL 请求范例：

```
curl -X POST https://www.catchchatserver.com/api/auth/send_verify_code -F mobile='your_mobile'
```

返回范例：

```
{
  mobile: "your_mobile"
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

cURL 请求范例：

```
curl -X POST https://www.catchchatserver.com/api/auth/token_by_mobile -F mobile='your_mobile' -F verify_code='your_verify_code'
```

返回范例：

```
{
  mobile: "your_mobile",
  access_token: "p7DvqB4MoT5ux-B1xg"
}
```

cURL 请求范例（1小时过期）：

```
curl -X POST https://www.catchchatserver.com/api/auth/token -F mobile='your_mobile' -F verify_code='your_verify_code' -F expiring='3600'
```

如果手机号和验证码错误，或验证码已经过期，则返回 HTTP 401.
{
  error: "手机号或验证码错误"
}

### 用 Access Token 调用其他 API

现在你可以通过 **access_token** 来调用其他 API 了，比如：

```
GET /api/xxx
```

cURL 请求范例：

```
curl https://www.catchchatserver.com/api/xxx  -H 'Authorization: Token token="p7DvqB4MoT5ux-B1xg"'
```

如果 access_token 不存在或过期了，则返回 HTTP 401

HTTP Token: Access denied.

### 分页

默认返回30条记录，可以通过 `per_page` 参数指定最多每次返回100条记录，但是有些 API 因为一些技术原因，会忽略 `per_page` 参数。页码用 `?page` 参数指定，从1开始。

cURL 请求范例：

```
curl https://www.catchchatserver.com/api/friendships?page=2&per_page=100
```

分页信息包含在 [Link header](http://tools.ietf.org/html/rfc5988) 中。

```
Link: <https://www.catchchatserver.com/api/friendships?page=3&per_page=100>; rel="next",
  <https://www.catchchatserver.com/api/friendships?page=50&per_page=100>; rel="last",
  <https://www.catchchatserver.com/api/friendships?page=1&per_page=100>; rel="first",
  <https://www.catchchatserver.com/api/friendships?page=1&per_page=100>; rel="prev"
```

参数 `rel` 包含如下取值：

| Header 字段 | 描述 |
|---|---|
| next | 显示下一页结果 |
| last | 显示最后一页结果 |
| first | 显示第一页结果 |
| prev | 显示上一页结果 |

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

----

## User 用户

### 获取当前用户信息

```
GET /api/user
```

响应：

```
{
  username: "johnsmith",
  nickname: "John",
  country_id: 46,
  country_string: "China",
  mobile: "18612345678",
  email: "john@example.com",
  state: "active",
  avatar_url: "http://www.example.com/abc.jpg"
}
```

### 更新当前用户信息

```
PATCH /api/user
```

参数

| 名称 | 类型 | 描述 |
|---|---|---|
| nickname | string | 昵称 |
| country_id | integer | 国家 |
| mobile | string | 手机号 |
| email | string | Email |

示例：

```
{
  nickname: "Genius",
  country_id: 1,
  mobile: "2331234567",
  email: "smith@example.com"
}
```

响应：

```
{
  username: "johnsmith",
  nickname: "Genius",
  country_id: 1,
  country_string: "United States",
  mobile: "2331234567",
  email: "smith@example.com"
  state: "active",
  avatar_url: "http://www.example.com/abc.jpg"
}
```

### 搜索用户

```
GET /api/users/search?q=keyword
```

---

## Friendship 好友关系

### 所有好友

```
GET /api/friendships
```

### 最近有联系的好友

```
GET /api/friendships/recent
```

### 某一好友的详细信息

```
GET /api/friendships/:id
```

### 更新好友信息

```
PATCH /api/friendships/:id
```

### 搜索好友

```
GET /api/friendships/search?q=keyword
```

---

## FriendRequest 好友请求

### 获取当前用户收到的好友请求

按创建时间倒序排

```
GET /api/friend_requests/received/:state
```

#### 参数

名称 | 类型 | 是否必须 | 描述
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

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
state | String | 是 | `:state` 是状态，可选：`pending`, `accepted`, `rejected`, `blocked`

#### 示例

```
curl https://catchchatserver.com/api/v4/friend_requests/sent/accepted -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
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
        "id":3,
        "username":"user1",
        "nickname":"user1",
        "name":"user1", // remarked_name || contact_name || nickname || username
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      }
    },
    {
      "id":1,
      "user_id":1,
      "friend_id":2,
      "state":2,
      "state_string":"已接受",
      "created_at":"2014-11-22T16:41:07Z",
      "created_at_string":"2014年11月22日 16:41:07",
      "updated_at":"2014-11-22T16:41:07Z",
      "updated_at_string":"2014年11月22日 16:41:07",
      "friend":{
        "id":2,
        "username":"tumayun",
        "nickname":"tumayun",
        "name":"tumayun", // remarked_name || contact_name || nickname || username
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      }
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":2
}
```

### 新建好友请求

```
POST /api/friend_requests
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
friend_id | Integer | 是 | 要添加的好友ID

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/friend_requests -F friend_id=3 -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
```

#### 响应

```
{
  "id":3,
  "user_id":1,
  "friend_id":3,
  "state":1,
  "state_string":"等待中",
  "created_at":"2014-11-22T17:08:07Z",
  "created_at_string":"2014年11月22日 17:08:07",
  "updated_at":"2014-11-22T17:08:07Z",
  "updated_at_string":"2014年11月22日 17:08:07",
  "friend":{
    "id":3,
    "username":"user1",
    "nickname":"user1",
    "name":"user1", // 如果是通讯录好友，则返回通讯录名字，否则返回 nickname
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  }
}
```

### 同意好友请求

```
POST /api/friend_requests/received/:id/accept
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friend_request ID

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/friend_requests/received/3/accept  -H 'Authorization: Token token=ZV5HH2XzrBQTgzL1NW1v1416645756.998624'
```

#### 响应

```
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
    "name":"ruanwztest", // 如果是通讯录好友，则返回通讯录名字，否则返回 nickname
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  }
}
```

### 拒绝好友请求

```
POST /api/friend_requests/received/:id/reject
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friend_request ID

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/friend_requests/received/4/reject -H 'Authorization: Token token=XVVP8GpgezByNjhjWaEs1416648757.346672'
```

#### 响应

```
{
  "id":4,
  "user_id":1,
  "friend_id":4,
  "state":3,
  "state_string":"已拒绝",
  "created_at":"2014-11-22T17:31:53Z",
  "created_at_string":"2014年11月22日 17:31:53",
  "updated_at":"2014-11-22T17:33:41Z",
  "updated_at_string":"2014年11月22日 17:31:53",
  "friend":{
    "id":1,
    "username":"ruanwztest",
    "nickname":"ruanwztest",
    "name":"ruanwztest", // 如果是通讯录好友，则返回通讯录名字，否则返回 nickname
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  }
}
```

### 禁止好友请求

```
POST /api/friend_requests/received/:id/block
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friend_request ID

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/friend_requests/received/5/block -H 'Authorization: Token token=XVVP8GpgezByNjhjWaEs1416648757.346672'
```

#### 响应

```
{
  "id":5,
  "user_id":2,
  "friend_id":4,
  "state":4,
  "state_string":"已禁止",
  "created_at":"2014-11-22T17:31:53Z",
  "created_at_string":"2014年11月22日 17:31:53",
  "updated_at":"2014-11-22T17:33:41Z",
  "updated_at_string":"2014年11月22日 17:31:53",
  "friend":{
    "id":1,
    "username":"tumayun",
    "nickname":"tumayun",
    "name":"tumayun", // 如果是通讯录好友，则返回通讯录名字，否则返回 nickname
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  }
}
```


---

## UnfriendRequest 解除好友请求

### 新建请求解除好友关系

```
POST /api/unfriend_requests
```

---

## Group 组

### 获取所有组

```
GET /api/groups
```

### 新建组

```
POST /api/groups
```

### 获取单个组信息

```
GET /api/groups/:id
```

### 更新组信息

```
PATCH /api/groups/:id
```

### 删除组

```
DELETE /api/groups/:id
```

### 添加好友到组

```
POST /api/groups/:id/add_friendship/:friendship_id
```

### 从组里移除好友

```
POST /api/groups/:id/remove_friendship/:friendship_id
```

---

## Message 消息

### 获取所有未读消息

```
GET /api/messages/unread
```

### 获取单条消息

```
GET /api/messages/:id
```

### 新建消息

```
POST /api/messages
```

### 标记消息为已读

```
POST /api/messages/:id/mark_as_read
```

---

## Attachment 附件

### 获取上传 token

```
GET /api/messages/:id/attachment/upload_token
```

不能单独请求上传，必须依赖一条已经生成的 Message，没有上传完成前，Message 的 state 是 drafted。

返回：

```
{
  "provider": "qiniu",
  "options": {
    "token": "LsTN5ioRCbSo7Vb:wQ4ofysef1R7IKnrziqtomqyDvI=:eyJzY29wZSI6IodCksXCJodX0ifQ==",
    "bucket": "messages1",
    "key": "a97b1d2570fc4823aec57522b618e779.jpg"
  }
}
```

获取上传 token 后，根据 provider 上传到相应的云存储，options 中包含了客户端上传所需要的参数。

七牛参数：

| 名称 | 类型 | 描述 |
|---|---|---|
| token | String | 上传凭证 |
| bucket | String | Bucket 的名称 |
| key | String | 文件名 |

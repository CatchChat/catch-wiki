# CatchChat API Reference

* Author: Kgen (kgen@catch.la)

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

标准时区有：

```
[
  "American Samoa",
  "International Date Line West",
  "Midway Island",
  "Samoa",
  "Hawaii",
  "Alaska",
  "Pacific Time (US & Canada)",
  "Tijuana",
  "Arizona",
  "Chihuahua",
  "Mazatlan",
  "Mountain Time (US & Canada)",
  "Central America",
  "Central Time (US & Canada)",
  "Guadalajara",
  "Mexico City",
  "Monterrey",
  "Saskatchewan",
  "Bogota",
  "Eastern Time (US & Canada)",
  "Indiana (East)",
  "Lima",
  "Quito",
  "Caracas",
  "Atlantic Time (Canada)",
  "Georgetown",
  "La Paz",
  "Santiago",
  "Montevideo",
  "Newfoundland",
  "Brasilia",
  "Buenos Aires",
  "Greenland",
  "Mid-Atlantic",
  "Azores",
  "Cape Verde Is.",
  "Casablanca",
  "Dublin",
  "Edinburgh",
  "Lisbon",
  "London",
  "Monrovia",
  "UTC",
  "Amsterdam",
  "Belgrade",
  "Berlin",
  "Bern",
  "Bratislava",
  "Brussels",
  "Budapest",
  "Copenhagen",
  "Ljubljana",
  "Madrid",
  "Paris",
  "Prague",
  "Rome",
  "Sarajevo",
  "Skopje",
  "Stockholm",
  "Vienna",
  "Warsaw",
  "West Central Africa",
  "Zagreb",
  "Athens",
  "Bucharest",
  "Cairo",
  "Harare",
  "Helsinki",
  "Istanbul",
  "Jerusalem",
  "Kyiv",
  "Pretoria",
  "Riga",
  "Sofia",
  "Tallinn",
  "Vilnius",
  "Baghdad",
  "Kuwait",
  "Minsk",
  "Moscow",
  "Nairobi",
  "Riyadh",
  "St. Petersburg",
  "Volgograd",
  "Tehran",
  "Abu Dhabi",
  "Baku",
  "Muscat",
  "Tbilisi",
  "Yerevan",
  "Kabul",
  "Ekaterinburg",
  "Islamabad",
  "Karachi",
  "Tashkent",
  "Chennai",
  "Kolkata",
  "Mumbai",
  "New Delhi",
  "Sri Jayawardenepura",
  "Kathmandu",
  "Almaty",
  "Astana",
  "Dhaka",
  "Novosibirsk",
  "Urumqi",
  "Rangoon",
  "Bangkok",
  "Hanoi",
  "Jakarta",
  "Krasnoyarsk",
  "Beijing",
  "Chongqing",
  "Hong Kong",
  "Irkutsk",
  "Kuala Lumpur",
  "Perth",
  "Singapore",
  "Taipei",
  "Ulaanbaatar",
  "Osaka",
  "Sapporo",
  "Seoul",
  "Tokyo",
  "Yakutsk",
  "Adelaide",
  "Darwin",
  "Brisbane",
  "Canberra",
  "Guam",
  "Hobart",
  "Magadan",
  "Melbourne",
  "Port Moresby",
  "Sydney",
  "Vladivostok",
  "New Caledonia",
  "Solomon Is.",
  "Auckland",
  "Fiji",
  "Kamchatka",
  "Marshall Is.",
  "Wellington",
  "Chatham Is.",
  "Nuku'alofa",
  "Tokelau Is."
]
```

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
| expiring | access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认一年过期 |
| client | 用于推送, official=0, company=1, local=2 |

cURL 请求范例：

```
curl -X POST https://www.catchchatserver.com/api/auth/token_by_login -F login='your_login_name' -F password='yourpassword' -F client=2
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
curl -X POST https://www.catchchatserver.com/api/auth/token_by_login -F login='yourname' -F password='yourpassword' -F expiring='3600' -F client=2
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
curl -X POST https://www.catchchatserver.com/api/auth/send_verify_code -F mobile='your_mobile' -F phone_code='86'
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
| phone_code | 国家代码 |
| verify_code | 验证码 |
| expiring | access_token 过期时间。单位为秒，设置为0表示永不过期，不设置默认7天过期 |
| client | 用于推送, official=0, company=1, local=2 |

cURL 请求范例：

```
curl -X POST https://www.catchchatserver.com/api/auth/token_by_mobile -F mobile='your_mobile' -F verify_code='your_verify_code' -F client=2
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
curl -X POST https://www.catchchatserver.com/api/auth/token -F mobile='your_mobile' -F verify_code='your_verify_code' -F expiring='3600' -F client=2
```

如果手机号和验证码错误，或验证码已经过期，则返回 HTTP 401.
{
  error: "手机号或验证码错误"
}

### 发送用户名密码手机号码，发起注册,等待接收手机验证码

```
POST   /api/v4/registration/create
```

| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| username | 用户名 |
| password | 密码 |
| phone_code | 国家码 |


### 发手机验证码完成注册

```
PUT   /api/v4/registration/update
```
| 参数 | 描述 |
|--------|--------|
| mobile | 手机号 |
| phone_code | 国家码 |
| username | 用户名 |
| token | 手机短信收到的验证码 |

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

按照 position 升序排列，最近联系过的好友越靠前 

```
GET /api/v4/friendships
```
#### 参数

无

#### 示例

```
curl https://catchchatserver.com/api/v4/friendships\?page\=1\&per_page\=10 -H 'Authorization: Token token=8ni89Uirfva2Zyyzq1ou1416506910.739525'
```

#### 响应

```
{
  "friendships":[
    {
      "id":3,
      "user_id":3,
      "friend_id":4,
      "contact_name":null,
      "remarked_name":null,
      "position":1,
      "name":"tumayun",
      "created_at":"2014-11-20T17:00:53Z",
      "created_at_string":"2014年11月20日 17:00:53",
      "updated_at":"2014-11-20T17:00:53Z",
      "updated_at_string":"2014年11月20日 17:00:53",
      "friend":{
        "id":4,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url": "http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      }
    }
    .
    .
    .
  ],
  "current_page":1,
  "per_page":10,
  "count":1
}
```

### 最近有联系的好友

最近三天有过联系的好友，包括发送消息给我的和我发过消息的好友

```
GET /api/v4/friendships/recent
```

#### 参数

无

#### 示例

```
curl https://catchchatserver.com/api/v4/friendships/recent\?page\=1\&per_page\=10 -H 'Authorization: Token token=8ni89Uirfva2Zyyzq1ou1416506910.739525'
```

#### 响应

```
{
  "friendships":[
    {
      "id":3,
      "user_id":3,
      "friend_id":4,
      "contact_name":null,
      "remarked_name":null,
      "position":1,
      "name":"tumayun",
      "created_at":"2014-11-20T17:00:53Z",
      "created_at_string":"2014年11月20日 17:00:53",
      "updated_at":"2014-11-20T17:00:53Z",
      "updated_at_string":"2014年11月20日 17:00:53",
      "friend":{
        "id":4,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar_url": "http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      }
    }
    .
    .
    .
  ],
  "current_page":1,
  "per_page":10,
  "count":1
}
```

### 某一好友的详细信息(by friendship id)

```
GET /api/v4/friendships/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id

#### 示例

```
curl https://catchchatserver.com/api/v4/friendships/3 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":3,
  "user_id":3,
  "friend_id":4,
  "contact_name":null,
  "remarked_name":null,
  "position":2,
  "name":"tumayun",
  "created_at":"2014-11-20T17:00:53Z",
  "created_at_string":"2014年11月20日 17:00:53",
  "updated_at":"2014-11-20T17:00:53Z",
  "updated_at_string":"2014年11月20日 17:00:53",
  "friend":{
    "id":4,
    "username":"tumayun",
    "nickname":"tumayun",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  }
}
```

### 某一好友的详细信息(by friend id)

```
GET /api/v4/friendships/with/:friend_id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
friend_id | Integer | 是 | friend id

#### 示例

```
curl https://catchchatserver.com/api/v4/friendships/with/4 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":3,
  "user_id":3,
  "friend_id":4,
  "contact_name":null,
  "remarked_name":null,
  "position":2,
  "name":"tumayun",
  "created_at":"2014-11-20T17:00:53Z",
  "created_at_string":"2014年11月20日 17:00:53",
  "updated_at":"2014-11-20T17:00:53Z",
  "updated_at_string":"2014年11月20日 17:00:53",
  "friend":{
    "id":4,
    "username":"tumayun",
    "nickname":"tumayun",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
  }
}
```

### 更新好友信息

```
PATCH /api/v4/friendships/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id
remarked_name | String | 否 | 备注名
contact_name | String | 否 | 通讯录名

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/friendships/3 -F remarked_name=tumayun -F contact_name=涂马云 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":3,
  "user_id":3,
  "friend_id":4,
  "contact_name":"涂马云",
  "remarked_name":"tumayun",
  "position":2,
  "name":"tumayun",
  "created_at":"2014-11-20T17:00:53Z",
  "created_at_string":"2014年11月20日 17:00:53",
  "updated_at":"2014-11-21T02:22:53Z",
  "updated_at_string":"2014年11月20日 17:00:53",
  "friend":{
    "id":4,
    "username":"tumayun",
    "nickname":"tumayun",
    "avatar":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
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
q | String | 是 | remarked_name 或者 contact_name 或者 nickname 或者 username

#### 示例

```
curl https://catchchatserver.com/api/v4/friendships/search\?q\=tumayun -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "friendships":[
    {
      "id":3,
      "user_id":3,
      "friend_id":4,
      "contact_name":"涂马云",
      "remarked_name":"tumayun",
      "position":2,
      "name":"tumayun",
      "created_at":"2014-11-20T17:00:53Z",
      "created_at_string":"2014年11月20日 17:00:53",
      "updated_at":"2014-11-21T02:22:53Z",
      "updated_at_string":"2014年11月20日 17:00:53",
      "friend":{
        "id":4,
        "username":"tumayun",
        "nickname":"tumayun",
        "avatar":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
      }
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 好友置顶

```
PATCH /api/v4/friendships/:id/move_to_top
```
#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/friendships/3/move_to_top -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{}
```

---

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

名称 | 类型 | 是否必需 | 描述
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

名称 | 类型 | 是否必需 | 描述
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

名称 | 类型 | 是否必需 | 描述
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

名称 | 类型 | 是否必需 | 描述
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
POST /api/v4/unfriend_requests
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| friend_id | Integer | 是 | 好友 ID |

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/unfriend_requests -F friend_id=2 -H 'Authorization: Token token=yZp5UZMeCB8yKBUy_ae81416827431.104971'
```

#### 响应

```
{}
```

---

## Group 组

### 获取所有组

```
GET /api/v4/groups
```

#### 参数

无

#### 示例

```
curl https://catchchatserver.com/api/v4/groups -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{
  "groups":[
    {
      "id":1,
      "owner_id":1,
      "name":"group",
      "position":1,
      "created_at":"2014-11-25T21:08:48Z",
      "created_at_string":"2014年11月25日 21:08:48",
      "updated_at":"2014-11-25T21:08:48Z",
      "updated_at_string":"2014年11月25日 21:08:48",
      "friends":[
        {
          "id":2,
          "username":"tumayun",
          "nickname":"tumayun",
          "name":"tumayun",
          "contact_name":"tumayun",
          "remarked_name":"tumayun",
          "position":1,
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
        }
      ]
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 新建组

```
POST /api/v4/groups
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 群组名 |

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/groups -F name="test-group" -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{
  "id":2,
  "owner_id":1,
  "name":"test-group",
  "position":2,
  "created_at":"2014-11-25T21:21:25Z",
  "created_at_string":"2014年11月25日 21:21:25",
  "updated_at":"2014-11-25T21:21:25Z",
  "updated_at_string":"2014年11月25日 21:21:25",
  "friends":[

  ]
}
```

### 获取单个组信息

```
GET /api/v4/groups/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 群组 ID |

#### 示例

```
curl https://catchchatserver.com/api/v4/groups/2 -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{
  "id":2,
  "owner_id":1,
  "name":"test-group",
  "position":2,
  "created_at":"2014-11-25T21:21:25Z",
  "created_at_string":"2014年11月25日 21:21:25",
  "updated_at":"2014-11-25T21:21:25Z",
  "updated_at_string":"2014年11月25日 21:21:25",
  "friends":[
    {
      "id":2,
      "username":"tumayun",
      "nickname":"tumayun",
      "name":"tumayun",
      "contact_name":"tumayun",
      "remarked_name":"tumayun",
      "position":1,
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    }
  ]
}
```

### 更新组信息

```
PATCH /api/v4/groups/:id
```
#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 群组 ID |
| name | String | 是 | 群组名 |

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/groups/2 -F name='test' -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{
  "id":2,
  "owner_id":1,
  "name":"test",
  "position":2,
  "created_at":"2014-11-25T21:21:25Z",
  "created_at_string":"2014年11月25日 21:21:25",
  "updated_at":"2014-11-25T22:00:39Z",
  "updated_at_string":"2014年11月25日 21:21:25",
  "friends":[
    {
      "id":2,
      "username":"tumayun",
      "nickname":"tumayun",
      "name":"tumayun",
      "contact_name":"tumayun",
      "remarked_name":"tumayun",
      "position":1,
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    }
  ]
}
```


### 删除组

```
DELETE /api/v4/groups/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 群组 ID |

#### 示例

```
curl -X DELETE https://catchchatserver.com/api/v4/groups/2 -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{}
```

### 添加好友到组

```
POST /api/v4/groups/:id/add_friendship
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 群组 ID |
| friendship_id | Integer | 是 | friendship id |

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/groups/2/add_friendship -F friendship_id=3 -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{}
```

### 从组里移除好友

```
DELETE /api/v4/groups/:id/remove_friendship/:friendship_id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 群组 ID |
| friendship_id | Integer | 是 | friendship id |

#### 示例

```
curl -X DELETE https://catchchatserver.com/api/v4/groups/2/remove_friendship/3 -H 'Authorization: Token token=NDccv4Yvdi9UKtwPToxx1416921006.674603'
```

#### 响应

```
{}
```

---

## Message 消息

### 获取所有未读消息

```
GET /api/messages/unread
```

#### 参数

无

#### 示例

```
curl https://catchchatserver.com/api/v4/messages/unread -H 'Authorization: Token token="nH-CaGbGvS5tJRizTsiM1418019414.813717"' -k
```

#### 响应

```
{
  "messages":[
    {
      "id":1,
      "recipient_id":2,
      "recipient_type":"User",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":122.692612,
      "latitude":30.19902,
      "battery_level":50,
      "media_type":"text",
      "media_type_string":"一段文字",
      "state":2,
      "state_string":"未读",
      "created_at":"2014-12-06T22:19:30Z",
      "created_at_string":"2014年12月06日 22:19:30",
      "updated_at":"2014-12-06T22:19:30Z",
      "updated_at_string":"2014年12月06日 22:19:30",
      "sender":{
        "id":1,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "name":"ruanwztest"
      },
      "attachments":[

      ]
    },
    {
      "id":2,
      "recipient_id":1,
      "recipient_type":"Group",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":122.692612,
      "latitude":30.19902,
      "battery_level":50,
      "media_type":"text",
      "media_type_string":"一段文字",
      "state":2,
      "state_string":"未读",
      "created_at":"2014-12-06T22:19:30Z",
      "created_at_string":"2014年12月06日 22:19:30",
      "updated_at":"2014-12-06T22:19:30Z",
      "updated_at_string":"2014年12月06日 22:19:30",
      "sender":{
        "id":1,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "name":"ruanwztest"
      },
      "attachments":[

      ]
    },
    {
      "id":3,
      "recipient_id":2,
      "recipient_type":"User",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":122.692612,
      "latitude":30.19902,
      "battery_level":50,
      "media_type":"text",
      "media_type_string":"一段文字",
      "state":2,
      "state_string":"未读",
      "created_at":"2014-12-08T13:54:29Z",
      "created_at_string":"2014年12月08日 13:54:29",
      "updated_at":"2014-12-08T13:54:29Z",
      "updated_at_string":"2014年12月08日 13:54:29",
      "sender":{
        "id":1,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "name":"ruanwztest"
      },
      "attachments":[

      ]
    },
    {
      "id":4,
      "recipient_id":2,
      "recipient_type":"User",
      "text_content":"This is a test!",
      "parent_id":0,
      "longitude":122.692612,
      "latitude":30.19902,
      "battery_level":50,
      "media_type":"video",
      "media_type_string":"一段视频",
      "state":2,
      "state_string":"未读",
      "created_at":"2014-12-08T13:57:26Z",
      "created_at_string":"2014年12月08日 13:57:26",
      "updated_at":"2014-12-08T14:12:52Z",
      "updated_at_string":"2014年12月08日 13:57:26",
      "sender":{
        "id":1,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "name":"ruanwztest"
      },
      "attachments":[
        {
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
  "count":4
}
```

### 获取收到的单条消息

```
GET /api/v4/messages/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 消息 ID |

#### 示例

```
curl https://catchchatserver.com/api/v4/messages/4 -H 'Authorization: Token token="nH-CaGbGvS5tJRizTsiM1418019414.813717"'
```

#### 响应

```
{
  "id":4,
  "recipient_id":2,
  "recipient_type":"User",
  "text_content":"This is a test!",
  "parent_id":0,
  "longitude":122.692612,
  "latitude":30.19902,
  "battery_level":50,
  "media_type":"video",
  "media_type_string":"一段视频",
  "state":2,
  "state_string":"未读",
  "created_at":"2014-12-08T13:57:26Z",
  "created_at_string":"2014年12月08日 13:57:26",
  "updated_at":"2014-12-08T14:12:52Z",
  "updated_at_string":"2014年12月08日 13:57:26",
  "sender":{
    "id":1,
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "name":"ruanwztest"
  },
  "attachments":[
    {
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
```

### 新建消息

```
POST /api/v4/messages
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
recipient_id | Integer | 是 | 接收者 ID，接收者只有两种，User 或 Group，所以是 User ID 或者 Group ID |
recipient_type | String | 是 | 接受者类型，只能是 User、Group 或者 Circle（**Park Server 才有**）
media_type | Integer | 否 | 消息类型，0 表示文字, 1 表示图片, 2表示视频, 默认是文字
text_content | String | 否 | 文字内容，**只有是文字消息时才是必填字段，其他情况都是选填字段**
parent_id | Integer | 否 | 回复的消息 ID，表示当前要发送的消息是回复哪条消息
longitude | Float | 否 | 经度
latitude | Float | 否 | 纬度
battery_level | Integer | 否 | 电量，0 - 100 之间的值，默认为 50

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/messages -F recipient_id=2 -F recipient_type=User -F text_content='This is a test!' -F battery_level=70 -H 'Authorization: Token token="NDccv4Yvdi9UKtwPToxx1416921006.674603"'
```

#### 响应

```
{
  "id":3,
  "battery_level":70,
  "state":2,
  "state_string":"未读",
  "media_type":"text",
  "media_type_string":"一段文字"
}
```

### 标记消息为已读

```
PATCH /api/v4/messages/:id/mark_as_read
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | 消息 ID

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/messages/3/mark_as_read -H 'Authorization: Token token="TKWsindneiDsFj3gUHs31416969554.7962759"'
```

#### 响应

```
{}
```


### 标记消息已收到

```
PATCH /api/v4/messages/:id/deliver
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | 消息 ID

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/messages/3/deliver -H 'Authorization: Token token="TKWsindneiDsFj3gUHs31416969554.7962759"'
```

#### 响应

```
{}
```

### 通知消息被截图保存

```
POST /api/v4/messages/:id/notify_screenshot
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
id | Integer | 是 | 消息 ID

#### 示例

curl -X POST https://catchchatserver.com/api/v4/messages/3/notify_screenshot -H 'Authorization: Token token="TKWsindneiDsFj3gUHs31416969554.7962759"'

#### 响应

```
{}
```

---

## Attachment 附件

### 获取上传 token

```
GET /api/messages/:id/upload_token
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

## Contact 通讯录

### 上传通讯录并返回已注册的通讯录好友

**覆盖式上传，上传后会删除之前的通讯录**

```
POST /api/v4/contacts/upload
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| contacts | JSON | 是 | contacts JSON format, 如 "[{\"name\":\"abc\",\"number\:\"15158166372\"},{\"name\":\"bac\",\"number\":\"15158166723\"}]"|

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/contacts/upload -F contacts="[\"name\":\"涂马云\",\"number\":\"15158166372\"}]" -H 'Authorization: Token token="possbdsrHyRhU_zmwsNy1417016499.200043"'
```

#### 响应

```
{
  "registered_contacts":[
    {
      "id": 1,
      "name": "涂马云",
      "user": {
        "id": 1,
        "username": "tumayun",
        "avatar_url": "http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "nickname": "Tumayun",
        "number": "+8615158166372"
      }
    }
  ]
}
```

## User 个人信息 API

### 可能认识的好友

```
GET /api/v4/user/may_know_friends
```

#### 参数

无

#### 示例

```
curl https://catchchatserver.com/api/v4/user/may_know_friends -H Authorization: Token token="kuH3PbRifgSATCanYwxd1418031570.162303"'
```

#### 响应

```
{
  "friends":[
    {
      "id":8,
      "username":"friend6",
      "nickname":"friend6",
      "name":"friend6",
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
      "name":"friend7",
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
      "name":"friend8",
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
GET /api/v4/user
```

#### 参数

无

#### 示例

```
curl https://catchchatserver.com/api/v4/user -H 'Authorization: Token token="kuH3PbRifgSATCanYwxd1418031570.162303"'
```

#### 响应

```
{
  "id":2,
  "name":"tumayun",
  "username":"tumayun",
  "nickname":"tumayun",
  "phone_code":"86",
  "mobile":"15158166372",
  "mobile_verified":true,
  "time_zone":"Beijing",
  "state":1,
  "state_string":"活跃",
  "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
}
```

### 更新个人信息

```
PATCH /api/v4/user
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| nickname | String | 否 | 昵称 |
| avatar_url | String | 否 | 头像 URL |

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/user -F nickname=Tumayun -F avatar_url=http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg -H 'Authorization: Token token="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{
  "id":2,
  "name":"Tumayun",
  "username":"tumayun",
  "nickname":"Tumayun",
  "phone_code":"86",
  "mobile":"15158166372",
  "mobile_verified":true,
  "state":1,
  "state_string":"活跃",
  "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
}
```

### 更新手机号流程

1. 发送当前手机号验证码 (POST /api/v4/auth/send_verify_code)
2. 校验当前手机号验证码  (PATCH /api/v4/user/check_verify_code)
3. 发送新手机号验证码 (POST /api/v4/user/send_update_mobile_code)
4. 校验新手机号验证码，通关验证后更新手机号为新手机好 (PATCH /api/v1/user/update_mobile)

### 验证更新手机号请求的验证码

```
PATCH /api/v4/user/check_verify_code
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| token | String | 是 | 短信验证码 |

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/user/check_verify_code -F token=1234 -H 'Authorization: Token token="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{}
```

### 发送新手机号验证码

```
POST /api/v1/user/send_update_mobile_code
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 国家码 |
| mobile | String | 是 | 手机号 |

#### 示例

```
curl -X POST -H 'Authorization: Token token="E9PnSDQMRZvjzL84yBi21418033718.2053812"' -H "Content-Type: application/json" -d '{"phone_code":"86","mobile":"12345678"}' https://catchchatserver.com/api/v4/user/send_update_mobile_code
```

#### 响应

```
{}
```

### 更新手机号

```
PATCH /api/v1/user/update_mobile
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 手机号国家码，详见 [http://countrycode.org/](http://countrycode.org/) |
| mobile | String | 是 | 手机号 |
| token | String | 是 | 短信验证码 |

#### 示例

```
curl -X PATCH https://catchchatserver.com/api/v4/user/update_mobile -F phone_ode=86 -F mobile=15158166372 -F token=131421 -H 'Authorization: Token token="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{}
```

## Users 用户

### 搜索用户

```
GET api/v4/users/search
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| q | String | 是 | 用户名或者昵称 |

#### 示例

```
curl https://catchchatserver.com/api/v4/users/search\?q\=t -H 'Authorization: oken token="EtErCK18xN9pxakiCPp61418029033.582837"'
```

#### 响应

```
{
  "users":[
    {
      "id":24003,
      "name":"Tumayun",
      "username":"tumayun",
      "nickname":"Tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg"
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 校验 username 是否可用（无需登录）

```
GET api/v4/users/username_validate
```
username 长度范围为 4 到 16，只允许出现大小写字符和数字，且具有唯一性。

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| username | String | 是 | 用户名 |

#### 示例

```
curl https://catchchatserver.om/api/v4/users/username_validate\?username\=tumayun
```

#### 响应

* 用户名可用

```
{  
  "available":true
}
```

* 用户名不可用

```
{  
  "available":false,
  "message":"用户名已经被使用"
}
```

### 校验手机号是否可用（无需登录）

```
GET api/v4/users/mobile_validate
```
phone code 合法, mobile 合法，且具有唯一性。    
*在设置中修改手机号时，不能拿当前手机号去校验，否则会返回手机号已经被使用*

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 国家码 |
| mobile | String | 是 | 手机号 |

#### 示例

```
curl https://catchchatserver.com/api/v4/users/mobile_validate\?phone_code\=86\mobile\=15158166372
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

## Report 消息举报

### 举报

```
POST /api/v4/reports
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| message_id | Integer | 是 | 要举报的消息 ID |

#### 示例

```
curl -X POST https://catchchatserver.com/api/v4/reports -F message_id=1 -H 'Authorization: Token token="6Y62Cc4FQ1GVw3ZiLqzq1418393864.8241951"'
```

#### 响应

```
{}
```

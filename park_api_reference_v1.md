# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

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
      "nickname":"friend6",
      "phone_code":"86",
      "mobile":"15158161111",
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
      "nickname":"friend7",
      "phone_code":"86",
      "mobile":"15158161112",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "common_friend_names":[
        "friend3",
        "friend4",
        "friend5"
      ]
    },
    {
      "id":10,
      "nickname":"friend8",
      "phone_code":"86",
      "mobile":"15158161113",
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
  "id":8,
  "nickname":"Tumayun",
  "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
  "phone_code":"86",
  "mobile":"15158161111",
  "pusher_id":"54cb2da674756d5392010000",
  "state":"active",
  "state_string":"激活状态"
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

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/user -F nickname=Tumayun -F avatar_url=http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg -H 'Authorization: Token oken="E9PnSDQMRZvjzL84yBi21418033718.2053812"'
```

#### 响应

```
{
  "id":8,
  "nickname":"Tumayun",
  "avatar_url":"http://catch-avatars.qiniudn.om/sJAUYG6nc84glXkq.jpg",
  "phone_code":"86",
  "mobile":"15158161111",
  "pusher_id":"54cb2da674756d5392010000",
  "state":"active",
  "state_string":"激活状态"
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
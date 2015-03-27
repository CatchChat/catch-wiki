# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

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

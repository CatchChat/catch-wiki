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

## Friendship 好友关系

### 所有好友

按照 position 升序排列，最近联系过的好友越靠前 

```
GET /api/v1/friendships
```
#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v1/friendships\?page\=1\&per_page\=10 -H 'Authorization: Token token=8ni89Uirfva2Zyyzq1ou1416506910.739525'
```

#### 响应

```
{
  "friendships":[
    {
      "id":13,
      "user_id":8,
      "friend_id":10,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,           // 标示是否星组成员
      "blocked":false,           // 标示是否拒收 friend 的消息
      "position":1,
      "favored_position":null,
      "name":"user1",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":10,
        "nickname":"user1",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160001"
      }
    },
    {
      "id":15,
      "user_id":8,
      "friend_id":11,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":2,
      "favored_position":null,
      "name":"user2",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":11,
        "nickname":"user2",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160002"
      }
    },
    {
      "id":17,
      "user_id":8,
      "friend_id":12,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":3,
      "favored_position":null,
      "name":"user3",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":12,
        "nickname":"user3",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160003"
      }
    },
    {
      "id":25,
      "user_id":8,
      "friend_id":13,
      "contact_name":"abc",
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":4,
      "favored_position":null,
      "name":"abc",
      "created_at":"2015-01-30T07:17:07Z",
      "created_at_string":"2015年01月30日 07:17:07",
      "updated_at":"2015-01-30T07:17:07Z",
      "updated_at_string":"2015年01月30日 07:17:07",
      "friend":{
        "id":13,
        "nickname":"user4",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160004"
      }
    },
    {
      "id":27,
      "user_id":8,
      "friend_id":14,
      "contact_name":"bac",
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":5,
      "favored_position":null,
      "name":"bac",
      "created_at":"2015-01-30T07:17:07Z",
      "created_at_string":"2015年01月30日 07:17:07",
      "updated_at":"2015-01-30T07:17:07Z",
      "updated_at_string":"2015年01月30日 07:17:07",
      "friend":{
        "id":14,
        "nickname":"user5",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160005"
      }
    }
  ],
  "current_page":1,
  "per_page":10,
  "count":5
}
```

### 最近有联系的好友

最近三天有过联系的好友，包括发送消息给我的和我发过消息的好友

```
GET /api/v1/friendships/recent
```

#### 参数

无

#### 示例

```
curl https://park.catchchatchina.com/api/v1/friendships/recent\?page\=1\&per_page\=10 -H 'Authorization: Token token=8ni89Uirfva2Zyyzq1ou1416506910.739525'
```

#### 响应

```
{
  "friendships":[
    {
      "id":27,
      "user_id":8,
      "friend_id":14,
      "contact_name":"bac",
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":5,
      "favored_position":null,
      "name":"bac",
      "created_at":"2015-01-30T07:17:07Z",
      "created_at_string":"2015年01月30日 07:17:07",
      "updated_at":"2015-01-30T07:17:07Z",
      "updated_at_string":"2015年01月30日 07:17:07",
      "friend":{
        "id":14,
        "nickname":"user5",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160005"
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
GET /api/v1/friendships/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id

#### 示例

```
curl https://park.catchchatchina.com/api/v1/friendships/27 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":27,
  "user_id":8,
  "friend_id":14,
  "contact_name":"bac",
  "remarked_name":null,
  "favored":false,
  "blocked":false,
  "position":5,
  "favored_position":null,
  "name":"bac",
  "created_at":"2015-01-30T07:17:07Z",
  "created_at_string":"2015年01月30日 07:17:07",
  "updated_at":"2015-01-30T07:17:07Z",
  "updated_at_string":"2015年01月30日 07:17:07",
  "friend":{
    "id":14,
    "nickname":"user5",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158160005"
  }
}
```

### 某一好友的详细信息(by friend id)

```
GET /api/v1/friendships/with/:friend_id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
friend_id | Integer | 是 | friend id

#### 示例

```
curl https://park.catchchatchina.com/api/v1/friendships/with/14 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "id":27,
  "user_id":8,
  "friend_id":14,
  "contact_name":"bac",
  "remarked_name":null,
  "favored":false,
  "blocked":false,
  "position":5,
  "favored_position":null,
  "name":"bac",
  "created_at":"2015-01-30T07:17:07Z",
  "created_at_string":"2015年01月30日 07:17:07",
  "updated_at":"2015-01-30T07:17:07Z",
  "updated_at_string":"2015年01月30日 07:17:07",
  "friend":{
    "id":14,
    "nickname":"user5",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158160005"
  }
}
```

### 更新好友信息

```
PATCH /api/v1/friendships/:id
```

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id
remarked_name | String | 否 | 备注名
contact_name | String | 否 | 通讯录名
blocked | Boolean | 否 | 标示是否拒收 friend 消息

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/friendships/13 -F contact_name=contact_name -F remarked_name=remarked_name -F blocked=true -H 'Authorization: Token token="sVNxda9nywMLZkuzUqf31422601654.468095"'
```

#### 响应

```
{
  "id":13,
  "user_id":8,
  "friend_id":10,
  "contact_name":"contact_name",
  "remarked_name":"remarked_name",
  "favored":false,
  "blocked":true,
  "position":1,
  "favored_position":null,
  "name":"remarked_name",
  "created_at":"2015-01-30T07:07:19Z",
  "created_at_string":"2015年01月30日 07:07:19",
  "updated_at":"2015-01-30T08:59:30Z",
  "updated_at_string":"2015年01月30日 07:07:19",
  "friend":{
    "id":10,
    "nickname":"user1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158160001"
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
q | String | 是 | remarked_name 或者 contact_name 或者 nickname 或者 mobile

#### 示例

```
curl https://park.catchchatchina.com/api/v1/friendships/search\?q\=1515816 -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{
  "friendships":[
    {
      "id":13,
      "user_id":8,
      "friend_id":10,
      "contact_name":"contact_name",
      "remarked_name":"remarked_name",
      "favored":true,
      "blocked":true,
      "position":1,
      "favored_position":null,
      "name":"remarked_name",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T08:59:31Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":10,
        "nickname":"user1",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160001"
      }
    },
    {
      "id":15,
      "user_id":8,
      "friend_id":11,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":2,
      "favored_position":null,
      "name":"user2",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":11,
        "nickname":"user2",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160002"
      }
    },
    {
      "id":17,
      "user_id":8,
      "friend_id":12,
      "contact_name":null,
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":3,
      "favored_position":null,
      "name":"user3",
      "created_at":"2015-01-30T07:07:19Z",
      "created_at_string":"2015年01月30日 07:07:19",
      "updated_at":"2015-01-30T07:07:19Z",
      "updated_at_string":"2015年01月30日 07:07:19",
      "friend":{
        "id":12,
        "nickname":"user3",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160003"
      }
    },
    {
      "id":25,
      "user_id":8,
      "friend_id":13,
      "contact_name":"abc",
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":4,
      "favored_position":null,
      "name":"abc",
      "created_at":"2015-01-30T07:17:07Z",
      "created_at_string":"2015年01月30日 07:17:07",
      "updated_at":"2015-01-30T07:17:07Z",
      "updated_at_string":"2015年01月30日 07:17:07",
      "friend":{
        "id":13,
        "nickname":"user4",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160004"
      }
    },
    {
      "id":27,
      "user_id":8,
      "friend_id":14,
      "contact_name":"bac",
      "remarked_name":null,
      "favored":false,
      "blocked":false,
      "position":5,
      "favored_position":null,
      "name":"bac",
      "created_at":"2015-01-30T07:17:07Z",
      "created_at_string":"2015年01月30日 07:17:07",
      "updated_at":"2015-01-30T07:17:07Z",
      "updated_at_string":"2015年01月30日 07:17:07",
      "friend":{
        "id":14,
        "nickname":"user5",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160005"
      }
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":5
}
```

### 好友置顶

```
PATCH /api/v1/friendships/:id/move_to_top
```
#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
id | Integer | 是 | friendship id

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/friendships/3/move_to_top -H 'Authorization: Token token="8ni89Uirfva2Zyyzq1ou1416506910.739525"'
```

#### 响应

```
{}
```

### 批量设置星组好友

```
PATCH /api/v1/friendships/batch_mark_as_favored
```

**按照提交的ID顺序覆盖式设置星组好友**

#### 参数

名称 | 类型 | 是否必需 | 描述
--- |--- |--- |--- |
ids | Integer | 是 | friendship ids

#### 示例

```
curl -X PATCH https://park.catchchatchina.com/api/v1/friendships/batch_mark_as_favored -d '{ "ids": [3,5,1] }' -H "Content-Type: application/json" -H 'Authorization: Token token="wcdyPGTv3HqZ76vZt5VR1422869949.0730119"'
```

#### 响应

```
{
  "friendships":[
    {
      "id":3,
      "user_id":2,
      "friend_id":5,
      "contact_name":null,
      "remarked_name":null,
      "favored":true,
      "blocked":false,
      "position":2,
      "favored_position":0,
      "name":"user2",
      "created_at":"2015-02-02T08:06:19Z",
      "created_at_string":"2015年02月02日 08:06:19",
      "updated_at":"2015-02-02T08:06:19Z",
      "updated_at_string":"2015年02月02日 08:06:19",
      "friend":{
        "id":5,
        "nickname":"user2",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160002"
      }
    },
    {
      "id":5,
      "user_id":2,
      "friend_id":6,
      "contact_name":null,
      "remarked_name":null,
      "favored":true,
      "blocked":false,
      "position":3,
      "favored_position":1,
      "name":"user3",
      "created_at":"2015-02-02T08:06:19Z",
      "created_at_string":"2015年02月02日 08:06:19",
      "updated_at":"2015-02-02T08:06:19Z",
      "updated_at_string":"2015年02月02日 08:06:19",
      "friend":{
        "id":6,
        "nickname":"user3",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160003"
      }
    },
    {
      "id":1,
      "user_id":2,
      "friend_id":4,
      "contact_name":null,
      "remarked_name":null,
      "favored":true,
      "blocked":false,
      "position":1,
      "favored_position":2,
      "name":"user1",
      "created_at":"2015-02-02T08:06:19Z",
      "created_at_string":"2015年02月02日 08:06:19",
      "updated_at":"2015-02-02T08:06:19Z",
      "updated_at_string":"2015年02月02日 08:06:19",
      "friend":{
        "id":4,
        "nickname":"user1",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158160001"
      }
    }
  ]
}
```

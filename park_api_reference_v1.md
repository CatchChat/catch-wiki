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
GET /api/v4/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| page | Integer | 否 | 当然页码 |
| pre_page | Integer | 否 | 每页显示数量 |

#### 示例

```
curl -X GET 0.0.0.0:3000/api/v4/circles -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
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
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158161111"
      },
      "members":[
        {
          "id":1,
          "nickname":"tumayun",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161111"
        },
        {
          "id":3,
          "nickname":"friend1",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161113"
        },
        {
          "id":4,
          "nickname":"friend2",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161114"
        },
        {
          "id":5,
          "nickname":"friend3",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161115"
        }
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
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158161111"
      },
      "members":[
        {
          "id":1,
          "nickname":"tumayun",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161111"
        },
        {
          "id":4,
          "nickname":"friend2",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161114"
        },
        {
          "id":5,
          "nickname":"friend3",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161115"
        },
        {
          "id":3,
          "nickname":"friend1",
          "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
          "phone_code":"86",
          "mobile":"15158161113"
        }
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
POST /api/v4/circles
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| members | String | 否 | 成员ID数组 |

#### 示例

```
curl -i -X POST 0.0.0.0:3000/api/v4/circles -d '{ "members": [3,4,5], "name": "群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
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
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158161113"
  },
  "members":[
    {
      "id":3,
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161113"
    },
    {
      "id":4,
      "nickname":"friend2",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161114"
    },
    {
      "id":5,
      "nickname":"friend3",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161115"
    },
    {
      "id":1,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    }
  ]
}
```

### 更新公共群组

```
PUT /api/v4/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| name | String | 是 | 公共群组名 |
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X PUT 0.0.0.0:3000/api/v4/circles/2 -d '{ "name": "公共群组" }' -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"' -H "Content-Type: application/json"
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
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158161113"
  },
  "members":[
    {
      "id":3,
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161113"
    },
    {
      "id":4,
      "nickname":"friend2",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161114"
    },
    {
      "id":5,
      "nickname":"friend3",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161115"
    },
    {
      "id":1,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    }
  ]
}
```

#### 获取单个公共群组

```
GET /api/v4/circles/:id
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET 0.0.0.0:3000/api/v4/circles/2 -H 'Authorization: Token token="r6yCiGr4N2oYyMzL65sr1422524661.762872"'
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
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158161113"
  },
  "members":[
    {
      "id":3,
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161113"
    },
    {
      "id":4,
      "nickname":"friend2",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161114"
    },
    {
      "id":5,
      "nickname":"friend3",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161115"
    },
    {
      "id":1,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    }
  ]
}
```
I
#### 加入公共群组

```
POST /api/v4/circles/:id/join
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X POST 0.0.0.0:3000/api/v4/circles/2/join -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
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
    "nickname":"tumayun",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158161111"
  },
  "members":[
    {
      "id":1,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    },
    {
      "id":4,
      "nickname":"friend2",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161114"
    },
    {
      "id":5,
      "nickname":"friend3",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161115"
    },
    {
      "id":3,
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161113"
    }
  ]
}
```

### 退出公共群组

```
DELETE /api/v4/circles/:id/leave
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X DELETE http://0.0.0.0:3000/api/v4/circles/2/leave -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"'
```

#### 响应

```
{}
```

### 批量添加成员

```
POST /api/v4/circles/:id/batch_add
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要添加的成员ID数组 |

#### 示例

```
curl -X POST 0.0.0.0:3000/api/v4/circles/2/batch_add -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
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
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158161113"
  },
  "members":[
    {
      "id":3,
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161113"
    },
    {
      "id":1,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    },
    {
      "id":4,
      "nickname":"friend2",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161114"
    },
    {
      "id":5,
      "nickname":"friend3",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161115"
    }
  ]
}
```

### 批量删除成员（只能是群 owner 才能删除成员）

```
DELETE /api/v4/circles/:id/batch_delete
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |
| members | String | 否 | 要删除的成员ID数组 |

#### 示例

```
curl -X DELETE 0.0.0.0:3000/api/v4/circles/2/batch_delete -d '{ "members": [4,5] }' -H 'Authorization: Token token="g5zzZ2Pk5eJpC4CqZ5hJ1422527060.772875"' -H "Content-Type: application/json"
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
    "nickname":"friend1",
    "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
    "phone_code":"86",
    "mobile":"15158161113"
  },
  "members":[
    {
      "id":3,
      "nickname":"friend1",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161113"
    },
    {
      "id":1,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    }
  ]
}
```

### 公共群组未读消息

```
GET /api/v4/circles/:id/unread_messages
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| id | Integer | 是 | 公共群组 ID |

#### 示例

```
curl -X GET http://park.catchchatchina.com/api/v4/circles/2/unread_messages -H 'Authorization: Token token="QVXyAg6mpi14YDje9RSr1421656248.3836942"'
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
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158161111",
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
        "nickname":"tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "phone_code":"86",
        "mobile":"15158161111",
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

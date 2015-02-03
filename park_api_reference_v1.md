# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

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


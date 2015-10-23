# Faye Server API Reference

<!-- MarkdownTOC -->

* Author: tumayun (tumayun@catch.la)

## 说明

此为 Faye Server v1 文档，Faye Server 是使用 [Faye](http://faye.jcoglan.com/) 框架的基于 [Bayeux](http://svn.cometd.org/trunk/bayeux/bayeux.html) 协议的消息系统。

客户端在 Faye Server 可以做的操作有：

* 订阅消息
* 发布消息

订阅消息的频道有（对应各版本的 Faye Server）：

* Direct message `/v1/users/:user_id/messages`
* Group message `/v1/circles/:circle_id/messages`

发布消息的频道有（各版本 Faye Server 发布消息频道均一致）：

* Direct message `/users/:user_id/messages`
* Group message `/circles/:circle_id/messages`

## Version

在订阅发布等任何操作都必须在 `ext` 中加入 `version`，用来表明要使用 Faye Server 的哪个版本。有些操作会请求 API Server，API Server 的版本与 Faye Server 的版本一致。如：

```
{ ext: { version: 'v1' } }
```

此时 Faye Server 版本为 v1，如果在 Faye Server 需要请求 API Server，那么请求的 API Server 版本也将用 v1，API Server 版本与 Faye Server 版本同步更新。


## Authentication

订阅发布等操作都需要传入 access_token，用以表明身份，如：

```
{ ext: { version: 'v1', access_token: 'jXgVN7qKwPrsZvrazr5o1429260353.954617' } }
```

## 发布消息

消息类型（message_type）有 ：

* message 聊天消息
* instant_state 即时状态消息
* mark_as_read 标记为已读消息

### 发布即时状态消息

用户正在输入，正在上传等即时状态消息

客户端A想客户端B发送如下消息：

```
{
  "ext":{
    "version":"v1",
    "access_token":"jXgVN7qKwPrsZvrazr5o1429260353.954617"
  },
  "data":{
    "message_type":"instant_state", // 消息类型，此处为即时状态消息
    "message":{
      "state":"typing" // 即时状态，如 typing, uploading
    }
  }
}
```

客户端B将会接收到如下消息：

```
{
  "data":{
    "message_type":"instant_state", // 消息类型，此处为即时状态消息
    "message":{
      "state":"typing" // 即时状态，如 typing, uploading
      "user":{
        "id":"516055075accc1e4067dd5ff6b2682cd",
        "username":"tumayun",
        "nickname":"Tumayun"
      }
    }
  }
}
```

### 发布已读确认消息

**已读确认消息应该是接收者收到消息后发送给发送者（是个人而不是群组）的，该消息在后台将被标记为已读，且发送一条已读确认消息给发送者。已读确认消息可能会重复 ，应该在客户端标记已读后，忽略掉该条消息的其他已读确认消息**


客户端A发送给客户端B如下消息：

```
{
  "ext":{
    "version":"v1",
    "access_token":"jXgVN7qKwPrsZvrazr5o1429260353.954617"
  },
  "data":{
    "message_type":"mark_as_read", // 消息类型，此处为已读确认消息
    "message":{
      "max_id":"516055075accc1e4067dd5ff6b2682cd",
      "recipient_id":<id>, // 客户端 B 的 user id
      "recipient_type":"User"
    }
  }
}
```

客户端B将会接收到如下消息：

```
// 客户端可以通过 recipient_id 和 recipient_type 确定聊天窗口，然后将聊天窗口我发送消息中的 `created_at` 小于等于 `last_read_at` 的都标记为已读。
{
  "data":{
    "message_type":"mark_as_read", // 消息类型，此处为已读确认消息
    "message":{
      "last_read_at":1445596604.144 // 最后读取的时间，其实是 max_id 对应消息的 created_at 值
      "recipient_type":"User",
      "recipient_id":<id> // 客户端 A 的 user id
    }
  }
}
```

### 发布聊天消息

该操作会在后台创建一条消息，并且发送给接收者


客户端A发送给客户端B如下消息：

`message` 中的参数请参考 [API Server](https://github.com/CatchChat/catch-wiki/blob/master/park_api_reference_v1.md#%E5%8F%82%E6%95%B0-28)

```
{
  "ext":{
    "version":"v1",
    "access_token":"jXgVN7qKwPrsZvrazr5o1429260353.954617"
  },
  "data":{
    "message_type":"message", // 消息类型，此处为聊天消息
    "message": { // message 中的参数请参考 API Server
      "text_content":"This is a test!",
      "recipient_type":"User",
      "recipient_id":"516055075accc1e4067dd5ff6b2682cd"
    }
  }
}
```

客户端A将会收到 Faye Server 返回的如下 response：

```
{
  "id":"6",
  "clientId":"lijii8id8xakwa1oh8dj9xc4zhc32pn",
  "channel":"/users/516055075accc1e4067dd5ff6b2682cd/messages",
  "successful":true, // 创建成功，失败则是 false
  "ext":{
    "message":{
      "id":"317a0e603c9cddbdbcd626a18066ceaa" // 创建成功的消息ID
    }
  }
}
```

客户端B将会收到如下消息：

`message` 中的参数请参考 [API Server](https://github.com/CatchChat/catch-wiki/blob/master/park_api_reference_v1.md#%E5%93%8D%E5%BA%94-27)

```
{
  "data":{
    "message_type":"message",
    "message":{
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
        <user>,
        "remarked_name":null,
        "contact_name":null
      },
      "circle":{
        "id":<id>,
        "name":"公共群组",
        "created_at":1433930183, // UNIX 时间戳
        "updated_at":1433930183, // UNIX 时间戳
      },
      "attachments":[
        {
          "kind":"image",
          "metadata":"metadata",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          }
        },
        {
          "kind":"thumbnail",
          "metadata":"metadata",
          "file":{
            "storage":"qiniu",
            "expires_in":86400, // 单位：秒
            "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
          }
        }
      ]
    }
  }
}
```

### 发布撤回消息请求

该操作会将消息撤回，并且发送推送给接收者，推送的`extras`为：

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

撤回消息请求如下：

```
{
  "ext":{
    "version":"v1",
    "access_token":"jXgVN7qKwPrsZvrazr5o1429260353.954617"
  },
  "data":{
    "message_type":"message_deleted", // 消息类型
    "message": {
      "id":<id> // 需要撤回的消息ID
    }
  }
}
```

消息的接收者们会收到如下消息：
```
{
  "data": {
    "message_type":"message_deleted", // 表示有消息被撤回
    "message": {
      "id":<id>, // 被撤回的消息ID
      "recipient_type":"User", // 接收者类型，User 表示是一对一消息，Circle 表示是群发消息，客户端可以借此确定要撤回的消息是在哪个聊天界面
      "recipient_id":<id>, // 接收者ID
      "sender": { // 消息发送者，也就是消息的撤回者
        "id":<id>,
        "username":"username",
        "nickname":"nickname"
      }
    }
  }
}
```

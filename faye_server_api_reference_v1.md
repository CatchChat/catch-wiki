# Faye Server API Reference

* Author: tumayun (tumayun@catch.la)

## 说明

此为 Faye Server v1 文档，Faye Server 是使用 [Faye](http://faye.jcoglan.com/) 框架的基于 [Bayeux](http://svn.cometd.org/trunk/bayeux/bayeux.html) 协议的消息系统。

客户端在 Faye Server 可以做的操作有：

* 订阅消息
* 发布消息

订阅/发布消息的频道有：

* Direct message `/users/:user_id/messages`
* Group message `/circles/:circle_id/messages`

## Version

在订阅发布等任何操作都必须在 `ext` 中加入 `version`，用来表明要使用 Faye Server 的哪个版本，如：

```
{ ext: { version: 'v1' } }
```


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

**已读确认消息应该是接收者收到消息后发送给发送者（是个人而不是群组）的，该消息在后台将被标记为已读，且发送一条已读确认消息给发送者。已读确认消息可能会重复 ，应该在收到第一个已读确认后，忽略掉该条消息的其他已读确认消息**

客户端A发送给客户端B如下消息：

```
{
  "ext":{
    "version":"v1",
    "access_token":"jXgVN7qKwPrsZvrazr5o1429260353.954617"
  },
  "data":{
    "api_version":"v1", // API Server Version，因为发布已读确认消息需要请求 API Server，所以需要提供 api_version
    "message_type":"mark_as_read", // 消息类型，此处为已读确认消息
    "message":{
      "id":"516055075accc1e4067dd5ff6b2682cd" // message id，这条消息将会在后台被标记为已读，且会发送给客户端B一条已读确认消息
    }
  }
}
```
客户端B将会接收到如下消息：

```
{
  "data":{
    "message_type":"mark_as_read", // 消息类型，此处为已读确认消息
    "message":{
      "id":"516055075accc1e4067dd5ff6b2682cd" // message id
      "recipient_type":"User", // 可能出现的值为 User 或者 Circle，表示该消息是个人消息还是群组消息
      "recipient_id":"516055075accc1e4067dd5ff6b2682cd" // 接收者 ID
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
    "api_version":"v1", // API Server Version，因为发布聊天消息需要请求 API Server，所以需要提供 api_version
    "message_type":"message", // 消息类型，此处为聊天消息
    "message": { // message 中的参数请参考 API Server
      "text_content":"This is a test!",
      "recipient_type":"User",
      "recipient_id":"516055075accc1e4067dd5ff6b2682cd"
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
      "id":"317a0e603c9cddbdbcd626a18066ceaa",
      "recipient_id":"516055075accc1e4067dd5ff6b2682cd",
      "parent_id":"e8f3d46135cda79c413678e3ad91ddfc",
      "recipient_type":"User",
      "text_content":"This is a test.",
      "longitude":19.22,
      "latitude":110.33,
      "battery_level":50,
      "media_type":"text",
      "media_type_string":"一段文字",
      "state":"unread",
      "state_string":"未读",
      "created_at":"2015-04-17T10:03:37Z",
      "created_at_string":"2015年04月17日 10:03:37",
      "updated_at":"2015-04-17T10:03:37Z",
      "updated_at_string":"2015年04月17日 10:03:37",
      "sender":{
        "id":"516055075accc1e4067dd5ff6b2682cd",
        "username":"tumayun",
        "nickname":"Tumayun",
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "remarked_name":null,
        "contact_name":null
      },
      "attachments":[

      ]
    }
  }
}
```

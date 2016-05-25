# Yep Bot API Reference

* Author: Tumayun (tumayun@catch.la)

## 创建 Bot

通过 @botfather 机器人创建 Bot，根据 @botfather 的指示完成创建 Bot 的流程，拿到 websocket url 和 token.

@botfather 命令如下：

```
/newbot - create a new bot
/set_avatar - change bot profile photo
/set_introduction - change bot introduction
/set_nickname - change bot nickname
/token - get access token
/revoke - revoke bot access token
/deletebot - delete a bot
/cancel - cancel the current operation
```

## 连接 Websocket Server

通过 websocket url 连接到 websocket server.

### 处理实时消息的顺序

1. 忽略自己发出的消息
2. 收到消息后立即标记已读
3. Send typing
4. 回复消息

## 发送实时消息

**通过 websocket 发送一些实时消息**

### typing

```
{
  "message_type": "instant_state",
   "message": {
      "state": 0,
      "recipient_type": "User", // 接收者类型
      "recipient_id": "516055075accc1e4067dd5ff6b2682cd" // 接收者 ID
  }
}
```

## Bot Open API

**通过 token 请求 API**

### API URL

#### Staging

https://park-staging.catchchatchina.com/api

#### Production

```
https://api.soyep.com
```

### Header

所有 API 请求都需要包含如下 headers:

| header | 说明 |
|--------|--------|
| Accept-Language | 当前客户端所使用的语言，目前只支持 `en-US` 和 `zh-CN`，无效的设置将默认为 `en-US` |
| Authorization | 格式为 Token token="test-token"，将 test-token 即替换成真实 token 即可 |

### API Response

HTTP Code 大于等于 `200` 且小于 `300` 表示请求成功，反之则请求失败，失败会有错误信息，错误信息在 `error` 中，有些 API 由于客户端要根据不同的错误类型做处理，会附加一个 `code`，表示错误类型。

### 标记消息已读

```
PATCH /v1/:recipient_type/:recipient_id/messages/batch_mark_as_read
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
recipient_id | String | 是 | 接收者 ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID
recipient_type | String | 是 | 接受者类型，只能是 User（私聊） 或者 Circle（群聊）
max_id | String | 是 | 收到的消息 ID

#### 示例

```
curl -X PATCH https://api.soyep.com/v1/User/516055075accc1e4067dd5ff6b2682cd/messages/batch_mark_as_read -H 'Authorization: Token token="test-token"' -F max_id=<id>
```

#### 响应

```
{ "last_read_at":1445596604.144 }
```

### 发送消息

**目前 Bot 只支持发送 text 类型的消息**

发送图片视频语音消息时，需要先上传附件，拿到附件ID后再请求发送消息API。    
目前多设备通过 faye server message 进行新消息以及消息撤回的同步，faye server 会将消息同时推送给发送者和接收者的所有设备，为了避免消息重复，客户端应该用消息 ID 判断是否已经显示过改消息，忽略已经显示过的。    
当在群聊中提及（@）其他用户时，`text_content` 格式应该为 `@tumayun 你好`（符合正则`/(?:[^a-zA-Z0-9_!#\$%&*@＠]|^:?)[@＠]([a-zA-Z0-9_]{4,16})/`），此时如果用户`tumayun`没有屏蔽掉消息的发送者，且消息 5 秒内没有被撤回，则会自动添加`tumayun`为群组成员，并且[推送通知](#被用户提及)。

```
POST /v1/:recipient_type/:recipient_id/messages
```

#### 参数

名称 | 类型 | 是否必须 | 描述
--- |--- |--- |--- |
recipient_id | String | 是 | 接收者（聊天对象）ID，接收者只有两种，User 或者 Circle，所以是 User ID 或者 Circle ID |
recipient_type | String | 是 | 接受者（聊天对象）类型，只能是 users 或者 circles
media_type | String | 否 | 消息类型，text 表示文字, image 表示图片, video 表示视频, audio 表示语音, sticker 表示贴纸, location 表示位置, 默认是text
text_content | String | 否 | 文字内容，**只有是文字消息时才是必填字段，其他情况都是选填字段**
parent_id | String | 否 | 回复的消息 ID，表示当前要发送的消息是回复哪条消息
attachment_id | String | 否 | 附件 ID
longitude | Float | 否 | 只有位置消息才需要
latitude | Float | 否 | 只有位置消息才需要
random_id | String | 否 | // 客户端生成的随机 ID，可以用以防止发送消息时还没收到服务器的响应就已经收到 faye 消息的情况导致的消息重复问题

media_type 可选值：

可选值 | 解释
--- | --- |
text | 文字消息
image | 图片消息
video | 视频消息
audio | 语音消息
sticker | 贴纸消息
location | 位置消息

#### 示例

```
curl -X POST https://api.soyep.com/v1/Circle/516055075accc1e4067dd5ff6b2682cd/messages -d '{ "text_content": "This is a test!" }' -H 'Authorization: Token token="test-token"' -H "Content-Type: application/json"
```

#### 响应

##### 成功

```
{
  "id":516055075accc1e4067dd5ff6b2682cd,
  "random_id":"123456", // 客户端生成的随机 ID，可以用以防止发送消息时还没收到服务器的响应就已经收到 faye 消息的情况导致的消息重复问题
  "recipient_id":516055075accc1e4067dd5ff6b2682cd,
  "recipient_type":"Circle",
  "text_content":"Hello~",
  "latitude":113.033,
  "longitude":24.1231,
  "parent_id":0,
  "media_type":"image",
  "state":"unread",
  "created_at":1433930183, // UNIX 时间戳
  "updated_at":1433930183, // UNIX 时间戳
  "sender":{
    "id":516055075accc1e4067dd5ff6b2682cd,
    "username":"asdaasd",
    "nickname":"user9",
    "avatar":{
      "url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "thumb_url":"http://catch-avatars.qiniudn.com/thumb_sJAUYG6nc84glXkq.jpg"
    },
    "latitude":0.0,
    "longitude":0.0,
    "introduction":"",
    "badge":null,
    "website_url":"http://tumayun.com",
    "website_title":"Tumayun's Blog",
    "created_at":1441347808,
    "updated_at":1441693888,
    "last_sign_in_at":1441347780
  },
  "attachments":[
    {
      "id":516055075accc1e4067dd5ff6b2682cd,
      "metadata":"metadata",
      "file":{
        "expires_at":1454298804, // URL 过期时间
        "url":"http://catch.qiniudn.com/BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
        "thumb_url":"http://catch.qiniudn.com/thumb_BOmgCcbMqwaBs3OidTT2MbplmMLsCaIs.mp4?e=1419025369&token=YSMhpYfzim6GOG-_sqsm3C0CpWI7RAPeq5IxjHeD:MDp3E4cxzhderCN4zTWVlLc2Cs4="
      }
    }
    .
    .
    .
  ]
}
```

##### 失败

返回错误码

Error Code | 描述
--- |--- |
rejected_your_message | 当前用户已被对方屏蔽

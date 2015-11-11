# Faye Server API Reference

* Author: tumayun (tumayun@catch.la)

<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css">
 
<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>
 
<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript">
 $(document).ready(function(){
      $("h2,h3,h4,h5,h6").each(function(i,item){
        var tag = $(item).get(0).localName;
        $(item).attr("id","wow"+i);
        $("#category").append('<a class="new'+tag+'" href="#wow'+i+'">'+$(this).text()+'</a></br>');
        $(".newh2").css("margin-left",0);
        $(".newh3").css("margin-left",20);
        $(".newh4").css("margin-left",40);
        $(".newh5").css("margin-left",60);
        $(".newh6").css("margin-left",80);
      });
 });
</script>
<div id="category"></div>

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

在订阅发布等任何操作都必须在 `ext` 中加入 `version`，用来表明要使用 Faye Server 的哪个版本。如：

```
{ ext: { version: 'v1' } }
```

此时 Faye Server 版本为 v1。


## Authentication

只有订阅发布操作需要传入 access_token，用以表明身份，如：

```
{ ext: { version: 'v1', access_token: 'jXgVN7qKwPrsZvrazr5o1429260353.954617' } }
```

## 客户端向 faye server 发布消息

### 即时状态消息

用户正在输入，正在上传等即时状态消息

应向 faye server 提交如下消息体：

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

## 客户端收到 faye server 消息

收到的消息类型（message_type）有：

* [instant_state](#即时状态消息) （即时状态）
* [mark_as_read](#已读确认消息) （已读确认）
* [message](#有新的聊天消息) (聊天消息)
* [message_deleted](#撤回聊天消息) （聊天消息被撤回）

### 即时状态消息

用户正在输入，正在上传等即时状态消息

收到的 faye server 消息体如下：

```
{
  "message_type":"instant_state", // 消息类型，此处为即时状态消息
  "message":{
    "state":"typing" // 即时状态，如 typing, uploading
    "user":{  // 即时状态触发者
      "id":"516055075accc1e4067dd5ff6b2682cd",
      "username":"tumayun",
      "nickname":"Tumayun"
    }
  }
}
```

### 已读确认消息

向消息发送者发送通知，有人读取了自己发送的消息

收到的 faye server 消息体如下：

```
// 客户端可以通过 recipient_id 和 recipient_type 确定聊天窗口，然后将聊天窗口我发送消息中的 `created_at` 小于等于 `last_read_at` 的都标记为已读。
{
  message_type: 'mark_as_read',
  message: {
    "last_read_at":1445596604.144, // max_id 对应消息的 created_at 值
    "recipient_id":<id>,
    "recipient_type":"User"
  }
}
```

### 有新的聊天消息

表示收到一条新的聊天消息

收到的 faye server 消息体如下：

``` 
// `message` 中的参数请参考 Yep API
{
  "message_type": "message",
  "message": {
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
      <mini_user>,
      "remarked_name":null,
      "contact_name":null
    },
    "attachments":[
      <attachment>,
      .
      .
      .
    ]
  }
}
```

### 撤回聊天消息

发送者撤销聊天消息

```
{
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
```

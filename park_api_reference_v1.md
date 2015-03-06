# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

## Contact 通讯录

### 上传通讯录并返回已注册的通讯录好友

**覆盖式上传，上传后会删除之前的通讯录，返回已注册的通讯录好友，并且自动添加好友**

```
POST /api/v1/contacts/upload
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| contacts | JSON | 是 | contacts JSON format, 如 "[{\"name\":\"abc\",\"number\":\"15158166372\"},{\"name\":\"bac\",\"number\":\"15158166723\"}]"|

#### 示例

```
curl htts://park.catchchatchina.com/api/v1/contacts/upload -F contacts="[{\"name\":\"abc\",\"number\":\"15158160004\"},{\"name\":\"bac\",\"number\":\"15158160005\"}]" -H 'Authorization: Token token="sVNxda9nywMLZkuzUqf31422601654.468095"'
```

#### 响应

```
{
  "registered_contacts":[
    {
      "id":1,
      "name":"abc",
      "user":{
        "id":13,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "nickname":"user4",
        "mobile":"15158160004",
        "phone_code":"86"
      }
    },
    {
      "id":2,
      "name":"bac",
      "user":{
        "id":14,
        "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
        "nickname":"user5",
        "mobile":"15158160005",
        "phone_code":"86"
      }
    }
  ]
}
```

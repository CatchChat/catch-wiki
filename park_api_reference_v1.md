# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

## Attachment 附件

### 获取头像附件上传 token

GET /api/v4/attachments/public\_upload\_token

cURL 请求范例：

```

curl -X GET -H "Content-Type: application/json" -H 'Authorization: Token token="_mPSDhsxxYpJyVGc7qrU1422539900.9699185"' park.catchchatchina.com/api/v4/attachments/public_upload_token

```

返回范例：

```

{ 
  "provider":"qiniu",
  "options":{
    "token":"TJozzjvlXhkyA_OVOzHKYCavv2wq4L_afDTeYnhp:NlrhtV17EshCBuC8MsmH6QMHNSc=:eyJzY29wZSI6ImNhdGNoLWF2YXRhcnM6NDBiMmMwMzktZmNiZC00Y2EzLWIyNjAtNjBiNmRiZDUwY2RmIiwiY2FsbGJhY2tVcmwiOiJodHRwOi8vcGFyay5jYXRjaGNoYXRjaGluYS5jb20vYXBpL3Y0L2F0dGFjaG1lbnRzL3B1YmxpY19jYWxsYmFjay9xaW5pdSIsImNhbGxiYWNrQm9keSI6ImtleT0kKGtleSlcdTAwMjZidWNrZXQ9JChidWNrZXQpXHUwMDI2bWVzc2FnZV9pZD0kKHg6bWVzc2FnZV9pZCkiLCJkZWFkbGluZSI6MTQyMjU0Mzc3NX0=",
    "bucket":"catch-avatars",
    "key":"40b2c039-fcbd-4ca3-b260-60b6dbd50cdf",
    "download_url":"http://catch-avatars.qiniudn.com/40b2c039-fcbd-4ca3-b260-60b6dbd50cdf",
    "file_path":null,
    "file_length":null,
    "callback_url":"http://park.catchchatchina.com/api/v4/attachments/public_callback/qiniu",
    "callback_body":"key=$(key)\u0026bucket=$(bucket)\u0026message_id=$(x:message_id)"
  }
} 

```
### 获取Message附件上传 token


```
GET /api/v4/messages/:id/:kind/upload_token

```

| 名称 | 类型 | 描述 |
|---|---|---|
| id | String | message id|
| kind | String | message 类型,必须是以下3种之一： image, video, thumbnail|

不能单独请求上传，必须依赖一条已经生成的 Message，没有上传完成前，Message 的 state 是 drafted。


cURL 请求范例：
```
curl -X GET http://park.catchchatchina.com/api/v4/messages/9/image/upload_token -H 'Authorization: Token token="_mPSDhsxxYpJyVGc7qrU1422539900.9699185"'
```
返回范例：

```
{
  "provider":"qiniu",
  "options":{
    "token":"TJozzjvlXhkyA_OVOzHKYCavv2wq4L_afDTeYnhp:bNlrNprAnqTVtOt-j4aKmd2aARw=:eyJzY29wZSI6ImNhdGNoOmJmZWU0ZDQwLTZjYmMtNDMwNC05NmQzLTk2YzMwYThlNTBhYyIsImNhbGxiYWNrVXJsIjoiaHR0cDovL3BhcmsuY2F0Y2hjaGF0Y2hpbmEuY29tL2FwaS92NC9hdHRhY2htZW50cy9jYWxsYmFjay9xaW5pdSIsImNhbGxiYWNrQm9keSI6ImtleT0kKGtleSlcdTAwMjZidWNrZXQ9JChidWNrZXQpXHUwMDI2bWVzc2FnZV9pZD0kKHg6bWVzc2FnZV9pZCkiLCJkZWFkbGluZSI6MTQyMjU0NTM2MX0=",
    "message_id":9,
    "bucket":"catch",
    "key":"bfee4d40-6cbc-4304-96d3-96c30a8e50ac",
    "file_path":null,
    "file_length":null,
    "callback_url":"http://park.catchchatchina.com/api/v4/attachments/callback/qiniu",
    "callback_body":"key=$(key)\u0026bucket=$(bucket)\u0026message_id=$(x:message_id)",
    "notify_url":null
  }
}

```


获取上传 token 后，根据 provider 上传到相应的云存储，options 中包含了客户端上传所需要的参数。

七牛参数：

| 名称 | 类型 | 描述 |
|---|---|---|
| token | String | 上传凭证 |
| bucket | String | Bucket 的名称 |
| message\_id | String | Message ID|
| key | String | 文件名 |
| callback_url | String | callback url|
| callback_body | String | callback body|


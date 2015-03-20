# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

## Attachment 附件

### 获取七牛头像附件上传 token

GET /api/v1/attachments/public\_upload\_token

cURL 请求范例：

```

curl -X GET -H "Content-Type: application/json" -H 'Authorization: Token token="_mPSDhsxxYpJyVGc7qrU1422539900.9699185"' park.catchchatchina.com/api/v1/attachments/public_upload_token

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
    "callback_url":"http://park.catchchatchina.com/api/v1/attachments/public_callback/qiniu",
    "callback_body":"key=$(key)\u0026bucket=$(bucket)\u0026message_id=$(x:message_id)"
  }
} 

```
### 获取S3头像附件上传 token

GET /api/v1/attachments/s3upload\_public\_form_fields

cURL 请求范例：

```
curl -X GET -H 'Authorization: Token token="test-token"' -H "Content-Type:
application/json"
http://park.catchchatchina.com/api/v1/attachments/s3_upload_public_form_fields

```
返回范例：

```
{"provider":"s3","options":{"bucket":null,"key":"61e861c2-be37-4033-9c85-685347657481","url":"https://.s3.cn-north-1.amazonaws.com.cn/","policy":{"expiration":"2015-03-20T08:48:20.000Z","conditions":[{"bucket":null},{"key":"61e861c2-be37-4033-9c85-685347657481"},{"acl":"private"},{"x-amz-credential":"AKIAOGBVMZAU5EZPGPIQ/20150320/cn-north-1/s3/aws4_request"},{"x-amz-algorithm":"AWS4-HMAC-SHA256"},{"x-amz-date":"20150320T074820Z"}]},"encoded_policy":"eyJleHBpcmF0aW9uIjoiMjAxNS0wMy0yMFQwODo0ODoyMC4wMDBaIiwiY29uZGl0aW9ucyI6W3siYnVja2V0IjpudWxsfSx7ImtleSI6IjYxZTg2MWMyLWJlMzctNDAzMy05Yzg1LTY4NTM0NzY1NzQ4MSJ9LHsiYWNsIjoicHJpdmF0ZSJ9LHsieC1hbXotY3JlZGVudGlhbCI6IkFLSUFPR0JWTVpBVTVFWlBHUElRLzIwMTUwMzIwL2NuLW5vcnRoLTEvczMvYXdzNF9yZXF1ZXN0In0seyJ4LWFtei1hbGdvcml0aG0iOiJBV1M0LUhNQUMtU0hBMjU2In0seyJ4LWFtei1kYXRlIjoiMjAxNTAzMjBUMDc0ODIwWiJ9XX0=","signature":"60165b75396f9ab5f32c22014cef9de576da2d7423de872fc6ce1e20dd8ab5a8"}}

```

### 获取Message附件上传 token

#### 七牛
```
POST /api/v1/messages/:id/:kind/upload_token

```

| 名称 | 类型 | 描述 |
|---|---|---|
| id | String | message id|
| kind | String | message 类型,必须是以下3种之一： image, video, thumbnail|

不能单独请求上传，必须依赖一条已经生成的 Message，没有上传完成前，Message 的 state 是 drafted。


cURL 请求范例：
```
curl -X POST
http://park.catchchatchina.com/api/v1/messages/9/image/upload_token
-H 'Authorization: Token token="_mPSDhsxxYpJyVGc7qrU1422539900.9699185"''
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
    "callback_url":"http://park.catchchatchina.com/api/v1/attachments/callback/qiniu",
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

#### S3

获得S3上传所需的提交的form的参数

具体S3上传需要的格式可参考S3文档或Rails里的上传例子
[S3文档](http://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-post-example.html)

```
POST /api/v1/messages/:id/:kind/s3_upload_form_fields

```

| 名称 | 类型 | 描述 |
|---|---|---|
| id | String | message id|
| kind | String | message 类型,必须是以下3种之一： image, video, thumbnail|

不能单独请求上传，必须依赖一条已经生成的 Message，没有上传完成前，Message 的 state 是 drafted。


cURL 请求范例：
```
curl -X POST -H "Content-Type: application/json" -H 'Authorization: Token
token="_mPSDhsxxYpJyVGc7qrU1422539900"' -d ''
http://park.catchchatchina.com/api/v1/messages/3/image/s3_upload_form_fields'

```
返回范例：

```
{
  "provider":"s3",
  "options":{
    "message_id":3,
    "bucket":"ruanwz-test",
    "key":"7f597cbd-0c4c-427f-b267-8292b33983d3",
    "url":"https://ruanwz-test.s3.cn-north-1.amazonaws.com.cn/",
    "policy": {
      "expiration":"2015-03-12T08:05:08.000Z",
      "conditions":[
        {"bucket":"ruanwz-test"},
        {"key":"7f597cbd-0c4c-427f-b267-8292b33983d3"},
        {"acl":"private"},
        {"x-amz-credential":"AKIAOGBVMZAU5EZPGPIQ/20150312/cn-north-1/s3/aws4_request"},
        {"x-amz-algorithm":"AWS4-HMAC-SHA256"},{"x-amz-date":"20150312T070508Z"}]
      },
    "encoded_policy":"eyJleHBpcmF0aW9uIjoiMjAxNS0wMy0xMlQwODowNTowOC4wMDBaIiwiY29uZGl0aW9ucyI6W3siYnVja2V0IjoicnVhbnd6LXRlc3QifSx7ImtleSI6IjdmNTk3Y2JkLTBjNGMtNDI3Zi1iMjY3LTgyOTJiMzM5ODNkMyJ9LHsiYWNsIjoicHJpdmF0ZSJ9LHsieC1hbXotY3JlZGVudGlhbCI6IkFLSUFPR0JWTVpBVTVFWlBHUElRLzIwMTUwMzEyL2NuLW5vcnRoLTEvczMvYXdzNF9yZXF1ZXN0In0seyJ4LWFtei1hbGdvcml0aG0iOiJBV1M0LUhNQUMtU0hBMjU2In0seyJ4LWFtei1kYXRlIjoiMjAxNTAzMTJUMDcwNTA4WiJ9XX0=",
    "signature":"e7f94131b773e3c0db6fd20af1ac471b2a48de99012d23009ba2baf305219f50"}}
```


获取上传 form所需参数后，上传到S3, S3会回调设置的endpoint(需在AWS控制台设置)。


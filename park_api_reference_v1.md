# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

## UnfriendRequest 解除好友请求

### 新建请求解除好友关系

```
POST /api/v1/unfriend_requests
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| friend_id | Integer | 是 | 好友 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/unfriend_requests -F friend_id=2 -H 'Authorization: Token token=yZp5UZMeCB8yKBUy_ae81416827431.104971'
```

#### 响应

```
{}
```
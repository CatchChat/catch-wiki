# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

## Report 消息举报

### 举报

```
POST /api/v1/reports
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| message_id | Integer | 是 | 要举报的消息 ID |

#### 示例

```
curl -X POST https://park.catchchatchina.com/api/v1/reports -F message_id=1 -H 'Authorization: Token token="6Y62Cc4FQ1GVw3ZiLqzq1418393864.8241951"'
```

#### 响应

```
{}
```
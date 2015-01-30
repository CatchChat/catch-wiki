# Park API Reference

* Author: Kgen (kgen@catch.la)

## 概述

* 基于 HTTP
* RESTful
* 采用标准 HTTP Code 返回错误
* 返回格式为 JSON

## Users 用户

### 搜索用户

```
GET api/v1/users/search
```

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| q | String | 是 | mobile |

#### 示例

```
curl https://park.catchchatchina.com/api/v1/users/search\?q\=15158161111 -H 'Authorization: Token token="EtErCK18xN9pxakiCPp61418029033.582837"'
```

#### 响应

```
{
  "users":[
    {
      "id":8,
      "nickname":"tumayun",
      "avatar_url":"http://catch-avatars.qiniudn.com/sJAUYG6nc84glXkq.jpg",
      "phone_code":"86",
      "mobile":"15158161111"
    }
  ],
  "current_page":1,
  "per_page":30,
  "count":1
}
```

### 校验手机号是否可用（无需登录）

```
GET api/v1/users/mobile_validate
```
** phone code 合法, mobile 合法，且具有唯一性    
在设置中修改手机号时，不能拿当前手机号去校验，否则会返回手机号已经被使用**

#### 参数

| 名称 | 类型 | 是否必需 | 描述 |
|---|---|---|---|
| phone_code | String | 是 | 国家码 |
| mobile | String | 是 | 手机号 |

#### 示例

```
curl https://park.catchchatchina.com/api/v1/users/mobile_validate\?phone_code\=86\&mobile\=15158166372
```

#### 响应

* 手机号可用

```
{  
  "available":true
}
```

* 手机号不可用

```
{  
  "available":false,
  "message":"手机号已经被使用"
}
```
[TOC]

#用户管理API

## 获取用户详情

https请求方式GET

```
{INDUSTRY_DOMAIN}/user/{userid}?access_token=ACCESS_TOKEN
```

返回包体

```
{
    "code": 0,
    "msg": "ok",
    "name": "user name",
    "email": "email",
    "tel": "123123123",
    "status": 1,
    "roles": [
        {
            "corp_id": "corpid1", 
            "role": "admin"
        },
        {
            "corp_id": "corpid2", 
            "role": "user"
        }
        ...
    ]
}
```

## 获取用户详情列表

https请求方式POST

```
{INDUSTRY_DOMAIN}/users?access_token=ACCESS_TOKEN
```

请求JSON包体

```
{
    "user_ids": ["userid1", "userid2"]
}
```


| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| access_token | 是  | 公共参数，调用接口凭证 |
| user_ids|  是  | 被查询的用户userid列表,最多不超过100个|

返回JSON包体

```  
      
{
    "code": 0,
    "msg": "ok",
    "users": [
        {
            "user_id": "userid",
            "name": "用户1", // 姓名
            "gender": 1, // 性别
            "tel": "18902387651", // 电话
            "email": "eux@hotmail.com", // email
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
              ...
            "roles": [
                {
                    "corp_id": "corpid1", // 用户所属企业的id
                    "role": "admin" // 用户角色：admin: 企业管理员，user: 企业普通用户
                },
                {
                    "corp_id": "corpid2", // 用户所属企业的id
                    "role": "user" // 用户角色：admin: 企业管理员，user: 企业普通用户
                }
            ]
        }, 
    ]
}
```


## 订阅用户信息变更通知

http(s)请求方式POST:

```
CLIENT_SUBSCRIBE_URI
```

请求JSON包体

```
{
    "topic": "userChange",
    "changelist": [
        {
            "change_type": "add", // 新增一个user
            "user_id": "userid",
            "name": "name",
            "gender": 1,
            "tel": "tel",
            "email": "email"
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corp_id": "corpid",
                    "role": "user"
                }
            ]
        },
        {
            "change_type": "modify", // 修改一个user
            "user_id": "userid",
            "name": "name",
            "gender": 1,
            "tel": "tel",
            "email": "email"
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corp_id": "corpid",
                    "role": "user"
                }
            ]
        },
        {
            "change_type": "delete", //删除一个user
            "user_id": "userid",
        }
    ]
}
```

返回说明

```
{
    "code": 0,
    "msg": "ok"
}
```

> 前置要求： 应用为工业云内部应用，且已注册订阅URI


[TOC]

#用户管理API

## 获取用户详情

请求方式GET

```
http://129.211.44.155:8088/api/v1/user/{userid}?access_token=ACCESS_TOKEN
```

返回包体

```
{
    "Code": 0,
    "Msg": "ok",
    "Name": "user name",
    "Email": "email",
    "Tel": "123123123",
    "Status": 1,
    "Roles": [
        {
            "CorpId": "corpid1", 
            "Role": "admin"
        }...
    ],
    "UserRole": 0, 
}
```

| 参数 | 类型 | 说明 |
| -- | -- | -- |
| Code | int | 错误码 |
| Msg | string | 消息 |
| Name | string | 用户名 |
| Email | string | email |
| Tel | string | 电话 |
| Status | int | 用户状态. 0:账户未激活,1:账户已激活,2:账户认证中,3:账户认证通过,4:账户认证拒绝 |
| Roles | array | 用户企业角色列表（用户可能属于多个企业） |
| CorpId | string | 企业 id |
| Role | int | 用户在企业里的角色：1: 企业管理员，0: 企业普通用户 |
| UserRole | int | 0普通用户 10管理员（运营商或超级管理员) |

## 获取用户详情列表

https请求方式POST

```
http://129.211.44.155:8088/api/v1/users?access_token=ACCESS_TOKEN
```

请求JSON包体

```
{
    "UserIds": ["userid1", "userid2"]
}
```


| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| UserIds|  是  | 被查询的用户userid列表,最多不超过100个|

返回JSON包体

```  
      
{
    "Code": 0,
    "Msg": "ok",
    "Users": [
        {
            "UserId": "userid",
            "Name": "用户1", // 姓名
            "Gender": 1, // 性别
            "Tel": "18902387651", // 电话
            "Email": "eux@hotmail.com", // email
            "Id": "id", // 身份证号,
            "Status": 0, // 0: 未实名认证，1： 已实名认证
            "Roles": [
                {
                    "CorpId": "corpid1", // 用户所属企业的id
                    "Role": 0 // 用户角色：1: 企业管理员，0: 企业普通用户
                },
                ...
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
    "Topic": "userChange",
    "ChangeList": [
        {
            "ChangeType": "add", // 新增一个user
            "UserId": "userid",
            "Name": "name",
            "Gender": 1,
            "Tel": "tel",
            "Email": "email"
            "Id": "id", // 身份证号,
            "State": 0, // 0: 未实名认证，1： 已实名认证
            "Roles": [
                {
                    "CorpId": "corpid",
                    "Role": 0
                }
            ]
        },
        {
            "ChangeType": "modify", // 修改一个user
            "UserId": "userid",
            "Name": "name",
            "Gender": 1,
            "Tel": "tel",
            "Email": "email"
            "Id": "id", // 身份证号,
            "State": 0, // 0: 未实名认证，1： 已实名认证
            "Roles": [
                {
                    "CorpId": "corpid",
                    "Role": "user"
                }
            ]
        },
        {
            "ChangeType": "delete", //删除一个user
            "UserId": "userid",
        }
    ]
}
```

返回说明

```
{
    "Code": 0,
    "Msg": "ok"
}
```

> 前置要求： 应用为工业云内部应用，且已注册订阅URI

## 异步通知工业云用户是否可删除

请求方式POST

```
http://129.211.44.155:8088/api/v1/subscribe_notification?signature={sign}
```

请求包体

```
{
    "Code": 0,
    "Msg": "不能删除用户: 用户xxx拥有产品xxx权限",
    "Type": "DeleteUser",
    "UserId": "userid",
}
```

| 字段 | 类型 | 说明 |
| -- | -- | -- |
| Code | int | 错误码, 0为成功，其他为错误 |
| Msg | string | 失败提示 |
| Type | string | "DeleteUser" 删除用户 |
| UserId | string | 所删除的用户id |
| signature | string | 签名，用于验证请求来自寄云（而不是攻击者）|

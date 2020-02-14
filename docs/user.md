[TOC]

#用户管理API

## 获取用户详情

请求方式GET

```
http://{CloudIndustryHost}/iam/api/v1/user/{userid}?access_token=ACCESS_TOKEN
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
            "Role": "admin",
            "CorpStatus": 2,
            "CorpType": 3,
            "CorpName": "企业名"
        }...
    ],
    "UserRole": 0, 
    "CreateType": 1,
    "SubAccount": false
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
| Role | int | 用户在企业里的角色：1:企业管理员，0:企业普通用户 |
| CorpStatus | int | 企业状态：0:草稿 1:审核中 2:审核通过 3:审核驳回 |
| CorpType | int | 企业类型: 1:普通企业 2:服务提供商 3:医院 10:内部企业 |
| CorpName | string | 企业名称 |
| UserRole | int | 0:普通用户 10:管理员（运营商或超级管理员) |
| CreateType | int | 1:自注册用户 2:企业管理员注册用户 3:微信登录注册用户 10:系统创建用户 |
| SubAccount | bool | true:是子账号 false:非子账号 |

`注`：目前用户于企业是一对一的关系，Roles数组下只会有一项数据表示用户所属企业信息


## 获取用户详情列表

https请求方式POST

```
http://{CloudIndustryHost}/iam/api/v1/users?access_token=ACCESS_TOKEN
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
            "Status": 0, // 0:账户未激活,1:账户已激活,2:账户认证中,3:账户认证通过,4:账户认证拒绝
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
            "Status": 0, // 0:账户未激活,1:账户已激活,2:账户认证中,3:账户认证通过,4:账户认证拒绝
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
        },
        {
            "ChangeType": "deleteCorpUser", // 从企业移出user
            "DelUserId": "userid", // 要移出的userid
            "CorpId": "corpid", // 移出的corpid
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

## 通知云平台用户删除状态

请求方式POST

```
POST 10.1.1.17/api3

X-TC-Action NotifyUserDelStage
X-TC-Version v1
X-TC-Timestamp 1556977841
X-TC-Region hz
Authorization xyz
```

特殊头部是用于验证请求来自寄云，而不是第三方攻击者。头部生成规则与腾讯云API3.0完全一致，头部的生成有python示例，生成方法如下:

1. 查看企业秘钥对SECRET_ID, SECRET_KEY，前往查看[企业秘钥对](http://10.1.1.17/cp/identity/corp")
2. 下载客户端sdk，前往[下载算法市场sdk]("https://github.com/XWSTeam/cloudindustry-algo-sdk")，使用sdk生成api3.0规范的头部

请求包体

```json
{
    "Code": -1,
    "UserId": "id",
    "CorpId": "id",
    "ErrMsg": "内部错误消息",
    "Msg": "删除用户失败，用户正在使用app xyz"
}
```

返回包体

```json
{
    "Code": 0,
    "Msg": "ok"
}
```

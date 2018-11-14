#用户管理API
[TOC]



##获取用户详情

### 接口描述

通过 corp_id+user_id，可以在一个企业内唯一确定一个用户，通过免登授权可以获取到当前用户的user_id，再通过user_id调用本接口，获取用户的详情

https请求方式POST

```
PRE_URL/cgi-bin/user?access_token=ACCESS_TOKEN
```
### 参数说明

请求JSON包体

```
{
    "user_ids": ["userid"]
}
```


| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| access_token | 是  | 公共参数，调用接口凭证 |
| user_ids|  是  | 被查询的用户userid列表,最多不超过100个|

### 返回说明

```  
      
{
    "code" : 0,
    "userlist": [
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
}
```

> 前置要求： 应用为工业云内部应用，且已注册订阅URI


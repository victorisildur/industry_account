#用户管理API
[TOC]



##获取用户详情

### 1.1接口描述
接口请求域名：api.cloud_industry.qcloud.com 。

通过 CorpId+userid，可以在一个企业内唯一确定一个用户，通过免登授权可以获取到当前用户的userid，再通过userid调用本接口，获取用户的详情

https请求方式GET

```
https://api.cloud_industry.qcloud.com/cgi-bin/user/access_token=ACCESS_TOKEN
```
### 1.2传入参数


| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
| access_token | 是 | String | 公共参数，调用接口凭证 |
| query_users|  是| Array  | 被查询的用户userid列表 |
 
### 1.3返回说明

```   1        10
limit start， offset
      
{
    "code" : 0,
    "total" :1000,
    "userlist__": [
        {
            "userId": "userid",
            "name": "用户1", // 姓名
            "gender": 1, // 性别
            "tel": "18902387651", // 电话
            "email": "eux@hotmail.com", // email
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
              ...
            "roles": [
                {
                    "corpId": "corpid1", // 用户所属企业的id
                    "role": "admin" // 用户角色：admin: 企业管理员，user: 企业普通用户
                },
                {
                    "corpId": "corpid2", // 用户所属企业的id
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
    "changeList": [
        {
            "changeType": "add", // 新增一个user
            "userId": "userid",
            "name": "name",
            "gender": 1,
            "tel": "tel",
            "email": "email"
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corpId": "corpid",
                    "role": "user"
                }
            ]
        },
        {
            "changeType": "modify", // 修改一个user
            "userId": "userid",
            "name": "name",
            "gender": 1,
            "tel": "tel",
            "email": "email"
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corpId": "corpid",
                    "role": "user"
                }
            ]
        },
        {
            "changeType": "delete", //删除一个user
            "userId": "userid",
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


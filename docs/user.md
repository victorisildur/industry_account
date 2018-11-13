#用户管理API
[TOC]
##获取用户信息

### 4.1接口描述
接口请求域名：api.cloud_industry.qcloud.com 。



默认接口请求频率限制：1000次/每秒。

注意事项：

https请求方式POST

```
https://api.cloud_industry.qcloud.com/cgi-bin/user
```
### 4.2传入参数


| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
|Action  | 是 | String |公共参数，本接口取值：getUserInfo
  |
| sessionid	 | 是 | String | 公共参数 一次请求对应一个SessionId，会原样返回，建议传入类似于uuid的字符串防止重复 |
| access_token | 是 | String | 公共参数 |
| appid |是| String |公共参数 第三方应用实例，唯一标识 |
| userid | 是 |String  | 执行操作的用户身份唯一标识 |
| corpid | 否 |String  | 查询指定企业下该用户的信息 |
| query_users|  是| Array  | 被查询的用户userid列表 |
   
   
   
   
### 4.3返回说明

```
{
    "code" : 0,
    "users": [
        {
            "userId": "userid",
            "name": "用户1", // 姓名
            "gender": 1, // 性别
            "tel": "18902387651", // 电话
            "email": "eux@hotmail.com", // email
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
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


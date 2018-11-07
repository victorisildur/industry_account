# 员工管理API

## 获取员工信息

https请求方式GET

```
https://api.cloud_industry.qcloud.com/cgi-bin/org/user/{userid1,userid2...}&access_token={access_token}&appId={appId}
```


返回说明

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
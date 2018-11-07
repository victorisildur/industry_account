# APP管理API

## 创建单点登录应用

https请求方式POST

https请求方式:

https://api.cloud_industry.qcloud.com/cgi-bin/org/app?access_token={access_token}

请求JSON说明

```
{
    "userId":  "userid", // userId
    "corpId": "corpid1" // 企业CorpId
}
```

返回说明

```
{
   "code" : 0,
   "loginAppId": "6ea90b347c0fd", // 单点登录login appid
   "loginAppSecret": "23d8xh3yx93j7", // 单点登录login app secret
   "adminUserId": "adminUserId"  // 默认创建应用管理员
}
```


## 开通/关闭用户单点登录应用的权限

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/app/user?access_token=access_token

https请求方式POST

POST数据格式：JSON

POST数据例子

```
{
 "loginAppId": "appid",  // 应用的LoginAppId
 "userId": "userid", // 开通用户的userid
 "corpId": "corpid", // 开通用户所属企业的corpid
 "action": "enable" // "enable"表示开通，"disable"表示关闭
}
```

返回说明

```
{
    "code" : 0
}
```
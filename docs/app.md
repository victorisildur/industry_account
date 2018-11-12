# 应用管理API

## 创建单点登录应用

https请求方式POST

```
https://api.cloud_industry.qcloud.com/cgi/app?access_token={access_token}
```

请求JSON说明

```
{
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

## 删除单点登录应用

https请求方式DELETE

```
https://api.cloud_industry.qcloud.com/cgi/app?access_token={access_token}
```

请求JSON说明

```
{
    "loginAppId": "appid1" 
}
```

返回说明

```
{
   "code" : 0
}
```


## 开通/关闭用户单点登录应用的权限

https请求方式PUT

```
https://api.cloud_industry.qcloud.com/cgi-bin/org/app/user?access_token=access_token
```


请求JSON包体

```
{
 "loginAppId": "appid",  // 应用的LoginAppId
 "corpId": "corpid", // 开通用户所属企业的corpid
 "users": ["userid1", "userid2", "userid3"] // 批量开启/关闭
 "action": "enable" // "enable"表示开通，"disable"表示关闭
}
```

返回说明

```
{
    "code" : 0
}
```
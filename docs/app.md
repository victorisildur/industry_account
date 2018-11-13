# 应用管理API

## 创建单点登录应用

https请求方式POST

```
https://api.cloud_industry.qcloud.com/cgi/app?access_token={access_token}
```

请求JSON说明

```
{
    "corp_id": "corpid1" // 企业CorpId
    "name": "app name", // 应用名称
    "logo": "logo url", // 应用logo url
    "redirect_fqdn": "www.qq.com" // 回调地址的FQDN
}
```

返回说明

```
{
   "code" : 0,
   "login_appid": "6ea90b347c0fd", // 单点登录login appid
   "login_appsecret": "23d8xh3yx93j7", // 单点登录login app secret
   "virtual_admin": "adminUserId"  // 为第三方应用创建应用管理员
}
```

## 删除单点登录应用

https请求方式DELETE

```
https://api.cloud_industry.qcloud.com/cgi/app/{login_appid}?access_token={access_token}
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
https://api.cloud_industry.qcloud.com/cgi-bin/org/app/user?access_token={access_token}
```


请求JSON包体

```
{
 "login_appid": "appid",  // 应用的LoginAppId
 "corp_id": "corpid", // 开通用户所属企业的corpid
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

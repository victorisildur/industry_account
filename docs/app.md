# 应用管理API

## 创建单点登录应用

https请求方式POST

```
https://{INDUSTRY_DOMAIN}/login_app?access_token={access_token}
```

请求JSON说明

```
{
    "corp_id": "corpid1" // 企业CorpId
    "name": "app name", // 应用名称
    "logo": "logo url", // 应用logo url
    "domain": "www.qq.com", // 回调地址的域名
    "subscribe_uri": "example.com/subscrieb", // 订阅uri
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
https://{INDUSTRY_DOMAIN}/login_app/{login_appid}?access_token={access_token}
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
https://{INDUSTRY_DOMAIN}/login_app/{login_appid}/users?access_token={access_token}
```


请求JSON包体

```
{
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

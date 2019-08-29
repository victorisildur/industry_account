[TOC]

# 应用管理API

## 创建单点登录应用

https请求方式POST

```
http://{CloudIndustryHost}/iam/api/v1/login_app?access_token={access_token}
```

请求JSON说明

```
{
    "CorpId": "corpid1" // 企业CorpId
    "Name": "app name", // 应用名称
    "Logo": "logo url", // 应用logo url
    "Domain": "www.qq.com", // 回调地址的域名
    "SubscribeUri": "example.com/subscrieb", // 订阅uri
}
```

返回说明

```
{
   "Code" : 0,
   "Msg" : "ok",
   "LoginAppid": "6ea90b347c0fd", // 单点登录login appid
   "LoginAppsecret": "23d8xh3yx93j7", // 单点登录login app secret
   "VirtualAdmin": "adminUserId"  // 企业虚拟管理员id
}
```

## 删除单点登录应用

https请求方式DELETE

```
http://{CloudIndustryHost}/iam/api/v1/login_app/{login_appid}?access_token={access_token}
```

返回说明

```
{
   "Code" : 0,
   "Msg" : "ok"
}
```


## 开通/关闭用户单点登录应用的权限

https请求方式POST

```
http://{CloudIndustryHost}/iam/api/v1/login_app/{login_appid}/users?access_token={access_token}
```


请求JSON包体

```
{
 "CorpId": "corpid", // 开通用户所属企业的corpid
 "Users": ["userid1", "userid2", "userid3"] // 批量开启/关闭
 "Action": "enable" // "enable"表示开通，"disable"表示关闭
}
```

返回说明

```
{
   "Code" : 0,
   "Msg" : "ok"
}
```

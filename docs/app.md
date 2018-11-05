# APP管理API

## 开通/关闭用户单点登录应用的权限

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/app_user?access_token=access_token

https请求方式POST

POST数据格式：JSON

POST数据例子

```
{
 "appId": "appid",  // 应用实例的AppId
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
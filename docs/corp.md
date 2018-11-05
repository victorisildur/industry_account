# 企业管理API

## 获取企业信息

https请求方式GET

https请求方式:
https://api.cloud_industry.qcloud.com/cgi-bin/org/corp?corp={id}&access_token={access_token}

返回说明
```
{
   "code" : 0,
   "userCount": 5,
   "users" : [ "userid1" , "userid2" , "userid3" ...],
   "admin": "userid1"
}
```

## 创建新应用

https请求方式POST

https请求方式:

https://api.cloud_industry.qcloud.com/cgi-bin/org/corp?access_token={access_token}

请求JSON说明

```
{
    "appId":  "782b67da98ce", // 应用的产品AppId
    "corpId": "corpid1" // 企业CorpId
}
```

返回说明

```
{
   "code" : 0,
   "appId": "6ea90b347c0fd",
   "appSecret": "23d8xh3yx93j7"
}
```
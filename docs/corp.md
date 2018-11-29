[TOC]

# 企业管理API

## 获取企业基础信息

请求方式POST

```
{INDUSTRY_DOMAIN}/corps?access_token=ACCESS_TOKEN
```

请求JSON包体

```
{
    "corpids": ["001","002"]
}
```

| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| corpids | 是   | 企业的id列表，最多不能超过50个|

返回JSON包体

```
{
   "errcode": 0,
   "errmsg": "ok",
   "corps": [
        {
            "corp_id": "id",
            "name": "name",
            "logo": "logo",
            "email": "email",
            "tel": "tel",
            "addr": "address",
            "type": 0,
            "status": 1,
           ...
        }
   ]
}
```

## 获取企业下用户详情列表

请求方式GET

```
{INDUSTRY_DOMAIN}/corp/{corpid}/users?access_token={access_token}&offset=0&size=100
```

| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
| access_token | 是 | String | 公共参数，调用接口凭证 |
| corp_id | 否 |String  | 企业唯一标识 |
| offset|否|long|支持分页查询，与size参数同时设置时才生效，此参数代表偏移量|
| size |否|int|支持分页查询，与offset参数同时设置时才生效，此参数代表分页大小，最大100，排序说明-代表按照用户关联到企业账号时间升序


返回JSON包体

```
{
    "code": 0,
    "msg": "ok",
    "users":[
        {
            "name": "user name",
            "email": "email",
            "phone": "123123123",
            "status": 1,
            "role": "admin"
        }
    ]
}
```

## 订阅企业信息变更通知

请求方式POST

```
CLIENT_SUBSCRIBE_URI
```

请求JSON包体:

```
{
    "topic": "corpChange",
    "changelist": [
        {
            "corp_id": "corpid",
            "user_count": 5,
            "users" : [ "userid1" , "userid2" , "userid3" ...],
            "admins": [ "userid1", "userid2" ...]
            "name": "企业名称",
            "addr": "地址",
            "contacts": "张三"
        }
    ]
}
```

返回说明：

```
{
    "code": 0
}
```
> 前置条件：应用为工业云内部应用，且已注册订阅URI


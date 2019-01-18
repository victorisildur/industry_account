[TOC]

# 企业管理API

## 获取企业基础信息

请求方式POST

```
http://129.211.44.155:8088/api/v1/corps?access_token=ACCESS_TOKEN
```



请求JSON包体

```
{
    "CorpIds": ["001","002"]
}
```

| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| CorpIds | 是   | 企业的id列表，最多不能超过50个|

返回JSON包体

```
{
   "Code" : 0,
   "Msg" : "ok",
   "Corps": [
        {
            "CorpId": "id",
            "Name": "name",
            "Logo": "logo",
            "Email": "email",
            "Tel": "tel",
            "Addr": "address",
            "Type": 0,
            "Status": 1,
           ...
        }
   ]
}
```

## 获取企业下用户详情列表

请求方式GET

```
http://129.211.44.155:8088/api/v1/corp/{corpid}/users?access_token={access_token}&offset=0&size=100
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
    "Code" : 0,
    "Msg" : "ok",
    "Users":[
        {
            "Name": "user name",
            "Email": "email",
            "Phone": "123123123",
            "Status": 1,
            "Role": 10
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
    "Topic": "corpChange",
    "ChangeList": [
        {
            "CorpId": "corpid",
            "UserCount": 5,
            "Users" : [ "userid1" , "userid2" , "userid3" ...],
            "Admins": [ "userid1", "userid2" ...]
            "Name": "企业名称",
            "Addr": "地址",
            "Contacts": "张三"
        }
    ]
}
```

返回说明：

```
{
    "Code" : 0,
    "Msg" : "ok"
}
```

> 前置条件：应用为工业云内部应用，且已注册订阅URI


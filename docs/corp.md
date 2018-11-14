# 企业管理API
[TOC]


## 获取企业基础信息

### 1.1接口描述
https请求方式GET

```
https://api.cloud_industry.qcloud.com/cgi-bin/corps?access_token=ACCESS_TOKEN&corpids=corpid1,corpid2
```

### 1.2传入参数

| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
| access_token | 是 | String | 公共参数 |
| corpids | 是 |Array  |  |


### 1.3返回说明

```

{
   "errcode": 0,
   "errmsg": "ok",
   "corps": [
        {
            "corp_id":"corpid1"
            "name": "企业名称",
            "addr": "地址",
            "contract": {
                "name": "张三",
                "tel": "123123"
            },
           ...
        }
   ]
}
```

## 获取企业下用户userid列表

### 2.1接口描述

* 请求方式：GET（HTTPS）
* 请求地址：

```
https://api.cloud_industry.qcloud.com/cgi-bin/corp/userids?access_token=ACCESS_TOKEN&corp_id=corpid1&offset=0&size=100

```
### 2.2传入参数
| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
| access_token | 是 | String | 公共参数，调用接口凭证 |
| corp_id | 否 |String  | 企业唯一标识 |
| offset|否|long|支持分页查询，与size参数同时设置时才生效，此参数代表偏移量|
| size |否|int|支持分页查询，与offset参数同时设置时才生效，此参数代表分页大小，最大100,排序说明-代表按照用户关联到企业账号时间升序


### 2.3返回说明

```
{
    "errcode": 0,
    "errmsg": "ok",
    "has_more": false,
    "users": [
        {
            "user_id":"zhangpkay",
            "role":"admin"
        }
    ]
}
```

| 参数名称 | 描述 |
| --- | --- |
| 参数| 	说明| 
| errcode	| 返回码
| errmsg| 	对返回码的文本描述内容
| has_more| 	在分页查询时返回，代表是否还有下一页更多数据
| users| 	用户id列表|
| user_id| 	用户唯一标识ID（不可修改）| 
| role	| 用户角色| 

## 获取企业下用户详情列表

### 3.1接口描述

请求方式：GET（HTTPS）
请求地址：
```
https://api.cloud_industry.qcloud.com/cgi-bin/corp/users?access_token=ACCESS_TOKEN&corp_id=corpid1&offset=0&size=100
```
### 3.2传入参数

| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
| access_token | 是 | String | 公共参数，调用接口凭证 |
| corp_id | 否 |String  | 企业唯一标识 |
| offset|否|long|支持分页查询，与size参数同时设置时才生效，此参数代表偏移量|
| size |否|int|支持分页查询，与offset参数同时设置时才生效，此参数代表分页大小，最大100，排序说明-代表按照用户关联到企业账号时间升序


### 3.3返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "has_more": false,
    "userlist":[
        {
            "user_id": "zhangpkay",
            "mobile": "13122222222",
            "tel" : "010-123333",
            "id" : "3625121991112792xx"
            "remark" : "",
            "is_admin": true,
            "name": "张凯",
            "active": true,
            "email": "zhangpkay@tencent-inc.com",
            "avatar":  "./iiotcloud/abc.jpg",
            "extattr": {
                    "爱好":"旅游",
                    "年龄":"24",
                    "hired_date": 1520265600000,
                    "workPlace" :"",
                    "jobnumber": "111111",
                    "position": "工程师",
                    "department": [1, 2],
                      ···
            }
        }
    ]
}
```

* 参数说明

| 参数名称 | 描述 |
| --- | --- |
|errcode|返回码|
|errmsg|对返回码的文本描述内容|
|has_more|在分页查询时返回，代表是否还有下一页更多数据|
|userlist|用户详情列表|
|user_id|用户唯一标识ID（不可修改）|
|id|用户身份证号码|
|is_admin|是否是企业的管理员,true表示是false表示不是|
|name|用户名称|
|remark|用户别名|
|active|表示该用户是否激活了工业云平台账户|
|department|用户所属部门id列表|
|position|职位信息|
|avatar|头像url|
|jobnumber|用户工号|
|hired_date|入职时间|

##获取企业的管理员列表

### 4.1接口描述

是企业内部管理员，则有权限调用。

* 请求方式：GET（HTTPS）
* 请求地址：

```
https://api.cloud_industry.qcloud.com/cgi-bin/corp/admin_ids?access_token=ACCESS_TOKEN&corp_id=corpid1

```
### 4.2传入参数

| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
| access_token | 是 | String | 公共参数，调用接口凭证 |
| corp_id | 否 |String  | 企业唯一标识 |


### 4.3返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "admins":[
        "userid1",
        "userid2",
         ...
    ]
}
```

| 参数名称 | 描述 |
| --- | --- |
|errcode|返回码|
|errmsg|对返回码的文本描述内容|
|admins|管理员id列表|

## 订阅企业信息变更通知

http(s)请求方式POST

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
            "contract": {
                "name": "张三",
                "tel": "123123"
            },
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




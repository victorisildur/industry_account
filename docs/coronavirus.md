[TOC]

# 1 微信授权场景

## 1.1 请求用户微信授权

页面重定向至

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect
```

| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| appid | 是 | 微信公众号appid |
| redirect_uri | 是 | 授权成功后的回跳url,  请使用 urlEncode 对链接进行处理 |
| response_type | 是 | 写死"code" |
| scope | 是 | 写死"snsapi_userinfo" |
| state | 否 | 重定向后会带上state参数，开发者可以填写a-zA-Z0-9的参数值，最多128字节 |

> 由于授权操作安全等级较高，所以在发起授权请求时，微信会对授权链接做正则强匹配校验，如果链接的参数顺序不对，授权页面将无法正常访问

用户同意授权后
如果用户同意授权，页面将跳转至 `redirect_uri/?code=CODE&state=STATE`

## 1.2 查询用户信息

请求方式GET

```
https://{CloudIndustryHost}/iam/api/v1/wx/get_user_info?code={code}
```

| 参数 | 必选 | 含义 |
| -- | ---- | ---- |
| code | 否 | 1.1中获取的微信授权码 |

返回包体

```json
{
    "Code": 0,
    "Msg": "ok",
    "OpenId": "open id",
    "CorpId": "corp id",
    "UserId": "user id",
    "AccessToken": "access token"
}
```

| 参数 | 必选 | 含义 |
| ---- | --- | --- |
| OpenId | 是 | 用户微信open id |
| UserId | 是 | 工业云用户id |
| CorpId | 是 | 工业云企业id |
| AccessToken | 是 | 用户在工业云的access token |

## 1.3 绑定手机和微信

> 对防疫场景，仅需求方（医院侧）需要

请求方式POST

```
https://{CloudIndustryHost}/iam/api/v1/wx/bind_mobile
```

请求包体

```json
{
    "OpenId": "open id",
    "Mobile": "13700000000",
    "Ticket": "123456"
}
```

| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| OpenId | 是 | 微信OpenId |
| Mobile | 是 | 手机号 |
| Ticket | 是 | 6位短信验证码 |

返回包体

```json
{
    "Code": 0,
    "Msg": "",
    "UserId": "user id",
    "CorpId": "corp id",
    "AccessToken": "access token"
}
```

| 参数 | 必选 | 含义 |
| ---- | --- | --- |
| Code | 是 | 0: 成功, 非0: 失败 |
|  UserId | 是 | 手机号对应的工业云user id|
|  CorpId | 是 | 手机号对应的工业云corp id|
|  AccessToken | 是 | 手机号对应的工业云access token|

> 异常情况：该手机号未在工业云注册过，则需前端提示医院侧前往sidacloud.com进行注册、需求发布

# 2. 我要供货场景

## 2.1 更新企业信息

> 供货方调用

请求方式PUT

```
https://{CloudIndustryHost}/iam/api/v1/corp/{corpId}?access_token={access token}
```

请求包体

```json
{
    "Name": "企业名称",
    "Contact": "联系人名称",
    "Tel": "13700000000"
}
```

| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| access_token | 是 | 工业云access token |
| Name | 是 | 企业名称 |
| Contact | 是 | 联系人姓名 |
| Tel | 是  | 联系电话 |

## 2.2 读企业信息

> 供货方调用

请求方式POST

```
http://{CloudIndustryHost}/iam/api/v1/corps?access_token=ACCESS_TOKEN
```



请求JSON包体

```
{
    "CorpIds": ["corp id"]
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
            "Tel": "tel",
            "Contact": "contact",
        }
   ]
}
```

| 参数名称 | 必选  | 描述 |
| --- | --- | --- |
| Name | 否 | 企业名称 |
| Tel | 否 | 联系电话 |
| Contact | 否 | 联系人姓名 |

## 2.3 投标（我要供货)

请求方式POST

```
https://{CloudIndustryHost}/asm/api/do_bid?access_token={access token}
```

请求包体

```json
{
    "DemandId": "xxx", //需求id
    "ServiceCorpId":"123", //投标商企业id
    "ServicePrice":20.5, //服务报价（单位元）
    "ServiceNum": 200000, //数量
    "ServiceDescription": "xxx", //服务描述
    "ServicePhoneNum":"18824909432"//投标商联系方式
}
```

| 参数名 | 是否必填 | 说明 |
| ------ | ------ | ------ |
| DemandId | 是 | 需求id |
| ServiceCorpId | 是 | 投标商企业id |
| ServicePrice | 是 | 服务报价（单位元）|
| ServiceNum | 是 | 数量 |
| ServiceDescription | 是 | 服务描述 |
| ServicePhoneNum | 是 | 投标商联系方式 |

返回包体

```json
{
    "Code": 0,
    "Msg": ""
}
```

## 2.4 读已投标列表

请求方式POST

```
/api/bid_list?access_token={access_token}
```

请求包体

```json
{
    "Filters": [
        {
            "Name": "demand_id",//需求id字段
            "Op": "in", //需求
            "Values": [
                "12341",
                "12342",
                "12344"
            ]
        }
    ],
    "Sorts": [//排序
        {
            "Name": "demand_rating",//排序字段
            "OrderBy": "desc"//默认asc生序，desc降序
        }
    ],
    "PageSize": 10,
    "Offset": 4
}
```

| 参数名 | 是否必填 | 说明 | 类型 |
| ------ | ------ | ------ | --- |
| Filters | 否 | 过滤条件(如果是sp)，可以为空，如果是cp不能为空 | object array |
| Sorts | 否 | 排序条件 | object array |
| PageSize | 否 | 分页查找一次获取的数量 | int |
| Offset | 否 | 分页查找起始位置 | int |

过滤条件Sorts说明

| 参数名 | 是否必填 | 说明 |
| ------ | ------ | ------ |
| Name | 是 | 字段名 |
| OrderBy | 是 | 排序方式支持降序和升序："desc"、"asc" |

过滤条件Filters说明

| 参数名 | 是否必填 | 说明 |
| ------ | ------ | ------ |
| Name | 是 | 字段名 |
| Op | 是 | 比较方式 |
| Values | 是 | 取值列表，字符串数组 |

投标字段名Name说明
	
| 取值 |  说明 |
| ------ |------ |
| created_at | 投标信息创建时间 | 
| bid_id | 创建该需求的企业ID | 
| bid_status | 投标状态 |
| demand_id | 需求id |
| demand_rating | 需求评分 |
| demand_comment | 需求评价信息 |
| service_corp_id | 投标商企业id |
| service_price | 服务报价（单位元）|
| service_period | 服务周期（单位月）|
| service_description | 服务描述 |
| service_doc | 服务相关文档 | 
| service_rating | 服务商评分 |
| service_comment | 服务商评价信息 |

比较方式说明：

| 取值 |  说明 |
| ------ |------ |
| > |  | 
| < |  | 
| >= |  | 
| <= |  | 
| == |  | 
| like | 数据库操作：类似于 | 
| in | 处于Values列表中的值 | 
| neq | 仅用于比较数字，不等于 | 
| != | 仅用于比较数字，不等于 | 


返回包体

```json
{
    "Code": 0,
    "Msg": "成功",
    "Total": 3,
    "Infos": [
        {
            "CreatedAt": "1550720866",
            "BidId": "15507208663542",
            "DemandId": "12341",
            "DemandRating": 5,
            "ServiceCorpId": "133334",
            "ServiceCorpName": "xxx",
            "ServiceCorpLogo": "xxx", // 服务商logo
            "ServicePrice": 23.3,
            "ServicePeriod": 30,
            "ServicePeriodUint": 1,
            "ServiceDescription": "投标了",
            "ServiceDoc": "",
            "Rating": 4,
            "BidStatus": 0,
	        "Guarantee": {
	            "RealNameAuthStatus": 2,
	            "OfflineCertStatus": 0,
	            "EarnestMoneyStatus": 0
	        },
        },
    ]
}
```

| 参数名 | 基础字段 | 说明 |
| ------ | ------ | ------ |
| Code | 是 | 错误码 |
| Msg | 是 | 错误说明 |
| Infos | 否 | 需求列表 |
| Total | 是 | 符合条件的总数量，分页的时候可能用来判断是否有更多 |
| Guarantee | 企业保障认证信息 |  |
|     RealNameAuthStatus | int | 企业实名认证状态 1 审核中 2 审核通过 3 审核拒绝 |
|     OfflineCertStatus | int | 申请线下认证状态 1 审核中 2 审核通过 3 审核拒绝 |
|     EarnestMoneyStatus | int | 申请缴纳保证金状态 1 审核中 2 审核通过 3 审核拒绝 |
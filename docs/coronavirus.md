[TOC]

# 1 微信授权相关

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
|  UserId | 是 | 手机号对应的工业云user id|
|  CorpId | 是 | 手机号对应的工业云corp id|
|  AccessToken | 是 | 手机号对应的工业云access token|

## 1.4 更新企业信息

请求方式PUT

```
https://{CloudIndustryHost}/iam/api/v1/corp/{corpId}
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
| Name | 是 | 企业名称 |
| Contact | 是 | 联系人姓名 |
| Tel | 是  | 联系电话 |

## 1.5 读企业信息

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
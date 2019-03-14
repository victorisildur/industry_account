[TOC]

# 应用场景

平台业务模块在下列场景，需要与账号系统交互：
   
* **平台业务模块对接帐号系统，实现单点登录**
    1. 平台业务模块对接帐号系统，平台模块向账号系统开发接口人提供单点登录所需的`name`, `logo`, `redirect_fqdn`, `subscribe_uri`。账号系统开发接口人输出`login_appid`, `login_appsecret`
    2. 平台业务侧接入单点登录页面，以标准oauth2授权码模式取得令牌(详见[单点登录流程](login.md))，然后使用令牌调用API，获取用户，企业等信息(详见API文档)
* **平台业务模块下的第三方应用，实现单点登录**
    1. 平台业务模块为第三方应用注册`login_appid`(详见[创建单点登录应用API](app.md))
    2. 应用保存loginAppid，接入单点登录页面，以标准oauth2授权码模式取得令牌(详见[单点登录流程](login.md))，然后使用令牌调用API，获取用户，企业等信息(详见API文档)
* **平台业务模块管理第三方应用**
    1. 向第三方应用加入、移出用户(原boss系统ASSIGN，UNASSIGN接口)，详见[开通/关闭用户单点登录应用的权限API](app.md)
    2. 删除第三方应用(原boss系统DELETE接口)，详见[删除单点登录应用API](app.md)


# 应用场景详细说明

## 平台业务模块下的第三方应用，实现单点登录

```sequence
participant 用户
participant 第三方应用
participant 工业云平台

用户->第三方应用: 发起登录
第三方应用->工业云平台: 1. 第三方应用跳转到工业云单点登录态页面
工业云平台-->用户: 2. 用户请求授权： 工业云将用户重定向应用的redirect_uri，附授权码code
用户->第三方应用: 3. 第三方应用在redirect_uri拿到code
第三方应用->工业云平台: 4. 用code申请令牌
工业云平台-->第三方应用: 5. access_token
```

```
说明： 平台业务模块实现单点登录，与第三方应用实现单点登录流程类似。

区别在于平台业务模块的login_appid是内部对接时申请的.

第三方应用的login_appid是企业管理员在boss系统购买第三方应用时，由boss调用平台[创建单点登录应用API]创建的.

第三方应用login_appid生成流程详见下文[企业管理员在boss上购买第三方应用]部分
```

**流程说明**

1. 用户访问第三方应用，后者将前者导向工业云平台单点登录页
2. 用户输入自己在工业云平台注册的用户名密码，点击登录请求授权，工业云平台将用户重定向到login_appid事先指定的重定向地址(redirect_uri)，同时附上授权码。
3. 用户收到重定向返回，重定向到第三方应用的重定向地址
4. 第三方应用在重定向地址收到授权码，附重定向地址，向认证服务器申请令牌。这一步是在第三方应用的后台完成的，对用户不可见。
5. 认证服务器核对了授权码和重定向地址，确认无误后，返回令牌(access_token)等信息

**第1步详解**

用户在第三方应用上发起登录，第三方应用将用户导向工业云平台单点登录页

```
http://129.211.44.155:9092/login?login_appid={login_appid}&redirect_uri={redirect_uri}&scope=all&state={state}
```

| 参数  | 含义 | 是否必填 |
| --- | --- | --- | 
| login_appid | 第三方应用的login_appid | 是 |
| scope | 授权范围，值："all" | 是 |
| redirect_uri | 授权成功重定向地址 | 是 |
| state | 第三方应用填写，工业云平台在回跳redirect_uri时会原样返回，用于防CSRF攻击 | 是 |

**第2步详解**

用户在工业云平台单点登录页面输入在工业云平台注册的用户名、密码，点击登录。

工业云平台验证用户名密码，如通过，返回302到第三方应用事先指定的重定向地址（redirect_uri），同时附上一个授权码。

返回Headers:

```
Status Code: 302
Location: {REDIRECT_URI}?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
```

**第3、4步详解**

用户重定向到第三方应用redirect_uri，第三方应用收到授权码，附上早先的”重定向URI”，向认证服务器申请令牌。

这一步是在app的后台的服务器上完成的，对用户不可见。


请求方式GET

```
http://129.211.44.155:8088/api/v1/oauth/token?grant_type=authorization_code&code={code}&redirect_uri={redirect_uri}&login_appid={login_appid}&login_appsecret={login_appsecret}
```

| 参数 | 含义 | 是否必填 |
| --- | ---- | --- |
| grant_type | 表示使用的授权模式，此处的值固定为”authorization_code" | 是 |
| code | 表示上一步获得的授权码 | 是 |
| redirect_uri | 表示重定向URI，且必须与A步骤中的该参数值保持一致 | 是 |
| login_appid | 表示appID | 是  |
| login_appsecret | 表示app密钥 | 是 |


**第5步详解**

认证服务器核对了授权码和”重定向URI”，确认无误后，向app发送访问令牌（access token）、id_token和更新令牌（refresh token）。

返回JSON包体：

```
{
    "Code" : 0,
    "Msg" : "ok",
    "access_token": "access_token",
    "token_type": "Bearer",
    "refresh_token": "5VO-NEIVVQAMV9KXP_VPNA",
    "id_token": "xxxx.yyyy.zzzz",
    "expires_in": 86400,
    "user_id": "userid"
}
```

| 参数 | 含义 |
| --- | --- |
| access_token | 表示访问令牌，必选项 |
| id_token | id_token，JWT格式 |
| token_type | 表示令牌类型，该值大小写不敏感，必选项，可以是bearer类型或mac类型 |
| expires_in | 表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间 | 
| refresh_token | 表示更新令牌，用来获取下一次的访问令牌，可选项 |
| user_id | 用户id |


## 企业管理员在boss上购买第三方应用

```sequence
participant 用户
participant boss
participant 工业云平台
participant 第三方应用

用户->boss: 购买第三方应用
boss->工业云平台: 申请创建单点登录应用API
工业云平台-->boss: login_appid, login_appsecret \n virtual_admin_userid
boss->第三方应用: boss在原open接口多传 \n login_appid, login_appsecret, admin_userid
第三方应用-->boss: 成功
```

> 前置条件：boss与第三方应用约定，保存第三方应用的`name`, `redirect_fqdn`, `logo`，以供boss调用[申请创建单点登录应用API](app.md)时使用


# 应用场景

平台业务模块在下列场景，需要与账号系统交互：
   
* **平台业务模块对接帐号系统，实现单点登录**
    1. 平台业务模块对接帐号系统，平台模块向账号系统开发接口人提供单点登录所需的`name`, `logo`, `redirect_fqdn`, `subscribe_uri`。账号系统开发接口人输出`login_appid`, `login_appsecret`
    2. 平台业务侧接入单点登录页面，以标准oauth2授权码模式取得令牌(详见[单点登录流程](login.md))，然后使用令牌调用API，获取用户，企业等信息(详见API文档)
* **平台业务模块下的第三方应用，实现单点登录**
    1. 平台业务模块为第三方应用注册`login_appid`(详见[创建单点登录应用API](app.md))
    2. 应用保存loginAppid，接入单点登录页面，以标准oauth2授权码模式取得令牌(详见[单点登录流程](login.md))，然后使用令牌调用API，获取用户，企业等信息(详见API文档)
* **平台业务模块管理第三方应用**
    1. 向第三方应用加入、移出用户(原boss系统ASSIGN，UNASSIGN接口)，详见[开通/关闭用户单点登录应用的权限](app.md)
    2. 删除第三方应用(原boss系统DELETE接口)，详见[删除单点登录应用](app.md)
* **平台业务模块管理企业部门树（待定）**
    - 创建部门
    - 删除部门


# 使用场景流程说明

## 管理员购买app实例开通服务流程图

```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例

用户->boss: 购买某应用app  

boss-> 工业云平台: 购买成功后申请注册全局唯一产品实例
工业云平台->boss: 返回login_appid， secret \n 和虚拟企业管理员userid
boss->app实例:寄云在原open接口新增传入字段 \n login_appid，secret

```

##管理员删除app实例流程

```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:DELETE删除软件服务
app实例->boss: 成功
boss->工业云平台:同步通知工业云平台，传入参数\n type，openid，login_appid
```

##管理员创建部门流程

```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:DEPT_CREATE创建部门
app实例->boss: 成功
boss->工业云平台:同步通知工业云平台，传入参数\n type，openid，login_appid,parameters
```

##管理员删除部门流程

```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:DEPT_REMOVE删除部门
app实例->boss: 成功
boss->工业云平台:同步通知工业云平台，传入参数\n type，openid，login_appid,parameters
```

##管理员创建用户流程

```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:USER_ASSIGN创建用户
app实例->boss: 成功
boss->工业云平台:同步通知工业云平台，传入参数\n type，openid，login_appid,parameters
```

##管理员删除用户流程

```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:USER_UNASSIGN删除部门
app实例->boss: 成功
boss->工业云平台:同步通知工业云平台，传入参数\n type，openid，login_appid,parameters

```

##用户sso登录app实例授权流程
```sequence
participant 用户
participant 工业云平台
participant boss
participant app实例
用户->boss:用户在boss页面点击登录app实例
boss->工业云平台:申请单点登录 \n 传入参数login_appid，secret
工业云平台-->app实例: oauth2.0鉴权授信流程
用户->app实例:用户直接在app实例上发起登录
app实例->工业云平台: 申请单点登录 传入参数 \n工业云平台分给app实例的  login_appid+secret 
工业云平台-->app实例:oauth2.0鉴权授信流程
```


## oauth2 单点登录流程

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


# 单点登录流程详解

1. 用户访问第三方应用，后者将前者导向工业云平台单点登录页
2. 用户输入自己在工业云平台注册的用户名密码，点击登录请求授权，工业云平台将用户重定向到login_appid事先指定的重定向地址(redirect_uri)，同时附上授权码。
3. 用户收到重定向返回，重定向到第三方应用的重定向地址
4. 第三方应用在重定向地址收到授权码，附重定向地址，向认证服务器申请令牌。这一步是在第三方应用的后台完成的，对用户不可见。
5. 认证服务器核对了授权码和重定向地址，确认无误后，返回令牌(access_token)等信息


## 第1步详解

用户在第三方应用上发起登录，第三方应用将用户导向工业云平台单点登录页

```
https://cloud_industry.com/login?login_appid={login_appid}&redirect_uri={redirect_uri}
```


## 第2步详解

用户在工业云平台单点登录页面输入在工业云平台注册的用户名、密码，点击登录


## 第3步详解

工业云平台验证用户名密码，如通过，返回302到第三方应用事先指定的重定向地址（redirection URI），同时附上一个授权码。

返回Headers:

```
HTTP/1.1 302 Found
Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
```

## 第4步详解

用户重定向到第三方应用redirect_uri，第三方应用收到授权码，附上早先的”重定向URI”，向认证服务器申请令牌。

这一步是在app的后台的服务器上完成的，对用户不可见。


请求方式GET：

```
https://cloud_industry.com/oauth/token?grant_type=authorization_code&code={code}&redirect_uri={redirect_uri}&login_appid={login_appid}&login_appsecret={login_appsecret}&state={state}
```

| 参数 | 含义 |
| --- | ---- |
| grant_type | 表示使用的授权模式，必选项，此处的值固定为”authorization_code" |
| code | 表示上一步获得的授权码，必选项 |
| redirect_uri | 表示重定向URI，必选项，且必须与A步骤中的该参数值保持一致 |
| login_appid | 表示appID，必选项 |
| secret | 表示app密钥，必选项 |
| state | 第三方应用随机生成，工业云平台会原样返回，用于防CSRF攻击 |


## 第5步详解

认证服务器核对了授权码和”重定向URI”，确认无误后，向app发送访问令牌（access token）和更新令牌（refresh token）。

返回JSON包体：

```
{
    "code": 0,
    "access_token": "access_token",
    "token_type": "Bearer",
    "refresh_token": "5VO-NEIVVQAMV9KXP_VPNA",
    "expires_in": "2018-11-13T12:21:38.655826+08:00"
    "user_id": "userid"
    "state": "state" // 原样带回，用于防止csrf攻击
}
```

| 参数 | 含义 |
| --- | --- |
| access_token | 表示访问令牌，必选项 |
| token_type | 表示令牌类型，该值大小写不敏感，必选项，可以是bearer类型或mac类型 |
| expires_in | 表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间 | 
| refresh_token | 表示更新令牌，用来获取下一次的访问令牌，可选项 |
| state | 第三方应用随机生成，工业云平台会原样返回，用于防CSRF攻击 |
| user_id | 用户id |

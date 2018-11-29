[TOC]

# 鉴权与统一登录

## 统一单点登录

1. 统一登录页面url: `https://{INDUSTRY_DOMAIN}/login?login_appid={client_appid}&redirect_uri={client_redirect_uri}&scope={scope}&state={state}`

2. 统一登录页判断`client_redirect_uri`是否在应用注册的重定向域名下，如果满足域名约束且用户输入正确的用户名密码后，统一登录页重定向到地址`{client_redirect_uri}?code={code}&state={state}`

3. 应用接入方在`client_redirect_uri`根据uri里的code参数, 自己的client_appid, client_appsecret请求`https://{INDUSTRY_DOMAIN}/oauth/access_token`接口，获取access_token(详见[应用登录流程](login.md))

4. 应用根据access_token，即可调用工业云账号系统的全部API


```
注意：
access_token是企点api的全局唯一接口调用凭据，调用各api时都需使用access_token，开发者需要进行妥善保存。
access_token的存储至少要保留140个字符空间。
access_token的有效期目前为2个小时，需定时刷新。
```

## 统一单点登录前置条件

要接入统一单点登录的应用，首先申请应用client_appid(又称login_appid)和appsecret，并设置授权回调域名(client_redirect_uri)。
平台内应用应前往[应用注册入口](https://qcloud.com)申请注册，账号系统未提供注册页面前，可以在对接阶段直接和开发人员沟通处理。

接入账号系统的第三方应用由负责具体对接的平台内应用（如：工业云BOSS系统）代为申请，申请接口可参考[应用管理API](app.md)。

业务场景:
用户在网页授权页同意授权给应用后，账号系统会将授权数据传给一个应用指定的回调页面（client_redirect_uri），回调页面需在此域名下，以确保安全可靠。

注意事项

1、回调页面域名或路径需使用字母、数字及"-"的组合，不支持ip地址、端口号及短链域名。填写的域名或路径需与实际回调url中的域名或路径相同。需要首先使用该接口设置网页授权成功后跳转的地址域名。

请注意，这里填写的域名不要加"http://" 或者"https://"


2、授权回调域名配置规范为全域名(FQDN)，比如需要网页授权的域名为：
"www.qq.com", 配置以后此域名下面的页面
"http://www.qq.com/music.html" 、 "http://www.qq.com/login.html" 都可以进行授权。
但"http://pay.qq.com" 、 "http://music.qq.com" 、 "http://qq.com"无法进行授权


对于平台内应用，还可设置client_appid相关联的订阅URI(client_subscibe_uri)，用于接收用户信息变更通知、企业信息变更通知、部门信息变更通知（可选）。相关订阅通知接口详见

* [用户管理API](user.md)
* [企业管理API](corp.md)
* [应用管理API](app.md)

## API鉴权

调用API前，必须引导用户完成"统一单点登录"的流程，使应用拿到access_token

所有API都要求传入access_token做为调用凭据。详见其他API文档:

* [用户管理API](user.md)
* [企业管理API](corp.md)
* [应用管理API](app.md)


## API统一错误码

| 错误码 | 错误说明 | 排查方法 |
| ------------ | ------------- | ------------ |
| -1 | 系统繁忙 | 服务器暂不可用，建议稍候重试。建议重试次数不超过3次 |
| 0 | 请求成功 | 接口调用成功 |
| 40001 | 不合法的secret参数 | secret在应用详情/通讯录管理助手可查看 |
| 40002 | redirect_url未登记可信域名 | 查看帮助 |
| 40003 | 不合法的access_token | 查看帮助 |
| 40004 | 不合法的请求字符 | 不能包含\uxxxx格式的字符 |
| 40005 | 不合法的参数 | 查看帮助 |
| 40006 | API接口无权限调用 | 查看帮助 |
| 40007 | API接口已废弃 | 接口已不再支持，建议改用新接口或者新方案 |
| 50001 | 无效的UserID | 查看帮助 | 
| 50002 | 指定的UserID不存在 | 需要确认指定的用户存在于企业通讯录中 |
| 60001 | 不合法的CorpID | 需确认CorpID是否填写正确，在 web管理端-设置 可查看 |
| 60002 | 不合法的UserID列表 | 指定的UserID列表，至少存在一个UserID不在通讯录中|
| 60003 | 不合法的部门列表 | 部门列表为空，或者至少存在一个部门ID不存在于通讯录中 |
| 60004 | 部门长度不符合限制 | 部门名称不能为空且长度不能超过32个字 |
| 60005 | 部门ID不存在 | 需要确认部门ID是否有带，并且存在通讯录中 |
| 60006 | 父部门不存在 | 需要确认父亲部门ID是否有带，并且存在通讯录中 |
| 60007 | 部门下存在成员 | 不允许删除有成员的部门 |
| 60008 | 部门下存在子部门 | 不允许删除有子部门的部门 |
| 60009 | 不允许删除根部门 | - |
| 60010 | 部门已存在 | 部门名称已存在 |
| 60011 | 部门名称含有非法字符 | 不能含有 \:?*“<> 等字符 |
| 60012 | 部门存在循环关系 | 查看帮助 |
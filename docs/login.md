# 应用场景
   

###  平台业务模块对接帐号系统，实现单点登录
 - 平台业务模块对接帐号系统，约定信息
 - 平台业务侧获取用户,企业信息

###  业务模块的应用，实现单点登录

 - 为应用注册loginAppid ，和应用管理员
 - 应用保存loginAppid，
 
###  平台业务模块如何管理应用
  - 管理应用下的用户权限（ ASSIGN，UNASSIGN）
  - 删除应用(DELETE)

###  平台业务如何管理部门
 
 - 创建部门
 - 删除部门
 





  

###  应用注册入口 申请注册loginAppid 
用户在购买sass应用时，boss应为应用先 [申请单点登录loginAppid](app.md) 应用使用获得的 loginAppid（即client_id）进行后续 [单点登录](oauth.md)




##管理员购买app实例开通服务流程图
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例

用户->boss: 购买某应用app  

boss-> iiot平台: 购买成功后申请注册全局唯一产品实例
iiot平台->boss: 返回login_appid， secret \n 和虚拟企业管理员userid
boss->app实例:寄云在原open接口新增传入字段 \n login_appid，secret

```
##管理员删除app实例流程图
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:DELETE删除软件服务
app实例->boss: 成功
boss->iiot平台:同步通知iiot平台，传入参数\n type，openid，login_appid
```
##管理员创建部门流程图
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:DEPT_CREATE创建部门
app实例->boss: 成功
boss->iiot平台:同步通知iiot平台，传入参数\n type，openid，login_appid,parameters
```
##管理员删除部门流程图
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:DEPT_REMOVE删除部门
app实例->boss: 成功
boss->iiot平台:同步通知iiot平台，传入参数\n type，openid，login_appid,parameters
```
##管理员创建用户流程图
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:USER_ASSIGN创建用户
app实例->boss: 成功
boss->iiot平台:同步通知iiot平台，传入参数\n type，openid，login_appid,parameters
```
##管理员删除用户流程图
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例
用户->boss:管理员登录
boss->app实例:USER_UNASSIGN删除部门
app实例->boss: 成功
boss->iiot平台:同步通知iiot平台，传入参数\n type，openid，login_appid,parameters

```
##用户sso登录app实例授权流程
```sequence
participant 用户
participant iiot平台
participant boss
participant app实例
用户->boss:用户在boss页面点击登录app实例
boss->iiot平台:申请单点登录 \n 传入参数login_appid，secret
iiot平台-->app实例: oauth2.0鉴权授信流程
用户->app实例:用户直接在app实例上发起登录
app实例->iiot平台: 申请单点登录 传入参数 \niiot平台分给app实例的  login_appid+secret 
iiot平台-->app实例:oauth2.0鉴权授信流程
```


- ### oauth授权码流程
```sequence
participant 第三方app
participant 第三方后台服务器
participant iiot认证服务器
participant iiot资源服务器
第三方app->iiot认证服务器: 导向认证服务器 
iiot认证服务器->第三方app:用户授权，认证服务器将用户导向客户端事先指定的 \n （redirection URI），同时附上一个授权码code
第三方后台服务器->iiot认证服务器:向认证服务器申请令牌
iiot认证服务器->第三方后台服务器:返回access_token
```


- ###授权码模式详细步骤
    -  1.用户访问app，后者将前者导向认证服务器。
    -  2.用户选择是否给予app授权。如果选”否”，整个授权流程结束。
    -  3.假设用户给予授权，认证服务器将用户导向app事先指定的”重定向URI”（redirection URI），同时附上一个授权码。
    -  4.app收到授权码，附上早先的”重定向URI”，向认证服务器申请令牌。这一步是在客户端的后台的服务器上完成的，对用户不可见。
    -  5.认证服务器核对了授权码和”重定向URI”，确认无误后，返回 访问令牌（access token）和更新令牌（refresh token）。


```
https://graph.qq.com/oauth2.0/show?which=Login&display=pc&response_type=code&client_id=1106062274&redirect_uri=https://xiaowei.qcloud.com/cgi-bin/skill/login_redirect&scope=get_user_info&state=7B226163636F756E7454797065223A312C22735F75726C223A2268747470733A2F2F7869616F7765692E71636C6F75642E636F6D2F227D

```



####第1步详解

用户访问app，后者将前者导向认证服务器。

app申请认证的URI，包含以下参数：

- response_type：表示授权类型，必选项，此处的值固定为”code”
- loginAppid：表示app的ID，必选项
- redirect_uri：表示重定向URI，可选项
- state：表示app的当前状态，可以指定任意值（最好是随机字符串），认证服务器会原封不动地返回这个值，可防止CSRF攻击
下面是一个例子：

```
GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
Host: server.example.com

```
####第2步详解
默认用户授权

####第3步详解

假设用户给予授权，认证服务器将用户导向app事先指定的”重定向URI”（redirection URI），同时附上一个授权码。

服务器回应app的URI，包含以下参数：

- code：表示授权码，必选项。该码的有效期应该很短，通常设为10分钟，app只能使用该码一次， 否则会被授权服务器拒绝。该码与appID和重定向URI，是一一对应关系。
- state：如果app的请求中包含这个参数，认证服务器的回应也必须一模一样包含这个参数。
下面是一个例子：

```
HTTP/1.1 302 Found
Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
```
####第4步详解

app收到授权码，附上早先的”重定向URI”，向认证服务器申请令牌。这一步是在app的后台的服务器上完成的，对用户不可见。

app向认证服务器申请令牌的HTTP请求，包含以下参数：

- grant_type：表示使用的授权模式，必选项，此处的值固定为”authorization_code”。
- code：表示上一步获得的授权码，必选项。
- redirect_uri：表示重定向URI，必选项，且必须与A步骤中的该参数值保持一致。
- loginAppid：表示appID，必选项。
- secret: 表示app密钥，必选项。
下面是一个例子：

```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/json

params = {
  grant_type: "authorization_code",
  code: "SplxlOBeZQQYbYS6WxSbIA",
  loginAppid: "s6BhdRkqt3",
  secret: "xxx",
  redirect_uri: "https://client.example.com/cb"
}
```
####第5步详解

认证服务器核对了授权码和”重定向URI”，确认无误后，向app发送访问令牌（access token）和更新令牌（refresh token）。

认证服务器发送的HTTP回复，包含以下内容：

- access_token：表示访问令牌，必选项。
- token_type：表示令牌类型，该值大小写不敏感，必选项，可以是bearer类型或mac类型。
- expires_in：表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间。
- refresh_token：表示更新令牌，用来获取下一次的访问令牌，可选项。


```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache


{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
  "example_parameter":"example_value"
}
```



# 鉴权与统一登录

## 统一单点登录

1. 统一登录页面url: https://cloud.industry.com/oauth/login?client_id={id}&redirect_uri={client_redirect_uri}&response_type=code

2. 统一登录页判断client_redirect_uri是否在应用注册的重定向域名下，如果满足域名约束且用户输入正确的用户名密码后，统一登录页重定向到地址{client_redirect_uri}?code={code}

3. 应用接入方在{client_redirect_uri}根据uri里的code参数请求https://cloud.industry.com/oauth/access_token接口，获取access_token

4. 应用根据access_token，即可调用工业云账号系统的全部API


> 注意：
>
> access_token是企点api的全局唯一接口调用凭据，调用各api时都需使用access_token，开发者需要进行妥善保存。
>
> access_token的存储至少要保留140个字符空间。
> 
> access_token的有效期目前为2个小时，需定时刷新。

## API鉴权

调用API前，必须引导用户完成"统一单点登录"的流程，使应用拿到access_token。

所有API都要求传入access_token做为调用凭据。详见其他API文档:

* [用户管理API](user.md)
* [企业管理API](corp.md)
* [应用管理API](app.md)
# 鉴权与统一登录

## 鉴权

请求示例（请使用https协议）：
https://api.qidian.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET

请求参数说明:

* grant_type：获取access_token填写client_credential,
* appid：第三方用户唯一凭证,
* secret：第三方用户唯一凭证密钥，即appsecret

返回参数说明:

* access_token：API接口调用凭证
* expires_in：过期时

> 注意：
>
> access_token是企点api的全局唯一接口调用凭据，调用各api时都需使用access_token，开发者需要进行妥善保存。
>
> access_token的存储至少要保留140个字符空间。
> 
> access_token的有效期目前为2个小时，需定时刷新。

## 统一登录接入

统一登录url: https://cloud.industry.com/login?appId={id}

登录后会根据oauth2授权码模式重定向，返回code

App接入方再根据code请求access_token
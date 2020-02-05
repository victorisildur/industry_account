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

## 1.2 用户同意授权后

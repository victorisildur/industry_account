[TOC]

# 快速入门

工业云账号系统整体能力分为3块：

1. 单点登录
2. 用户资料
3. 企业资料

为了让开发者能够快速接入，我们从接入登录，到获取用户资料，到获取企业资料，来看下完整流程

## 1. 接入登录

1. 首先，从工业云开发处获得postman接口调用集
2. 浏览器访问单点登录页[http://129.211.44.155:9092/login?login_appid=1941e3552b37c15d8ba2&redirect_uri={redirect_uri}/identity&response_type=code&scope=all&state=xyz](http://129.211.44.155:9092/login?login_appid=1941e3552b37c15d8ba2&redirect_uri={redirect_uri}&response_type=code&scope=all&state=xyz)，其中{redirect_uri}填入自己的一个地址，用于接收授权码code
3. 浏览器中输入测试账号密码victorisildur@163.com:12345678。或者用手机号自行注册一个用户(推荐，测试账号多人使用会刷新token，使得token失效)，点击登录，浏览器将最终重定向到redirect_uri，并附参数code
4. 复制code，在postman调用示例里找到"code换access_token"示例，将参数中的code和redirect_uri换成2~3步中的值，发送请求，返回access_token, id_token, user_id
5. 真实接入中，code换access_token的过程应由redirect_uri发起，这里postman示例仅做演示

## 2. 用户资料
1. postman调用示例中找到"获取用户详情"示例，将参数中的user_id, access_token换成1.4中得到的值
2. 返回用户详情，用户所在企业在返回body的Roles字段
3. 用户api具体描述详见[https://industry-account2.readthedocs.io/en/latest/user/](https://industry-account2.readthedocs.io/en/latest/user/)

## 3. 企业资料
1. postman调用示例中找到"获取企业详情"示例，将参数中的access_token换成1.4中的值, 包体里的corpIds换成2.2中的值
2. 返回企业详情
3. 企业api具体描述详见[https://industry-account2.readthedocs.io/en/latest/corp/](https://industry-account2.readthedocs.io/en/latest/corp/)
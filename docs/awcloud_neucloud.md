# 寄云、海云、工业云平台账号系统接入

对工业云客户来说，寄云SaaS、PaaS服务, 海云Iaas服务均为工业云服务，其登录状态、用户企业资料、企业微信都应一次开通，到处可用。

为了提供更统一无缝的用户体验，期望寄云、海云在工业云平台统一接入一个账号系统，共享账号信息，减少信息壁垒，都拥有最大化平台能力。

## 技术方案

工业云账号系统同时完成认证(authentication)和授权(authorization)，符合[OpenId Connect](https://connect2id.com/learn/openid-connect)最新协议。

* 认证: 鉴定用户是不是他们宣称的人，由`id token`机制实现
* 授权: 判断应用可以访问哪些资源，由`access token`机制实现

整体方案与google identity platform类同，详细参见[Google OIDC](https://developers.google.com/identity/protocols/OpenIDConnect)

# 接入场景

整体融合后，工业云账号系统做为用户身份提供者，海云、寄云做为内部平台，用户身份消费者。
原先用例下的交互将有所改变，按场景看三方将按照以下流程协同:

## 1 用户注册、登录

用户、企业的注册、认证统一由账号系统提供页面、入口, 账号系统提供后续登录、鉴权、信息拉取服务。

### 1.1 对寄云(海云)来说，用户注册、登录场景为：

```sequence
participant 用户
participant 寄云
participant 工业云平台

用户->工业云平台: 发起注册
工业云平台->工业云平台: 引导用户完成注册、实名认证、企业认证流程
用户->寄云: 登录
寄云-->工业云平台: 将用户导向工业云单点登录页，参数为寄云(海云)login_appid，发起单点登录
用户->工业云平台: 用户输入用户名、密码
工业云平台-->寄云:  重定向到寄云，参数为授权码code，寄云用code换出access_token, id_token
寄云->工业云平台: 用access_token请求用户、用户所属企业信息
工业云平台-->寄云:  返回用户、用户所属企业信息
```

```
id_token为JWT格式，中包含用户id，企业id，签发者（工业云），受众者（海云or寄云），签名（防伪造）。

对海云、寄云来说，直接从工业云API得到的id_token可以信任，直接解出用户、企业信息使用即可。

从其他来源（如海云收到寄云的id_token），工业云提供JWT校验服务校验id token是否可信。
```

对海云来说，用户注册和直接登录的流程同上, 不同的是用户从寄云Boss直接跳转海云控制台的场景:

### 1.2 寄云跳转至海云（方案1）

```sequence
participant 用户
participant 寄云Boss
participant 海云控制台
participant 工业云平台

用户->寄云Boss: 点击"前往云管控制台"
寄云Boss->工业云平台: 请求跳转到海云，参数为海云login_appid、寄云当前id_token
工业云平台->海云控制台: 重定向到海云redirect_uri，参数为授权码code，海云用code换出access_token, id_token
海云控制台->海云控制台: 从id_token中获得用户id，企业id
```

### 1.3 寄云跳转至海云（方案2）

```sequence
participant 用户
participant 寄云Boss
participant 海云控制台
participant 工业云平台

用户->寄云Boss: 点击"前往云管控制台"
寄云Boss->海云控制台: 请求海云access_token，参数为寄云当前id_token
海云控制台->工业云平台: 校验id_token是否合法
工业云平台-->海云控制台: 合法
海云控制台->海云控制台: 从id_token中获取用户id, 企业id, 受众（寄云）
海云控制台-->寄云Boss: 海云access_token
寄云Boss->海云控制台: 跳转海云控制台，参数token={海云access_token}
```

```
方案1、2区别：

1. 因为海云已实现1.1单点登录redirect_uri，所以方案1中海云无任何额外工作。跳转安全性由工业云平台保证。
2. 海云原先账号系统不变，新增根据id_token换海云access_token能力即可
```


## 2 开通、操作海云资源

### 2.1 寄云操作海云资源（方案1）

```sequence
participant 用户
participant 寄云Boss
participant 海云
participant 工业云平台

用户->寄云Boss: 发起开通资源（如购买云主机、硬盘）
寄云Boss->海云: 请求海云创建资源接口，参数为寄云当前id_token
海云->工业云平台: 校验id_token是否合法
工业云平台-->海云: 合法
海云->海云: 从id_token中获取用户id, 受众（寄云），企业id
海云->海云: 若企业(或用户)没有海云租户，则以企业（或用户）为粒度新建租户，并将用户加入租户
海云->海云: 查找对应的租户，进行资源分配
海云-->寄云Boss: 创建状态
寄云Boss->寄云Boss: 更新状态
```

```
海云: 若企业(或用户)没有海云租户，则以企业（或用户）为粒度新建租户，并将用户加入租户

海云根据工业云用户id, 企业id分配租户，有两种方案：

1. 如上图，懒创建，在用户实际调用海云时创建
2. 用户在工业云平台注册时，由工业云平台通知海云用户id, 企业id
```

### 2.2 寄云操作海云资源（方案2）

```sequence
participant 用户
participant 寄云Boss
participant 海云
participant 工业云平台

用户->寄云Boss: 发起操作资源（如购买云主机、查看硬盘）
寄云Boss->海云: 请求海云access_token，参数为寄云当前id_token
海云->工业云平台: 校验id_token是否合法
工业云平台-->海云: 合法
海云->海云: 从id_token中获取用户id, 受众（寄云），企业id
海云-->寄云Boss: 海云access_token
寄云Boss->海云: 操作资源接口，参数为access_token
海云->海云: 查找对应的租户，进行资源操作
海云-->寄云Boss: 操作状态
寄云Boss->寄云Boss: 更新状态
```
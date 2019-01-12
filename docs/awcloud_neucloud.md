# 寄云、海云、工业云平台账号系统接入

对工业云客户来说，寄云SaaS、PaaS服务， 海云Iaas服务均为工业云统一服务，其登录状态、用户企业资料、企业微信打通都理应一次开通，到处可用。
为了提供更统一更无缝的用户体验，期望寄云、海云在工业云平台统一接入一个账号系统，共享账号信息，减少信息壁垒，都拥有最大化平台能力。

# 接入场景

## 1. 用户注册、登录

用户、企业的注册、认证统一由账号系统提供页面、入口, 账号系统提供后续登录、鉴权、信息拉取服务。

对寄云来说，用户注册、登录场景为：

```sequence
participant 用户
participant 寄云
participant 工业云平台

用户->工业云平台: 发起注册
工业云平台->工业云平台: 引导用户完成注册、实名认证、企业认证流程
用户->寄云: 登录
寄云-->工业云平台: 将用户导向工业云单点登录页，参数为寄云login_appid，发起单点登录
用户->工业云平台: 用户输入用户名、密码
工业云平台-->寄云:  重定向到寄云，参数为授权码code，寄云用code换出access_token
寄云->工业云平台: 用access_token请求用户、用户所属企业信息
工业云平台-->寄云:  返回用户、用户所属企业信息
```

对海云来说，用户注册和直接登录的流程同上, 不同的是用户从寄云Boss直接跳转海云控制台的场景:

```sequence
participant 用户
participant 寄云Boss
participant 海云控制台
participant 工业云平台

用户->寄云Boss: 点击"前往云管控制台"
寄云Boss->工业云平台: 请求跳转到海云，参数为海云login_appid、当前access_token
工业云平台->海云控制台: 重定向到海云redirect_uri，参数为授权码code，海云用code换出access_token
海云控制台->工业云平台: 请求用户、用户所属企业信息，参数为access_token
工业云平台-->海云控制台: 用户信息、用户企业信息
海云控制台->海云控制台: 若用户企业没有海云租户，则以企业id为key新建租户，并将用户加入租户
```

## 2、开通海云资源

```sequence
participant 用户
participant 寄云Boss
participant 海云
participant 工业云平台

用户->寄云Boss: 发起开通资源（如购买云主机、硬盘）
寄云Boss->工业云平台: 请求海云access_token，参数为当前access_token、海云login_appid
工业云平台-->寄云Boss: 海云access_token
寄云Boss->海云: 请求海云创建资源接口，参数为海云access_token
海云->工业云平台: 请求用户、企业信息，参数为access_token
工业云平台-->海云: 用户、企业信息
海云->海云: 查找企业对应的租户，进行资源分配
海云-->寄云Boss: 创建状态
寄云Boss->寄云Boss: 更新状态
```

## 3、操作海云资源

```sequence
participant 用户
participant 寄云Boss
participant 海云
participant 工业云平台

用户->寄云Boss: 发起开通资源（如云主机启动、查看云硬盘、项目分配用户）
寄云Boss->工业云平台: 请求海云access_token，参数为当前access_token、海云login_appid
工业云平台-->寄云Boss: 海云access_token
寄云Boss->海云: 请求海云资源操作接口，参数为海云access_token
海云->工业云平台: 请求用户、企业信息，参数为access_token
工业云平台-->海云: 用户、企业信息
海云->海云: 查找企业对应的租户，进行资源操作
海云-->寄云Boss: 操作状态
寄云Boss->寄云Boss: 更新状态
```
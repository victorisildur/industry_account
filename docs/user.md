# 员工管理API
[TOC]
## 员工信息审核
### 接口描述
### 传入参数
### 返回说明

## 添加员工
### 1.1接口描述
创建成员
请求方式：POST（HTTPS）
请求地址：https://qyapi.weixin.qq.com/cgi-bin/user/create?access_token=ACCESS_TOKEN
### 1.2传入参数
```
请求包体：

{
    "userid": "zhangsan",
    "name": "张三",
    "alias": "jackzhang",
    "mobile": "15913215421",
    "department": [1, 2],
    "order":[10,40],
    "position": "产品经理",
    "gender": "1",
    "email": "zhangsan@gzdev.com",
    "isleader": 1,
    "enable":1,
    "avatar_mediaid": "2-G6nrLmr5EC3MNb_-zL1dDdzkd0p7cNliYu9V5w7o8K0",
    "telephone": "020-123456",
    "extattr": {"attrs":[{"name":"爱好","value":"旅游"},{"name":"卡号","value":"1234567234"}]},
    "to_invite": false, 
}
```

* 参数说明：

| 参数 | 必须 | 类型  | 说明 |
| --- | --- | --- | --- |		
|access_token|	是| |	调用接口凭证。获取方法查看“获取access_token”|
|userid	|是	||成员UserID。对应管理端的帐号，企业内必须唯一。不区分大小写，长度为1~64个字节|
|name	|是||	成员名称。长度为1~64个utf8字符|
|alias	|否||	成员别名。长度1~32个utf8字符|
|mobile|	否	||手机号码。企业内必须唯一，mobile/email二者不能同时为空|
|department|	否||部门预留字段,成员所属部门id列表,不超过20个,|
|order	|否||	部门预留字段,部门内的排序值，默认为0，成员次序以创建时间从小到大排列。数量必须和department一致，数值越大排序越前面。有效的值范围是[0, 2^32)|
|position|	否	||职务信息。长度为0~128个字符|
|gender	|否	||性别。1表示男性，2表示女性|
|email	|否	||邮箱。长度6~64个字节，且为有效的email格式。企业内必须唯一，|mobile/email二者不能同时为空|
|telephone|	否	||座机。32字节以内，由纯数字或’-‘号组成。|
|isleader	|否	||上级字段，标识是否为上级。在审批等应用里可以用来标识上级审批人|
|avatar_mediaid	|否	||成员头像的mediaid，通过素材管理接口上传图片获得的mediaid|
|enable|	否||	启用/禁用成员。1表示启用成员，0表示禁用成员|
|extattr|	否| |	自定义字段。自定义字段需要先在WEB管理端添加，见扩展属性添加方法，否则忽略未知属性的赋值。<\br>自定义字段name长度不能多于16个utf8字符，value不能多于32个utf8字符，超过将被截断|
|to_invite|	否	||是否邀请该成员加入工业云平台（将通过微信服务通知或短信或邮件下发邀请，每天自动下发一次，最多持续3个工作日），默认值为true。


权限说明：

应用须拥有指定部门的管理权限。

注意，每个部门下的部门、成员总数不能超过3万个。建议保证创建department对应的部门和创建成员是串行化处理。
### 1.3返回说明

返回结果：

{
   "errcode": 0,
   "errmsg": "created"
}
参数说明：

参数	说明
errcode	返回码
errmsg	对返回码的文本描述内容

## 删除员工
### 2.1接口描述
### 2.2传入参数
### 2.3返回说明
## 修改员工信息
### 3.1接口描述
### 3.2传入参数
### 3.3返回说明
##获取员工信息

### 4.1接口描述
接口请求域名：api.cloud_industry.qcloud.com 。



默认接口请求频率限制：1000次/每秒。

注意事项：

https请求方式POST

```
https://api.cloud_industry.qcloud.com/cgi-bin/org/user/{userid1,userid2...}&access_token={access_token}&appId={appId}
```
### 4.2传入参数


| 参数名称 | 必选 | 类型 | 描述 |
| --- | --- | --- | --- |
|Action  | 是 | String |公共参数，本接口取值：getUserInfo
  |
| sessionid	 | 是 | String | 公共参数 一次请求对应一个SessionId，会原样返回，建议传入类似于uuid的字符串防止重复 |
| access_token | 是 | String | 公共参数 |
| appid |是| String |公共参数 第三方应用实例，唯一标识 |
| userid | 是 |String  | 执行操作的用户身份唯一标识 |
| corpid | 否 |String  | 查询指定企业下该用户的信息 |
| query_users|  是| Array  | 被查询的用户userid列表 |
   
   
   
   
### 4.3返回说明

```
{
    "code" : 0,
    "users": [
        {
            "userId": "userid",
            "name": "用户1", // 姓名
            "gender": 1, // 性别
            "tel": "18902387651", // 电话
            "email": "eux@hotmail.com", // email
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corpId": "corpid1", // 用户所属企业的id
                    "role": "admin" // 用户角色：admin: 企业管理员，user: 企业普通用户
                },
                {
                    "corpId": "corpid2", // 用户所属企业的id
                    "role": "user" // 用户角色：admin: 企业管理员，user: 企业普通用户
                }
            ]
        }, 
    ]
}
```



## 订阅用户信息变更通知

http(s)请求方式POST:

```
CLIENT_SUBSCRIBE_URI
```

请求JSON包体

```
{
    "topic": "userChange",
    "changeList": [
        {
            "changeType": "add", // 新增一个user
            "userId": "userid",
            "name": "name",
            "gender": 1,
            "tel": "tel",
            "email": "email"
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corpId": "corpid",
                    "role": "user"
                }
            ]
        },
        {
            "changeType": "modify", // 修改一个user
            "userId": "userid",
            "name": "name",
            "gender": 1,
            "tel": "tel",
            "email": "email"
            "id": "id", // 身份证号,
            "state": 0, // 0: 未实名认证，1： 已实名认证
            "roles": [
                {
                    "corpId": "corpid",
                    "role": "user"
                }
            ]
        },
        {
            "changeType": "delete", //删除一个user
            "userId": "userid",
        }
    ]
}
```

返回说明

```
{
    "code": 0,
}
```

> 前置要求： 应用为工业云内部应用，且已注册订阅URI


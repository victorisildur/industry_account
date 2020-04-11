# 消息通知模板

接口以API3.0形式提供

```
接口请求方式：API3.0
接口方法: SendMsg
资源id: 1009
```

## 接口请求包格式

```
{
    "Receivers": [
        "411541839540128097"
    ],
    "ReceiverType": 0,
    "Type": 0,
    "Source": 1009,
    "Title": "线下交付产品订单待审核通知",
    "TplId": 9003,
    "Params": null,
    "MsgAction": 5,
    "Extra": "",
    "KvParams": [
        {
            "Key": "UserName",
            "Value": "xliang"
        },
        {
            "Key": "CorpName",
            "Value": "molo555"
        },
        {
            "Key": "CorpAdmin",
            "Value": "xliang"
        },
        {
            "Key": "Product",
            "Value": "ProudThink 普奥"
        },
        {
            "Key": "Version",
            "Value": "v1.0"
        },
        {
            "Key": "Operation",
            "Value": "续费"
        },
        {
            "Key": "OrderId",
            "Value": "470106675538299985"
        },
        {
            "Key": "Contact",
            "Value": "xliang"
        },
        {
            "Key": "Tel",
            "Value": "15967122585"
        },
        {
            "Key": "Email",
            "Value": "lxq520064@126.com"
        }
    ],
    "TemplateParams": null
}
```

| 字段 | 是否必须 | 说明 |
| :--- |:--- |:--- |
| Receivers | 是 | 消息接收者 |
| ReceiverType | 是 | 消息接受者类型，0普通用户；1企业管理员；2所有用户 | 
| Type | 是 | 消息类型，0通知／1待办 |
| Source | 是 | 消息来源，1009: 云服务与精选应用 |
| Title | 是 | 消息标题 |
| TplId | 是 | 消息模板，模板id见下表 |
| Params | 否 | 消息模版参数，若模板没有参数，请设置为空数组 |
| MsgAction | 是 | 消息最终行为(0x01-只发邮件 0x02-只发短信 0x04-只发消息中心 0x07-All) |
| Extra | 否 | 扩展字段，json格式序列化后的字符串 |
| KvParams | 否 | key-value形式的参数 |
| TemplateParams | 否 | 模板参数 |


消息模板：

| 消息模板id | 消息模板内容 |
|:--- | :--- |
| 9003 | 线下交付产品订单待审核通知 |
| 9004 | 线下交付产品订单待支付通知 |
| 9005 | 线下交付产品应用待交付通知 |
| 9006 | 线下交付产品应用已交付通知 |

## 消息模块9003

```
尊敬的{{.UserName}}，您好！

买方企业 {{.CorpName}} 已完成 {{.Product}} 产品 {{.Version}} 版本 {{.Operation}} 订单的创建, 订单号为 {{.OrderId}}

请您在门户-控制台-云服务与精选应用-卖方-订单管理中查看，并尽快完成订单审核。

如有疑问，请联系买方企业，联系人：{{.Contact}}，联系电话:{{.Tel}}，联系邮箱：{{.Email}}。

此致！
{{.Sign}}
```

| 字段 | 说明 |
|:--- |:--- |
| UserName | 邮件接收者姓名 |
| CorpName | 企业名称 |
| Product | 产品名称 |
| Version | 产品版本 |
| Operation | 操作: 新购、续费、版本升级、属性升级 |
| OrderId | 订单ID |
| Contact | 联系人姓名 |
| Tel | 联系电话 |
| Email | 联系人邮箱 |
| Sign | 邮件署名，由平台填入 |

## 消息模板9004

```
尊敬的{{.UserName}}，您好！

卖方企业 {{.CorpName}} 已完成 {{.Product}} 产品 {{.Version}} 版本 {{.Operation}} 订单的审核, 订单号为 {{.OrderId}}

请您在门户-控制台-云服务与精选应用-买方-我的订单中查看，您可支付或取消该订单。

如有疑问，请联系卖方企业，联系人：{{.Contact}}，联系电话:{{.Tel}}，联系邮箱：{{.Email}}。

此致！
{{.Sign}}
```

## 消息模板9005

```
尊敬的{{.UserName}}，您好！

买方企业 {{.CorpName}} 已完成 {{.Product}} 产品 {{.Version}} 版本 {{.Operation}} 订单的支付, 订单号为 {{.OrderId}}

请您在门户-控制台-云服务与精选应用-卖方-应用管理中查看，并尽快完成线下交付。

如有疑问，请联系买方企业，联系人：{{.Contact}}，联系电话:{{.Tel}}，联系邮箱：{{.Email}}。

此致！
{{.Sign}}
```

## 消息模板9006

```
尊敬的{{.UserName}}，您好！

卖方企业 {{.CorpName}} 已完成 {{.Product}} 产品 {{.Version}} 版本 {{.Operation}} 订单的交付, 订单号为 {{.OrderId}}

请您在门户-控制台-云服务与精选应用-买方-我的应用中查看。

如有疑问，请联系卖方企业，联系人：{{.Contact}}，联系电话:{{.Tel}}，联系邮箱：{{.Email}}。

此致！
{{.Sign}}
```
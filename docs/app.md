# APP管理API

## 创建APP

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/app?access_token=access_token

https请求方式POST

POST数据格式：JSON

POST数据例子

```
{
 "name": "app_boss",
 "openId": "520EF0036FDFB9F9D3CED44DEC1CDEAB",
 "corpId": "E38D8X73HX783GF6X3CED44DEC1CDEAB",
}
```

返回说明

```
{
    "code" : 0,
    "appId" : "4152759276",
    "appSecret": "XzDD3h8NPFsIVVdxq"
}
```

## 获取App属性

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/app?appId=appId&access_token=access_token

https请求方式GET

返回说明

```
{
    "code" : 0,
    "appId" : "4152759276",
    "appSecret": "XzDD3h8NPFsIVVdxq"
    "name": "app_boss",
    "appAuthority": [
        {
            "authId": "28938123",
            "name": "购买"
        },
        {
            "authId": "28443122",
            "name": "查看"
        },
        {
            "authId": "28977023",
            "name": "取消购买"
        }
    ]
    "userAuthority": [
        {
            "authId": "28938123",
        },
        {
            "authId": "28443122",
        }
    ],
    "departmentAuthority": [
        {
            "authId": "28977023",
        }
    ],
    "userAttribute": [
        {
            "id": "name",
            "alias": "姓名",
            "type": "string"
        },
        {
            "id": "gender",
            "alias": "性别",
            "type": "number"
        },
        {
            "id": "authority",
            "alias": "权限"
            "type": "string[]"
        },
    ],
    "departmentAttribute": [
        {
            "id": "name",
            "alias": "部门名称",
            "type": "string"
        },
        {
            "id": "leader",
            "alias": "部门leader",
            "type": "string"
        },
        {
            "id": "authority",
            "alias": "权限"
            "type": "string[]"
        },
    ]
}
```
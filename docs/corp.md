# 企业管理API
[TOC]
## 创建企业
### 1.1接口描述
### 1.2传入参数
### 1.3返回说明
## 企业信息审核
### 2.1接口描述
### 2.2传入参数
### 3.3返回说明


## 获取企业信息

https请求方式GET

```
https://api.cloud_industry.qcloud.com/cgi-bin/org/corp/{corpid1,corpid2}&access_token={access_token}
```

返回说明
```
{
   "code" : 0,
   "corps": [
        {
            "name": "企业名称",
            "addr": "地址",
            "contract": {
                "name": "张三",
                "tel": "123123"
            },
            "userCount": 5,
            "users" : [ "userid1" , "userid2" , "userid3" ...],
            "adminCount": 3,
            "admin": [ "userid1", "userid2" ...]
        }
   ]
}
```

## 订阅企业信息变更通知

http(s)请求方式POST

```
CLIENT_SUBSCRIBE_URI
```

请求JSON包体:

```
{
    "topic": "corpChange",
    "changeList": [
        {
            "corpId": "corpid",
            "userCount": 5,
            "users" : [ "userid1" , "userid2" , "userid3" ...],
            "admin": [ "userid1", "userid2" ...]
            "name": "企业名称",
            "addr": "地址",
            "contract": {
                "name": "张三",
                "tel": "123123"
            },
        }
    ]
}
```

返回说明：
{
    "code": 0
}

> 前置条件：应用为工业云内部应用，且已注册订阅URI

## 创建部门

http(s)请求方式POST

```
http(s)://api.industry.com/corp/department?access_token={access_token}
```

请求JSON包体：

```
{
   "name" : "部门名称",
   "corpId": "corpId",
   "parent" : "336", // 父部门deptId, 如创建根部门，不传入该参数
   "leader" : "userid" // 部门领导userId, userId必须在企业内, 一个部门只有一个leader,
   "employees": ["userid1", "userid2" ...]
}
```

返回说明

```
{
    "code": 0,
    "deptId": "336" // 创建出来部门的deptId
}
```

## 获取部门信息

http(s)请求方式GET

```
http(s)://api.industry.com/corp/department/{deptId1,deptid2}?access_token={access_token}
```

返回说明

```
{
    "code": 0,
    "depts": [
        {
            "deptId": "deptid1"
            "name" : "部门名称",
            "corpId": "corpId",
            "parent" : "336", // 父部门deptId, 如创建根部门，不传入该参数
            "leader" : "userid" // 部门领导userId, 一个部门只有一个leader
        }
    ]
}
```

## 获取组织架构列表

http(s)请求方式GET

```
http(s)://api.industry.com/corp/department/{deptId1,deptId2...}/members?access_token={access_token}
```

返回说明

```
{
   "code" : 0,
   "departments" [
       {
           "deptId": "deptid1",
           "employeesTotal" : 7,
           "employees" : [ "userid1" , "userid2" , "userid3" ...]
           "departmentsTotal" : 5,
           "departments" : [ "deptid1", "deptid2", "deptid3" ]
       }, {
           "deptId": "deptid2",
           "employeesTotal" : 7,
           "employees" : [ "userid1" , "userid2" , "userid3" ...]
           "departmentsTotal" : 5,
           "departments" : [ "deptid1", "deptid2", "deptid3" ]
       }
   ]
}
```

> 获取组织架构api，组织架构不是一次拉取所有内容并返回，而是一层一层拉取

## 删除部门
http(s)请求方式DELETE

```
http(s)://api.industry.com/corp/department/{deptId1,deptId2}?access_token={access_token}
```

返回说明
```
{
    "code": 0,
}
```

## 修改部门信息

http(s)请求方式PUT

```
http(s)://api.industry.com/corp/department/{deptId}?access_token={access_token}
```


请求JSON包体：


```
{
   "name" : "部门名称",
   "parent" : "336", // 父部门deptId, 如创建根部门，不传入该参数
   "leader" : "userid" // 部门领导userId, userId必须在企业内, 一个部门只有一个leader
   "employees": ["userid1", "userid2" ...]
}
```

返回说明
```
{
    "code": 0,
}
```


## 订阅部门信息修改

http(s)请求方式POST:

```
CLIENT_SUBSCRIBE_URI
```
请求JSON包体

```
{
    "topic": "deptChange",
    "changeList": [
        {
            "changeType": "add", // 新增一个部门
            "deptId": "253",
            "name" : "部门名称",
            "parent" : "336", // 父部门deptId, 如创建根部门，不传入该参数
            "leader" : "userid" // 部门领导userId, userId必须在企业内, 一个部门只有一个leader
            "employees": ["userid1", "userid2" ...]
        },
        {
            "changeType": "modify", // 修改一个部门
            "deptId": "253",
            "name" : "部门名称",
            "parent" : "336", // 父部门deptId, 如创建根部门，不传入该参数
            "leader" : "userid" // 部门领导userId, userId必须在企业内, 一个部门只有一个leader
            "employees": ["userid1", "userid2" ...]
        },
        {
            "changeType": "modify", // 删除一个部门
            "deptId": "253",
        }
    ]
}
```

返回说明

```
{
    "code": 0
}
```

> 前置条件：应用为工业云内部应用，且已注册订阅URI


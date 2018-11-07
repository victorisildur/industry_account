# 企业管理API

## 获取企业信息

https请求方式GET

https请求方式:
https://api.cloud_industry.qcloud.com/cgi-bin/org/corp?corp={id}&access_token={access_token}

返回说明
```
{
   "code" : 0,
   "userCount": 5,
   "users" : [ "userid1" , "userid2" , "userid3" ...],
   "admin": [ "userid1", "userid2" ...]
}
```

## 订阅企业信息变更通知

http(s)请求方式POST

http(s)请求:
http(s)://{CLIENT_SUBSCRIBE_URL}

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
        }
    ]
}
```

## 创建部门

http(s)请求方式POST

http(s)请求:
http(s)://api.industry.com/corp/department?access_token={access_token}

请求JSON包体：

```
{
   "name" : "部门名称",
   "corpId": "corpId",
   "parent" : 336, // 父部门deptId, 如创建根部门，不传入该参数
   "leader" : "userid" // 部门领导userId, userId必须在企业内, 一个部门只有一个leader,
   "employees": ["userid1", "userid2" ...]
}
```

返回说明

```
{
    "code": 0,
    "deptId": 336 // 创建出来部门的deptId
}
```

## 获取部门信息

http(s)请求方式GET

http(s)请求:
http(s)://api.industry.com/corp/department/{deptId}?access_token={access_token}

返回说明

```
{
   "name" : "部门名称",
   "corpId": "corpId",
   "parent" : 336, // 父部门deptId, 如创建根部门，不传入该参数
   "leader" : "userid" // 部门领导userId, userId必须在企业内, 一个部门只有一个leader
}
```

## 获取组织架构列表

http(s)请求方式GET

http(s)请求:
http(s)://api.industry.com/corp/department/{deptId}/members?access_token={access_token}

返回说明

```
{
   "code" : 0,
   "employeesTotal" : 7,
   "employees" : [ "userid1" , "userid2" , "userid3" ...]
   "departmentsTotal" : 5,
   "departments" : [ partmentid1 , partmentid2 , partmentid3]
}
```

> 获取组织架构api，组织架构不是一次拉取所有内容并返回，而是一层一层拉取

## 删除部门
http(s)请求方式DELETE

http(s)请求:
http(s)://api.industry.com/corp/department/{deptId}?access_token={access_token}

返回说明
```
{
    "code": 0,
}
```

## 修改部门信息

http(s)请求方式PUT

http(s)请求:
http(s)://api.industry.com/corp/department/{deptId}?access_token={access_token}

请求JSON包体：

```
{
   "name" : "部门名称",
   "parent" : 336, // 父部门deptId, 如创建根部门，不传入该参数
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
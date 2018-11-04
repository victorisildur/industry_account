# 部门管理API

## 获取组织架构列表

https请求方式GET

https请求方式: https://api.cloud_industry.qcloud.com/cgi-bin/org/departments?departmentId={id}&access_token=access_token

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

> 备注： 获取组织架构api，组织架构不是一次拉取所有内容并返回，而是一层一层拉取。根部门departmentId===0

## 创建部门 

https请求方式POST

https请求方式: https://api.cloud_industry.qcloud.com/cgi-bin/org/departments?access_token=access_token

POST数据格式：JSON

```
{
   "name" : "engineering",
   "parent" : 336,
   "leader" : "08857FACD73DF75A40075054BEE06C11"
   "authority": ["12", "13"]
}
```


> 备注：父部门ID可通过获取组织架构列表获得。

返回说明

```
{
   "code" : 0,
   "id" : 1
}
```

## 获取部门信息

https请求方式GET

https请求方式: https://api.cloud_industry.qcloud.com/cgi-bin/org/departments?departmentId={id}&access_token=access_token



返回说明

```
{
   "code" : 0,
   "name" : "采购部",
   "parent" : 336,
   "leader" : "08857FACD73DF75A40075054BEE06C11"
   "authority": ["13", "12"]
}
```

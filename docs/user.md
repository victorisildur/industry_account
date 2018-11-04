# 员工管理API

## 创建员工

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/employees?access_token=access_token

https请求方式POST

POST数据格式：JSON

POST数据例子

```
{
 "name": "api_8760",
 "gender": 1,
 "department": ["68", "45"],
 "authority": ["12", "13"]
}
```

返回说明

```
{
    "code" : 0,
    "openId" : "520EF0036FDFB9F9D3CED44DEC1CDEAB",
    "subcode": 0
}
```

## 获取员工信息

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/user?userId={userId}&access_token=access_token

https请求方式GET

返回说明

```
{
    "code" : 0,
    "name": "api_8760",
    "gender": 1,
    "department": ["68", "45"],
    "authority": ["12", "13"]
}
```
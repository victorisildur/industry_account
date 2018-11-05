# 员工管理API

## 获取员工信息

https请求: https://api.cloud_industry.qcloud.com/cgi-bin/org/user?userId={userId}&access_token={access_token}&appId={appId}

https请求方式GET

返回说明

```
{
    "code" : 0,
    "name": "用户1", // 姓名
    "gender": 1, // 性别
    "tel": "18902387651", // 电话
    "email": "eux@hotmail.com", // email
    "corpId": "93eb78a3b7", // 用户所属企业的id
    "role": "admin" // 用户角色：admin: 企业管理员，user: 企业普通用户
}
```
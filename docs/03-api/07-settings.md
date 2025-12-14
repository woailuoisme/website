---
sidebar_position: 7
sidebar_label: 系统配置
title: 系统配置 API
description: 系统配置相关的 API 接口
---

# 系统配置 API

系统配置相关的 API 接口。

## 获取系统配置

**请求**

```http
GET /v1/settings
Authorization: Bearer {your_access_token}
```

**请求示例**

```bash
curl -X GET https://api.example.com/v1/settings \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "site_name": "我的网站",
    "site_description": "网站描述",
    "maintenance_mode": false,
    "allow_registration": true,
    "max_upload_size": 10485760
  },
  "timestamp": 1640995200000
}
```

## 更新系统配置

**请求**

```http
PUT /v1/settings
Authorization: Bearer {your_access_token}
Content-Type: application/json
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| site_name | string | 否 | 网站名称 |
| site_description | string | 否 | 网站描述 |
| maintenance_mode | boolean | 否 | 维护模式 |
| allow_registration | boolean | 否 | 允许注册 |

**请求示例**

```bash
curl -X PUT https://api.example.com/v1/settings \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "site_name": "新网站名称",
    "maintenance_mode": false
  }'
```

:::warning 权限要求
此接口需要管理员权限。
:::

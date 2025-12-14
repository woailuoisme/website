---
sidebar_position: 3
sidebar_label: 用户管理
title: 用户管理 API
description: 用户管理相关的 API 接口
---

# 用户管理 API

用户管理相关的 API 接口，包括用户的创建、查询、更新和删除操作。

## 获取用户列表

**请求**

```http
GET /v1/users
Authorization: Bearer {your_access_token}
```

**查询参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| page | integer | 否 | 页码，默认 1 |
| page_size | integer | 否 | 每页数量，默认 20 |
| keyword | string | 否 | 搜索关键词 |
| status | string | 否 | 用户状态：active, inactive |

**请求示例**

```bash
curl -X GET "https://api.example.com/v1/users?page=1&page_size=20" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "total": 100,
    "page": 1,
    "page_size": 20,
    "items": [
      {
        "id": "user_123",
        "username": "john_doe",
        "email": "john@example.com",
        "status": "active",
        "created_at": "2024-01-01T00:00:00Z",
        "updated_at": "2024-01-01T00:00:00Z"
      }
    ]
  },
  "timestamp": 1640995200000
}
```

## 获取用户详情

**请求**

```http
GET /v1/users/{user_id}
Authorization: Bearer {your_access_token}
```

**路径参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| user_id | string | 是 | 用户 ID |

**请求示例**

```bash
curl -X GET https://api.example.com/v1/users/user_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "user_123",
    "username": "john_doe",
    "email": "john@example.com",
    "phone": "+86 138 0000 0000",
    "avatar": "https://example.com/avatars/user_123.jpg",
    "status": "active",
    "role": "user",
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  },
  "timestamp": 1640995200000
}
```

## 创建用户

**请求**

```http
POST /v1/users
Authorization: Bearer {your_access_token}
Content-Type: application/json
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| username | string | 是 | 用户名，3-20 个字符 |
| email | string | 是 | 邮箱地址 |
| password | string | 是 | 密码，至少 8 个字符 |
| phone | string | 否 | 手机号码 |
| role | string | 否 | 用户角色，默认 user |

**请求示例**

```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "jane_doe",
    "email": "jane@example.com",
    "password": "SecurePass123",
    "phone": "+86 138 0000 0001"
  }'
```

**响应示例**

```json
{
  "code": 201,
  "message": "用户创建成功",
  "data": {
    "id": "user_124",
    "username": "jane_doe",
    "email": "jane@example.com",
    "status": "active",
    "created_at": "2024-01-02T00:00:00Z"
  },
  "timestamp": 1640995200000
}
```

## 更新用户

**请求**

```http
PUT /v1/users/{user_id}
Authorization: Bearer {your_access_token}
Content-Type: application/json
```

**路径参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| user_id | string | 是 | 用户 ID |

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| username | string | 否 | 用户名 |
| email | string | 否 | 邮箱地址 |
| phone | string | 否 | 手机号码 |
| avatar | string | 否 | 头像 URL |

**请求示例**

```bash
curl -X PUT https://api.example.com/v1/users/user_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "+86 138 0000 0002",
    "avatar": "https://example.com/avatars/new_avatar.jpg"
  }'
```

**响应示例**

```json
{
  "code": 200,
  "message": "用户更新成功",
  "data": {
    "id": "user_123",
    "username": "john_doe",
    "email": "john@example.com",
    "phone": "+86 138 0000 0002",
    "avatar": "https://example.com/avatars/new_avatar.jpg",
    "updated_at": "2024-01-02T00:00:00Z"
  },
  "timestamp": 1640995200000
}
```

## 删除用户

**请求**

```http
DELETE /v1/users/{user_id}
Authorization: Bearer {your_access_token}
```

**路径参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| user_id | string | 是 | 用户 ID |

**请求示例**

```bash
curl -X DELETE https://api.example.com/v1/users/user_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "用户删除成功",
  "data": null,
  "timestamp": 1640995200000
}
```

## 错误处理

| 错误码 | 说明 |
|--------|------|
| 400 | 请求参数错误 |
| 401 | 未授权 |
| 403 | 权限不足 |
| 404 | 用户不存在 |
| 409 | 用户名或邮箱已存在 |

## 下一步

- 查看 [文章管理](./04-articles.md) API
- 了解 [评论管理](./05-comments.md) API

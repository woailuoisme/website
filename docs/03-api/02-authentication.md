---
sidebar_position: 2
sidebar_label: 认证授权
title: 认证授权
description: API 认证和授权机制说明
---

# 认证授权

本文档说明如何进行 API 认证和授权。

## 认证方式

### Bearer Token 认证

所有需要认证的 API 请求都必须在请求头中包含 Bearer Token：

```http
Authorization: Bearer {your_access_token}
```

## 获取访问令牌

### 用户登录

**请求**

```http
POST /v1/auth/login
Content-Type: application/json
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| username | string | 是 | 用户名 |
| password | string | 是 | 密码 |

**请求示例**

```bash
curl -X POST https://api.example.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "user@example.com",
    "password": "your_password"
  }'
```

**响应示例**

```json
{
  "code": 200,
  "message": "登录成功",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  "timestamp": 1640995200000
}
```

## 刷新令牌

### 刷新访问令牌

**请求**

```http
POST /v1/auth/refresh
Content-Type: application/json
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| refresh_token | string | 是 | 刷新令牌 |

**请求示例**

```bash
curl -X POST https://api.example.com/v1/auth/refresh \
  -H "Content-Type: application/json" \
  -d '{
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }'
```

**响应示例**

```json
{
  "code": 200,
  "message": "刷新成功",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600
  },
  "timestamp": 1640995200000
}
```

## 退出登录

### 注销令牌

**请求**

```http
POST /v1/auth/logout
Authorization: Bearer {your_access_token}
```

**请求示例**

```bash
curl -X POST https://api.example.com/v1/auth/logout \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "退出成功",
  "data": null,
  "timestamp": 1640995200000
}
```

## 错误处理

### 认证错误

| 错误码 | 说明 | 解决方案 |
|--------|------|----------|
| 401 | 未提供认证信息 | 在请求头中添加 Authorization |
| 401 | 令牌无效或已过期 | 使用刷新令牌获取新的访问令牌 |
| 403 | 权限不足 | 检查用户权限或联系管理员 |

### 错误响应示例

```json
{
  "code": 401,
  "message": "令牌已过期",
  "data": null,
  "timestamp": 1640995200000
}
```

## 安全建议

:::warning 安全提示
- 不要在客户端代码中硬编码访问令牌
- 使用 HTTPS 传输所有 API 请求
- 定期刷新访问令牌
- 妥善保管刷新令牌
- 在用户退出时注销令牌
:::

## 下一步

- 查看 [用户管理](./03-users.md) API
- 了解 [文章管理](./04-articles.md) API

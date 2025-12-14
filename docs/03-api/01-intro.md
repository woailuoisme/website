---
sidebar_position: 1
sidebar_label: API 概览
title: API 概览
description: RESTful API 接口文档概览
---

# RESTful API 文档

欢迎使用我们的 RESTful API 文档。本文档提供了完整的 API 接口说明和使用示例。

## 基础信息

### 认证方式

所有 API 请求都需要在请求头中包含认证信息：

```http
Authorization: Bearer {your_access_token}
```

### 基础 URL

```
https://api.example.com/v1
```

### 响应格式

所有 API 响应都遵循统一的 JSON 格式：

```json
{
  "code": 200,
  "message": "success",
  "data": {},
  "timestamp": 1640995200000
}
```

### 状态码说明

| 状态码 | 说明 |
|--------|------|
| 200 | 请求成功 |
| 201 | 创建成功 |
| 400 | 请求参数错误 |
| 401 | 未授权 |
| 403 | 禁止访问 |
| 404 | 资源不存在 |
| 500 | 服务器内部错误 |

## 快速开始

### 获取访问令牌

```bash
curl -X POST https://api.example.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "your_username", "password": "your_password"}'
```

### 使用访问令牌

```bash
curl -X GET https://api.example.com/v1/users \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## 接口分类

- [认证授权](./02-authentication.md)
- [用户管理](./03-users.md)
- [文章管理](./04-articles.md)
- [评论管理](./05-comments.md)
- [文件上传](./06-upload.md)
- [系统配置](./07-settings.md)

## 版本控制

当前 API 版本：v1

所有 API 请求都需要在 URL 中包含版本号：

```
https://api.example.com/v1/{resource}
```

## 限流策略

- 普通用户：100 请求/小时
- 认证用户：1000 请求/小时
- API 密钥：10000 请求/小时

## 技术支持

如有问题，请联系：

- 邮箱：support@example.com
- 文档更新：https://github.com/example/api-docs

---
sidebar_position: 4
sidebar_label: 文章管理
title: 文章管理 API
description: 文章管理相关的 API 接口
---

# 文章管理 API

文章管理相关的 API 接口，包括文章的创建、查询、更新和删除操作。

## 获取文章列表

**请求**

```http
GET /v1/articles
Authorization: Bearer {your_access_token}
```

**查询参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| page | integer | 否 | 页码，默认 1 |
| page_size | integer | 否 | 每页数量，默认 20 |
| keyword | string | 否 | 搜索关键词 |
| status | string | 否 | 文章状态：draft, published |
| author_id | string | 否 | 作者 ID |

**请求示例**

```bash
curl -X GET "https://api.example.com/v1/articles?page=1&status=published" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "total": 50,
    "page": 1,
    "page_size": 20,
    "items": [
      {
        "id": "article_123",
        "title": "示例文章标题",
        "summary": "文章摘要...",
        "author": {
          "id": "user_123",
          "username": "john_doe"
        },
        "status": "published",
        "views": 1000,
        "created_at": "2024-01-01T00:00:00Z",
        "updated_at": "2024-01-01T00:00:00Z"
      }
    ]
  },
  "timestamp": 1640995200000
}
```

## 获取文章详情

**请求**

```http
GET /v1/articles/{article_id}
Authorization: Bearer {your_access_token}
```

**请求示例**

```bash
curl -X GET https://api.example.com/v1/articles/article_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**响应示例**

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "article_123",
    "title": "示例文章标题",
    "content": "文章完整内容...",
    "summary": "文章摘要...",
    "author": {
      "id": "user_123",
      "username": "john_doe",
      "avatar": "https://example.com/avatars/user_123.jpg"
    },
    "tags": ["技术", "教程"],
    "status": "published",
    "views": 1000,
    "likes": 50,
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  },
  "timestamp": 1640995200000
}
```

## 创建文章

**请求**

```http
POST /v1/articles
Authorization: Bearer {your_access_token}
Content-Type: application/json
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| title | string | 是 | 文章标题 |
| content | string | 是 | 文章内容 |
| summary | string | 否 | 文章摘要 |
| tags | array | 否 | 标签数组 |
| status | string | 否 | 状态，默认 draft |

**请求示例**

```bash
curl -X POST https://api.example.com/v1/articles \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "新文章标题",
    "content": "文章内容...",
    "summary": "文章摘要",
    "tags": ["技术", "教程"],
    "status": "published"
  }'
```

## 更新文章

**请求**

```http
PUT /v1/articles/{article_id}
Authorization: Bearer {your_access_token}
Content-Type: application/json
```

**请求示例**

```bash
curl -X PUT https://api.example.com/v1/articles/article_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "更新后的标题",
    "content": "更新后的内容..."
  }'
```

## 删除文章

**请求**

```http
DELETE /v1/articles/{article_id}
Authorization: Bearer {your_access_token}
```

**请求示例**

```bash
curl -X DELETE https://api.example.com/v1/articles/article_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## 下一步

- 查看 [评论管理](./05-comments.md) API
- 了解 [文件上传](./06-upload.md) API

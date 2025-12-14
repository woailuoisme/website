---
sidebar_position: 5
sidebar_label: 评论管理
title: 评论管理 API
description: 评论管理相关的 API 接口
---

# 评论管理 API

评论管理相关的 API 接口。

## 获取评论列表

**请求**

```http
GET /v1/comments
Authorization: Bearer {your_access_token}
```

**查询参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| article_id | string | 是 | 文章 ID |
| page | integer | 否 | 页码，默认 1 |
| page_size | integer | 否 | 每页数量，默认 20 |

**请求示例**

```bash
curl -X GET "https://api.example.com/v1/comments?article_id=article_123" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## 创建评论

**请求**

```http
POST /v1/comments
Authorization: Bearer {your_access_token}
Content-Type: application/json
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| article_id | string | 是 | 文章 ID |
| content | string | 是 | 评论内容 |
| parent_id | string | 否 | 父评论 ID（回复评论时使用） |

**请求示例**

```bash
curl -X POST https://api.example.com/v1/comments \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "article_id": "article_123",
    "content": "这是一条评论"
  }'
```

## 删除评论

**请求**

```http
DELETE /v1/comments/{comment_id}
Authorization: Bearer {your_access_token}
```

**请求示例**

```bash
curl -X DELETE https://api.example.com/v1/comments/comment_123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

---
sidebar_position: 6
sidebar_label: 文件上传
title: 文件上传 API
description: 文件上传相关的 API 接口
---

# 文件上传 API

文件上传相关的 API 接口。

## 上传文件

**请求**

```http
POST /v1/upload
Authorization: Bearer {your_access_token}
Content-Type: multipart/form-data
```

**请求参数**

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| file | file | 是 | 要上传的文件 |
| type | string | 否 | 文件类型：image, document, video |

**请求示例**

```bash
curl -X POST https://api.example.com/v1/upload \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -F "file=@/path/to/file.jpg" \
  -F "type=image"
```

**响应示例**

```json
{
  "code": 200,
  "message": "上传成功",
  "data": {
    "file_id": "file_123",
    "url": "https://cdn.example.com/files/file_123.jpg",
    "filename": "file.jpg",
    "size": 102400,
    "mime_type": "image/jpeg",
    "created_at": "2024-01-01T00:00:00Z"
  },
  "timestamp": 1640995200000
}
```

## 文件限制

- 单个文件最大 10MB
- 支持的图片格式：JPG, PNG, GIF, WebP
- 支持的文档格式：PDF, DOC, DOCX, XLS, XLSX
- 支持的视频格式：MP4, AVI, MOV

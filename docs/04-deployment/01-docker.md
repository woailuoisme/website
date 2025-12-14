---
sidebar_position: 1
sidebar_label: Docker 部署
title: Docker 部署
description: 使用 Docker 部署应用
---

# Docker 部署

本指南介绍如何使用 Docker 部署应用。

## 前置要求

- Docker 20.10 或更高版本
- Docker Compose 2.0 或更高版本

## 使用 Dockerfile

### 创建 Dockerfile

```dockerfile
FROM node:18-alpine

WORKDIR /app

# 复制 package 文件
COPY package.json pnpm-lock.yaml ./

# 安装 pnpm
RUN npm install -g pnpm

# 安装依赖
RUN pnpm install --frozen-lockfile

# 复制源代码
COPY . .

# 构建应用
RUN pnpm build

# 暴露端口
EXPOSE 3000

# 启动应用
CMD ["pnpm", "serve", "--host", "0.0.0.0", "--port", "3000"]
```

### 构建镜像

```bash
docker build -t my-docs:latest .
```

### 运行容器

```bash
docker run -d \
  --name my-docs \
  -p 3000:3000 \
  my-docs:latest
```

## 使用 Docker Compose

### 创建 docker-compose.yml

```yaml
version: '3.8'

services:
  docs:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    restart: unless-stopped
    volumes:
      - ./build:/app/build
```

### 启动服务

```bash
# 构建并启动
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

## 多阶段构建优化

使用多阶段构建减小镜像大小：

```dockerfile
# 构建阶段
FROM node:18-alpine AS builder

WORKDIR /app

COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm
RUN pnpm install --frozen-lockfile

COPY . .
RUN pnpm build

# 生产阶段
FROM nginx:alpine

# 复制构建产物
COPY --from=builder /app/build /usr/share/nginx/html

# 复制 nginx 配置
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Nginx 配置

创建 `nginx.conf`：

```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # 启用 gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    # 缓存静态资源
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

## 环境变量

在 Docker 中使用环境变量：

```bash
docker run -d \
  --name my-docs \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -e API_URL=https://api.example.com \
  my-docs:latest
```

## 健康检查

添加健康检查到 Dockerfile：

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000 || exit 1
```

## 日志管理

查看容器日志：

```bash
# 查看实时日志
docker logs -f my-docs

# 查看最近 100 行日志
docker logs --tail 100 my-docs
```

## 故障排查

### 容器无法启动

```bash
# 检查容器状态
docker ps -a

# 查看容器日志
docker logs my-docs

# 进入容器调试
docker exec -it my-docs sh
```

### 端口冲突

```bash
# 检查端口占用
docker ps | grep 3000

# 使用不同端口
docker run -d -p 8080:3000 my-docs:latest
```

## 下一步

- 查看 [Kubernetes 部署](./02-kubernetes.md)
- 了解 [云平台部署](./03-cloud-platforms.md)

---
sidebar_position: 3
sidebar_label: 云平台部署
title: 云平台部署
description: 在主流云平台上部署应用
---

# 云平台部署

本指南介绍如何在主流云平台上部署应用。

## Vercel 部署

Vercel 是部署静态网站和 Docusaurus 应用的最佳选择之一。

### 通过 Git 集成部署

1. 将代码推送到 GitHub/GitLab/Bitbucket
2. 访问 [Vercel](https://vercel.com)
3. 点击 "Import Project"
4. 选择你的仓库
5. Vercel 会自动检测 Docusaurus 并配置构建设置

### 使用 Vercel CLI

```bash
# 安装 Vercel CLI
pnpm add -g vercel

# 登录
vercel login

# 部署
vercel

# 部署到生产环境
vercel --prod
```

### vercel.json 配置

```json
{
  "buildCommand": "pnpm build",
  "outputDirectory": "build",
  "framework": "docusaurus",
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

## Netlify 部署

### 通过 Git 集成部署

1. 访问 [Netlify](https://netlify.com)
2. 点击 "New site from Git"
3. 选择你的仓库
4. 配置构建设置：
   - Build command: `pnpm build`
   - Publish directory: `build`

### netlify.toml 配置

```toml
[build]
  command = "pnpm build"
  publish = "build"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[build.environment]
  NODE_VERSION = "18"
```

### 使用 Netlify CLI

```bash
# 安装 Netlify CLI
pnpm add -g netlify-cli

# 登录
netlify login

# 部署
netlify deploy

# 部署到生产环境
netlify deploy --prod
```

## GitHub Pages 部署

### 配置 docusaurus.config.js

```javascript
module.exports = {
  url: 'https://username.github.io',
  baseUrl: '/repository-name/',
  organizationName: 'username',
  projectName: 'repository-name',
  deploymentBranch: 'gh-pages',
  trailingSlash: false,
};
```

### 使用 GitHub Actions

创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18
      
      - name: Install pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 8
      
      - name: Install dependencies
        run: pnpm install --frozen-lockfile
      
      - name: Build
        run: pnpm build
      
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
```

### 手动部署

```bash
# 设置 Git 用户信息
GIT_USER=<Your GitHub username> pnpm deploy
```

## AWS S3 + CloudFront

### 部署到 S3

```bash
# 构建应用
pnpm build

# 安装 AWS CLI
# 配置 AWS 凭证
aws configure

# 上传到 S3
aws s3 sync build/ s3://your-bucket-name --delete

# 设置 bucket 为静态网站
aws s3 website s3://your-bucket-name \
  --index-document index.html \
  --error-document index.html
```

### CloudFront 配置

1. 创建 CloudFront 分发
2. 源域名：选择 S3 bucket
3. 默认根对象：`index.html`
4. 错误页面：配置 404 重定向到 `index.html`

### 自动化部署脚本

创建 `deploy-aws.sh`：

```bash
#!/bin/bash

# 构建
pnpm build

# 上传到 S3
aws s3 sync build/ s3://your-bucket-name --delete

# 清除 CloudFront 缓存
aws cloudfront create-invalidation \
  --distribution-id YOUR_DISTRIBUTION_ID \
  --paths "/*"

echo "部署完成！"
```

## 阿里云 OSS

### 部署到 OSS

```bash
# 安装 ossutil
# 配置 OSS
ossutil config

# 上传文件
ossutil cp -r build/ oss://your-bucket-name/ --update

# 设置静态网站
ossutil website oss://your-bucket-name/ index.html index.html
```

## 腾讯云 COS

### 部署到 COS

```bash
# 安装 COSCMD
pip install coscmd

# 配置
coscmd config -a <SecretId> -s <SecretKey> \
  -b <BucketName> -r <Region>

# 上传文件
coscmd upload -r build/ / --delete

# 设置静态网站
coscmd putbucketwebsite index.html index.html
```

## 环境变量配置

### 在构建时使用环境变量

```javascript
// docusaurus.config.js
module.exports = {
  customFields: {
    apiUrl: process.env.API_URL || 'https://api.example.com',
  },
};
```

### 在组件中使用

```javascript
import useDocusaurusContext from '@docusaurus/useDocusaurusContext';

function MyComponent() {
  const {siteConfig} = useDocusaurusContext();
  const apiUrl = siteConfig.customFields.apiUrl;
  
  return <div>API URL: {apiUrl}</div>;
}
```

## 性能优化

### CDN 配置

- 启用 CDN 加速
- 配置缓存策略
- 启用 Gzip/Brotli 压缩

### 构建优化

```javascript
// docusaurus.config.js
module.exports = {
  future: {
    experimental_faster: true,
  },
  webpack: {
    jsLoader: (isServer) => ({
      loader: require.resolve('esbuild-loader'),
      options: {
        loader: 'tsx',
        target: isServer ? 'node12' : 'es2017',
      },
    }),
  },
};
```

## 监控和分析

### Google Analytics

```javascript
// docusaurus.config.js
module.exports = {
  themeConfig: {
    gtag: {
      trackingID: 'G-XXXXXXXXXX',
    },
  },
};
```

### 自定义分析

```javascript
// docusaurus.config.js
module.exports = {
  scripts: [
    {
      src: 'https://analytics.example.com/script.js',
      async: true,
    },
  ],
};
```

## 下一步

- 查看 [Docker 部署](./01-docker.md)
- 了解 [Kubernetes 部署](./02-kubernetes.md)

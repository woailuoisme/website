---
sidebar_position: 3
sidebar_label: 最佳实践
title: 最佳实践
description: 文档编写和维护的最佳实践
---

# 最佳实践

遵循这些最佳实践，可以帮助你创建高质量、易维护的文档。

## 文档组织

### 目录结构

保持清晰的目录结构：

```
docs/
├── 01-getting-started/
├── 02-user-guide/
├── 03-api/
├── 04-deployment/
├── 05-development/
└── 06-troubleshooting/
```

### 命名规范

- 使用有意义的文件名
- 使用小写字母和连字符
- 添加数字前缀控制顺序

```
01-introduction.md
02-installation.md
03-quick-start.md
```

## 内容编写

### 标题层级

保持合理的标题层级：

```markdown
# 一级标题（页面标题）
## 二级标题（主要章节）
### 三级标题（子章节）
#### 四级标题（详细内容）
```

### 代码示例

提供完整、可运行的代码示例：

```javascript
// ✅ 好的示例 - 完整且有注释
const config = {
  title: '我的网站',
  url: 'https://example.com',
};

export default config;
```

```javascript
// ❌ 不好的示例 - 不完整
const config = {
  ...
};
```

### 使用告示框

适当使用告示框突出重要信息：

:::tip 提示
使用 `pnpm` 可以获得更快的安装速度和更好的磁盘空间利用率。
:::

:::warning 注意
在生产环境中，请确保使用 `pnpm build` 而不是 `pnpm start`。
:::

## 维护性

### 保持更新

- 定期检查和更新文档
- 及时反映产品变化
- 修复过时的信息

### 版本管理

使用版本控制管理文档：

```bash
# 创建新版本
pnpm docusaurus docs:version 1.0.0
```

### 链接检查

定期检查文档中的链接：

```bash
# 构建时检查链接
pnpm build
```

## 可访问性

### 图片替代文本

为所有图片提供有意义的替代文本：

```markdown
![Docusaurus 架构图 - 展示了核心组件和它们之间的关系](/img/architecture.png)
```

### 语义化标记

使用语义化的 Markdown 标记：

```markdown
# 使用标题而不是加粗文本
## 正确的章节标题

**重要内容** - 使用加粗强调
*斜体文本* - 使用斜体表示术语
```

## SEO 优化

### 前置元数据

为每个页面添加完整的元数据：

```yaml
---
title: 页面标题
description: 简洁明了的页面描述，不超过 160 个字符
keywords: [关键词1, 关键词2, 关键词3]
---
```

### URL 结构

使用清晰、有意义的 URL：

```
✅ /docs/getting-started/installation
❌ /docs/doc1
```

## 性能优化

### 图片优化

- 使用适当的图片格式（WebP、PNG、JPEG）
- 压缩图片大小
- 使用响应式图片

### 代码分割

利用 Docusaurus 的自动代码分割功能，按路由加载内容。

## 协作

### 贡献指南

为贡献者提供清晰的指南：

- 如何设置开发环境
- 代码风格要求
- 提交流程

### 审查流程

建立文档审查流程：

1. 创建分支
2. 编写/修改文档
3. 提交 Pull Request
4. 团队审查
5. 合并到主分支

## 下一步

- 查看 [API 文档](../03-api/01-intro.md) 了解接口详情
- 阅读 [开发指南](../05-development/01-architecture.md) 了解架构设计

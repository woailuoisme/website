---
sidebar_position: 1
sidebar_label: 基础概念
title: 基础概念
description: 了解产品的核心概念和术语
---

# 基础概念

在开始使用之前，了解以下核心概念将帮助你更好地理解和使用我们的产品。

## 文档系统

文档系统是基于 Docusaurus 构建的静态网站生成器，它将 Markdown 文件转换为优化的静态 HTML 页面。

### 主要组件

- **文档 (Docs)** - 结构化的技术文档
- **博客 (Blog)** - 时间线式的文章内容
- **页面 (Pages)** - 自定义的独立页面

## Markdown 和 MDX

### Markdown

Markdown 是一种轻量级标记语言，使用简单的语法来格式化文本。

```markdown
# 这是一级标题
## 这是二级标题

这是一段普通文本。

- 列表项 1
- 列表项 2
```

### MDX

MDX 是 Markdown 的扩展，允许你在 Markdown 中使用 JSX 和 React 组件。

```mdx
import CustomComponent from '@site/src/components/CustomComponent';

# 标题

<CustomComponent />
```

## 前置元数据 (Frontmatter)

每个文档文件可以包含 YAML 格式的前置元数据，用于配置页面属性：

```yaml
---
sidebar_position: 1
sidebar_label: 自定义标签
title: 页面标题
description: 页面描述
---
```

## 侧边栏

侧边栏是文档的导航菜单，可以通过以下方式配置：

- **自动生成** - 基于文件结构自动生成
- **手动配置** - 在 `sidebars.js` 中手动定义

## 国际化 (i18n)

支持多语言文档，通过 `i18n` 目录管理不同语言的翻译内容。

## 下一步

继续阅读 [功能说明](./02-features.md) 了解产品的具体功能。

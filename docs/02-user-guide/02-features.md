---
sidebar_position: 2
sidebar_label: 功能说明
title: 功能说明
description: 详细的功能介绍和使用示例
---

# 功能说明

本文档详细介绍产品的各项功能及其使用方法。

## 文档编写

### 创建新文档

在 `docs/` 目录下创建新的 Markdown 文件：

```bash
docs/
└── my-category/
    └── my-document.md
```

### 文档结构

每个文档应包含清晰的结构：

```markdown
---
sidebar_position: 1
title: 文档标题
---

# 主标题

## 章节 1

内容...

## 章节 2

内容...
```

## 代码高亮

支持多种编程语言的语法高亮：

```javascript
function hello() {
  console.log('Hello, World!');
}
```

```python
def hello():
    print("Hello, World!")
```

```bash
echo "Hello, World!"
```

## 告示框

使用告示框突出显示重要信息：

:::note
这是一个普通提示。
:::

:::tip
这是一个有用的提示。
:::

:::info
这是一条信息。
:::

:::warning
这是一个警告。
:::

:::danger
这是一个危险警告。
:::

## 标签页

使用标签页组织相关内容：

```mdx
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="js" label="JavaScript">
    JavaScript 代码示例
  </TabItem>
  <TabItem value="py" label="Python">
    Python 代码示例
  </TabItem>
</Tabs>
```

## 图片和资源

### 添加图片

```markdown
![图片描述](/img/example.png)
```

### 静态资源

将静态资源放在 `static/` 目录中，它们会被复制到构建输出的根目录。

## 链接

### 内部链接

```markdown
[链接文本](./other-document.md)
[链接到特定章节](./other-document.md#section)
```

### 外部链接

```markdown
[外部链接](https://example.com)
```

## 表格

| 功能 | 描述 | 状态 |
|------|------|------|
| 文档 | Markdown 文档 | ✅ |
| 博客 | 博客文章 | ✅ |
| 搜索 | 全文搜索 | ✅ |
| i18n | 多语言支持 | ✅ |

## 下一步

查看 [最佳实践](./03-best-practices.md) 了解如何更好地使用产品。

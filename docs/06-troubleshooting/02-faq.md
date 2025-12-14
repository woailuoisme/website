---
sidebar_position: 2
sidebar_label: 常见问答
title: 常见问答 (FAQ)
description: 常见问题快速解答
---

# 常见问答 (FAQ)

快速解答常见问题。

## 一般问题

### Docusaurus 是什么？

Docusaurus 是由 Facebook (Meta) 开发的静态网站生成器，专门用于构建文档网站。它基于 React，提供了开箱即用的文档功能。

### 为什么选择 Docusaurus？

- ⚡ **快速** - 基于 React 的单页应用，加载速度快
- 📝 **易用** - 使用 Markdown 编写文档
- 🎨 **美观** - 现代化的默认主题
- 🔍 **搜索** - 内置搜索功能
- 🌍 **国际化** - 完善的多语言支持
- 📱 **响应式** - 完美支持移动设备

### Docusaurus 适合什么项目？

- 技术文档网站
- API 参考文档
- 产品文档
- 开源项目文档
- 知识库
- 博客

## 安装和设置

### 需要什么前置条件？

- Node.js 18.0 或更高版本
- 包管理器：pnpm、npm 或 yarn
- 基本的命令行知识
- 基本的 Markdown 知识

### 如何选择包管理器？

推荐使用 pnpm：

- **pnpm** - 最快，节省磁盘空间（推荐）
- **npm** - Node.js 自带，兼容性好
- **yarn** - 速度快，功能丰富

### 可以使用 TypeScript 吗？

可以！Docusaurus 完全支持 TypeScript：

```bash
# 安装类型定义
pnpm add -D @docusaurus/module-type-aliases @docusaurus/types
```

## 文档编写

### Markdown 和 MDX 有什么区别？

- **Markdown** - 纯文本标记语言，语法简单
- **MDX** - Markdown + JSX，可以在 Markdown 中使用 React 组件

### 如何添加图片？

```markdown
# 方法 1：使用静态资源
![图片描述](/img/example.png)

# 方法 2：使用相对路径
![图片描述](./assets/example.png)

# 方法 3：使用外部链接
![图片描述](https://example.com/image.png)
```

### 如何创建内部链接？

```markdown
# 相对路径（推荐）
[链接文本](./other-doc.md)
[链接到章节](./other-doc.md#section)

# 绝对路径
[链接文本](/docs/category/doc)
```

### 如何添加代码块？

````markdown
```javascript
function hello() {
  console.log('Hello, World!');
}
```

```python
def hello():
    print("Hello, World!")
```
````

### 如何使用告示框？

```markdown
:::note
这是一个普通提示。
:::

:::tip
这是一个有用的提示。
:::

:::warning
这是一个警告。
:::

:::danger
这是一个危险警告。
:::
```

## 配置

### 如何修改网站标题和 Logo？

编辑 `docusaurus.config.js`：

```javascript
module.exports = {
  title: '我的网站',
  tagline: '网站标语',
  favicon: 'img/favicon.ico',
  
  themeConfig: {
    navbar: {
      title: '我的网站',
      logo: {
        alt: 'Logo',
        src: 'img/logo.svg',
      },
    },
  },
};
```

### 如何自定义主题颜色？

编辑 `src/css/custom.css`：

```css
:root {
  --ifm-color-primary: #2e8555;
  --ifm-color-primary-dark: #29784c;
  --ifm-color-primary-darker: #277148;
  --ifm-color-primary-darkest: #205d3b;
  --ifm-color-primary-light: #33925d;
  --ifm-color-primary-lighter: #359962;
  --ifm-color-primary-lightest: #3cad6e;
}
```

### 如何添加 Google Analytics？

```javascript
// docusaurus.config.js
module.exports = {
  themeConfig: {
    gtag: {
      trackingID: 'G-XXXXXXXXXX',
      anonymizeIP: true,
    },
  },
};
```

## 国际化

### 如何添加多语言支持？

1. 配置 i18n：

```javascript
// docusaurus.config.js
module.exports = {
  i18n: {
    defaultLocale: 'zh-Hans',
    locales: ['zh-Hans', 'en'],
  },
};
```

2. 创建翻译文件：

```bash
pnpm write-translations --locale en
```

3. 翻译内容到 `i18n/en/` 目录

### 如何切换默认语言？

修改 `defaultLocale`：

```javascript
module.exports = {
  i18n: {
    defaultLocale: 'en', // 改为英文
    locales: ['en', 'zh-Hans'],
  },
};
```

## 部署

### 支持哪些部署平台？

- Vercel（推荐）
- Netlify
- GitHub Pages
- AWS S3 + CloudFront
- 阿里云 OSS
- 腾讯云 COS
- 任何支持静态网站的平台

### 如何部署到 Vercel？

1. 推送代码到 GitHub
2. 访问 [Vercel](https://vercel.com)
3. 导入项目
4. 自动部署完成

### 如何部署到 GitHub Pages？

```bash
# 配置 docusaurus.config.js
module.exports = {
  url: 'https://username.github.io',
  baseUrl: '/repository-name/',
  organizationName: 'username',
  projectName: 'repository-name',
};

# 部署
GIT_USER=username pnpm deploy
```

### 部署后如何更新？

推送代码到主分支，CI/CD 会自动部署更新。

## 性能

### 如何优化构建速度？

1. 启用实验性快速模式：

```javascript
module.exports = {
  future: {
    experimental_faster: true,
  },
};
```

2. 减少不必要的插件
3. 优化图片大小

### 如何优化页面加载速度？

- 压缩图片
- 使用 WebP 格式
- 启用 CDN
- 使用代码分割（默认启用）

## 搜索

### 如何添加搜索功能？

使用本地搜索插件：

```bash
pnpm add @easyops-cn/docusaurus-search-local
```

```javascript
// docusaurus.config.js
module.exports = {
  themes: [
    [
      require.resolve('@easyops-cn/docusaurus-search-local'),
      {
        hashed: true,
        language: ['zh', 'en'],
      },
    ],
  ],
};
```

### Algolia DocSearch 和本地搜索有什么区别？

- **Algolia DocSearch**
  - 云端搜索服务
  - 需要申请（免费）
  - 搜索速度快
  - 需要网络连接

- **本地搜索**
  - 无需申请
  - 离线可用
  - 搜索索引包含在构建中
  - 增加构建大小

## 自定义

### 如何添加自定义页面？

在 `src/pages/` 目录创建文件：

```jsx
// src/pages/custom.js
import Layout from '@theme/Layout';

export default function CustomPage() {
  return (
    <Layout title="自定义页面">
      <div>
        <h1>这是自定义页面</h1>
      </div>
    </Layout>
  );
}
```

访问：`http://localhost:3000/custom`

### 如何添加自定义组件？

1. 创建组件：

```jsx
// src/components/CustomButton.js
export default function CustomButton({children}) {
  return <button className="custom-button">{children}</button>;
}
```

2. 在 MDX 中使用：

```mdx
import CustomButton from '@site/src/components/CustomButton';

<CustomButton>点击我</CustomButton>
```

### 如何修改主题组件？

使用 swizzle 命令：

```bash
# 查看可修改的组件
pnpm swizzle @docusaurus/theme-classic --list

# 修改组件
pnpm swizzle @docusaurus/theme-classic Footer --eject
```

## 版本管理

### 如何创建文档版本？

```bash
pnpm docusaurus docs:version 1.0.0
```

这会创建 `versioned_docs/version-1.0.0/` 目录。

### 如何管理多个版本？

编辑 `versions.json`：

```json
[
  "2.0.0",
  "1.0.0"
]
```

配置版本下拉菜单：

```javascript
module.exports = {
  themeConfig: {
    navbar: {
      items: [
        {
          type: 'docsVersionDropdown',
          position: 'right',
        },
      ],
    },
  },
};
```

## 故障排查

### 构建失败怎么办？

1. 检查错误信息
2. 清除缓存：`pnpm clear`
3. 删除 node_modules 重新安装
4. 查看 [常见问题](./01-common-issues.md)

### 热重载不工作怎么办？

1. 重启开发服务器
2. 清除缓存
3. 检查文件监听限制（Linux）

### 在哪里获取帮助？

- [官方文档](https://docusaurus.io)
- [GitHub Issues](https://github.com/facebook/docusaurus/issues)
- [Discord 社区](https://discord.gg/docusaurus)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/docusaurus)

## 下一步

- 查看 [常见问题](./01-common-issues.md)
- 阅读 [用户指南](../02-user-guide/01-basic-concepts.md)
- 浏览 [API 文档](../03-api/01-intro.md)

---
sidebar_position: 1
sidebar_label: 常见问题
title: 常见问题
description: 常见问题和解决方案
---

# 常见问题

本文档列出了使用过程中的常见问题及其解决方案。

## 安装问题

### 问题：pnpm install 失败

**错误信息：**
```
ERR_PNPM_FETCH_404  GET https://registry.npmjs.org/@docusaurus/core: Not Found - 404
```

**原因：**
- 网络连接问题
- npm registry 配置错误
- 包版本不存在

**解决方案：**

1. 检查网络连接
2. 切换 npm registry：

```bash
# 使用淘宝镜像
pnpm config set registry https://registry.npmmirror.com

# 或使用官方镜像
pnpm config set registry https://registry.npmjs.org
```

3. 清除缓存并重新安装：

```bash
pnpm store prune
rm -rf node_modules
pnpm install
```

### 问题：Node.js 版本不兼容

**错误信息：**
```
error docusaurus@3.8.1: The engine "node" is incompatible with this module.
```

**解决方案：**

升级 Node.js 到 18.0 或更高版本：

```bash
# 使用 nvm
nvm install 18
nvm use 18

# 验证版本
node --version
```

## 构建问题

### 问题：构建失败 - 内存不足

**错误信息：**
```
FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory
```

**解决方案：**

增加 Node.js 内存限制：

```bash
# 临时增加内存
NODE_OPTIONS="--max-old-space-size=4096" pnpm build

# 或在 package.json 中配置
{
  "scripts": {
    "build": "NODE_OPTIONS='--max-old-space-size=4096' docusaurus build"
  }
}
```

### 问题：构建失败 - 链接错误

**错误信息：**
```
Error: Docs markdown link couldn't be resolved: (./non-existent.md)
```

**解决方案：**

1. 检查链接路径是否正确
2. 确保目标文件存在
3. 使用相对路径：

```markdown
# ✅ 正确
[链接](./existing-file.md)

# ❌ 错误
[链接](./non-existent.md)
```

### 问题：构建失败 - 前置元数据错误

**错误信息：**
```
Error: Error while parsing front matter
```

**解决方案：**

检查 YAML 前置元数据格式：

```yaml
---
# ✅ 正确
sidebar_position: 1
title: 标题
description: 描述
---

# ❌ 错误 - 缺少结束标记
---
sidebar_position: 1
title: 标题

# ❌ 错误 - YAML 语法错误
---
sidebar_position: 1
title: 标题: 副标题
---
```

## 开发服务器问题

### 问题：端口被占用

**错误信息：**
```
Error: listen EADDRINUSE: address already in use :::3000
```

**解决方案：**

1. 使用不同端口：

```bash
pnpm start -- --port 3001
```

2. 或终止占用端口的进程：

```bash
# macOS/Linux
lsof -ti:3000 | xargs kill -9

# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### 问题：热重载不工作

**症状：**
修改文件后浏览器不自动刷新

**解决方案：**

1. 检查文件监听限制（Linux）：

```bash
# 增加文件监听限制
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

2. 重启开发服务器：

```bash
# 停止服务器 (Ctrl+C)
# 清除缓存
pnpm clear
# 重新启动
pnpm start
```

## 搜索问题

### 问题：搜索功能不工作

**症状：**
搜索框无法输入或没有结果

**解决方案：**

1. 确保搜索插件已安装：

```bash
pnpm add @easyops-cn/docusaurus-search-local
```

2. 检查配置：

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

3. 重新构建搜索索引：

```bash
pnpm clear
pnpm build
```

## 国际化问题

### 问题：中文翻译不显示

**症状：**
切换到中文后仍显示英文内容

**解决方案：**

1. 检查 i18n 配置：

```javascript
// docusaurus.config.js
module.exports = {
  i18n: {
    defaultLocale: 'zh-Hans',
    locales: ['zh-Hans', 'en'],
  },
};
```

2. 确保翻译文件存在：

```
i18n/
└── zh-Hans/
    ├── docusaurus-plugin-content-docs/
    │   └── current/
    └── docusaurus-theme-classic/
        ├── navbar.json
        └── footer.json
```

3. 重新构建：

```bash
pnpm build
```

## 部署问题

### 问题：GitHub Pages 404 错误

**症状：**
部署后访问页面显示 404

**解决方案：**

1. 检查 baseUrl 配置：

```javascript
// docusaurus.config.js
module.exports = {
  url: 'https://username.github.io',
  baseUrl: '/repository-name/', // 注意斜杠
};
```

2. 确保 gh-pages 分支存在：

```bash
git branch -a | grep gh-pages
```

3. 检查 GitHub Pages 设置：
   - 访问仓库 Settings → Pages
   - 确保 Source 设置为 gh-pages 分支

### 问题：静态资源 404

**症状：**
CSS/JS 文件无法加载

**解决方案：**

检查 baseUrl 和资源路径：

```javascript
// ✅ 正确 - 使用绝对路径
<img src="/img/logo.png" />

// ❌ 错误 - 使用相对路径
<img src="img/logo.png" />
```

## 性能问题

### 问题：构建速度慢

**解决方案：**

1. 启用并行构建：

```javascript
// docusaurus.config.js
module.exports = {
  future: {
    experimental_faster: true,
  },
};
```

2. 减少文档数量（开发时）：

```bash
# 只构建特定文档
pnpm start -- --locale zh-Hans
```

3. 使用增量构建：

```bash
# 开发模式自动启用增量构建
pnpm start
```

### 问题：页面加载慢

**解决方案：**

1. 优化图片：
   - 压缩图片大小
   - 使用 WebP 格式
   - 使用适当的图片尺寸

2. 启用代码分割（默认启用）

3. 使用 CDN 加速静态资源

## 样式问题

### 问题：自定义样式不生效

**解决方案：**

1. 确保 CSS 文件被导入：

```javascript
// docusaurus.config.js
module.exports = {
  presets: [
    [
      'classic',
      {
        theme: {
          customCss: require.resolve('./src/css/custom.css'),
        },
      },
    ],
  ],
};
```

2. 检查 CSS 选择器优先级：

```css
/* 使用更具体的选择器 */
.navbar__item {
  color: red !important;
}
```

3. 清除缓存：

```bash
pnpm clear
pnpm start
```

## 获取帮助

如果以上解决方案都无法解决你的问题：

1. 查看 [FAQ](./02-faq.md)
2. 搜索 [GitHub Issues](https://github.com/facebook/docusaurus/issues)
3. 在 [GitHub Discussions](https://github.com/facebook/docusaurus/discussions) 提问
4. 加入 [Discord 社区](https://discord.gg/docusaurus)

## 下一步

- 查看 [FAQ](./02-faq.md)
- 阅读 [开发指南](../05-development/01-architecture.md)

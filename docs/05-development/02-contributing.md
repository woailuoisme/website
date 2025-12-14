---
sidebar_position: 2
sidebar_label: 贡献指南
title: 贡献指南
description: 如何为项目做出贡献
---

# 贡献指南

感谢你考虑为项目做出贡献！本指南将帮助你了解如何参与项目开发。

## 行为准则

### 我们的承诺

为了营造一个开放和友好的环境，我们承诺：

- 尊重不同的观点和经验
- 优雅地接受建设性批评
- 关注对社区最有利的事情
- 对其他社区成员表示同理心

### 不可接受的行为

- 使用性化的语言或图像
- 人身攻击或侮辱性评论
- 公开或私下骚扰
- 未经许可发布他人的私人信息

## 开始贡献

### 设置开发环境

1. **Fork 仓库**

访问项目仓库并点击 "Fork" 按钮。

2. **克隆仓库**

```bash
git clone https://github.com/your-username/repository-name.git
cd repository-name
```

3. **安装依赖**

```bash
pnpm install
```

4. **启动开发服务器**

```bash
pnpm start
```

5. **创建分支**

```bash
git checkout -b feature/your-feature-name
```

## 贡献类型

### 文档改进

文档是项目的重要组成部分。你可以：

- 修复拼写或语法错误
- 改进现有文档的清晰度
- 添加缺失的文档
- 翻译文档到其他语言

**文档位置：**
- 主文档：`docs/` 目录
- 博客文章：`blog/` 目录
- 中文翻译：`i18n/zh-Hans/` 目录

### 代码贡献

#### 修复 Bug

1. 在 Issues 中查找标记为 "bug" 的问题
2. 在评论中表明你想要处理这个问题
3. 创建分支并修复问题
4. 提交 Pull Request

#### 添加新功能

1. 先创建一个 Issue 讨论新功能
2. 等待维护者的反馈
3. 获得批准后开始开发
4. 提交 Pull Request

### 报告 Bug

提交 Bug 报告时，请包含：

- **清晰的标题** - 简洁描述问题
- **重现步骤** - 详细的重现步骤
- **预期行为** - 你期望发生什么
- **实际行为** - 实际发生了什么
- **环境信息** - 操作系统、浏览器、Node.js 版本等
- **截图** - 如果适用，添加截图

**Bug 报告模板：**

```markdown
## 问题描述
简要描述问题

## 重现步骤
1. 访问 '...'
2. 点击 '...'
3. 滚动到 '...'
4. 看到错误

## 预期行为
描述你期望发生什么

## 实际行为
描述实际发生了什么

## 环境
- OS: [例如 macOS 13.0]
- Browser: [例如 Chrome 120]
- Node.js: [例如 18.0.0]

## 截图
如果适用，添加截图
```

### 功能请求

提交功能请求时，请包含：

- **功能描述** - 清晰描述新功能
- **使用场景** - 为什么需要这个功能
- **建议实现** - 如果有想法，描述如何实现
- **替代方案** - 考虑过的其他方案

## 代码规范

### JavaScript/React

遵循 ESLint 配置：

```javascript
// ✅ 好的示例
function MyComponent({title, description}) {
  return (
    <div className="my-component">
      <h1>{title}</h1>
      <p>{description}</p>
    </div>
  );
}

// ❌ 不好的示例
function mycomponent(props) {
  return <div><h1>{props.title}</h1><p>{props.description}</p></div>
}
```

### Markdown

- 使用 ATX 风格的标题（`#`）
- 代码块指定语言
- 链接使用相对路径
- 保持行长度在 80-100 字符

```markdown
# 一级标题

## 二级标题

代码示例：

\`\`\`javascript
const example = 'code';
\`\`\`

[链接文本](./relative-path.md)
```

### 提交信息

遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

```bash
# 格式
<type>(<scope>): <subject>

# 类型
feat: 新功能
fix: Bug 修复
docs: 文档更新
style: 代码格式（不影响代码运行）
refactor: 重构
test: 测试相关
chore: 构建过程或辅助工具的变动

# 示例
feat(api): 添加用户认证接口
fix(docs): 修复安装指南中的错误链接
docs: 更新贡献指南
```

## Pull Request 流程

### 1. 准备工作

```bash
# 确保代码是最新的
git checkout main
git pull upstream main

# 创建功能分支
git checkout -b feature/your-feature
```

### 2. 开发和测试

```bash
# 进行更改
# ...

# 运行测试
pnpm test

# 构建检查
pnpm build

# 代码检查
pnpm lint
```

### 3. 提交更改

```bash
# 添加更改
git add .

# 提交（遵循提交信息规范）
git commit -m "feat: 添加新功能"

# 推送到你的 fork
git push origin feature/your-feature
```

### 4. 创建 Pull Request

1. 访问你的 fork 仓库
2. 点击 "New Pull Request"
3. 填写 PR 描述：
   - 更改内容
   - 相关 Issue
   - 测试说明
   - 截图（如适用）

**PR 模板：**

```markdown
## 更改内容
描述你的更改

## 相关 Issue
Closes #123

## 更改类型
- [ ] Bug 修复
- [ ] 新功能
- [ ] 文档更新
- [ ] 代码重构

## 测试
描述如何测试你的更改

## 截图
如果适用，添加截图

## 检查清单
- [ ] 代码遵循项目规范
- [ ] 已添加必要的测试
- [ ] 所有测试通过
- [ ] 文档已更新
- [ ] 提交信息遵循规范
```

### 5. 代码审查

- 响应审查意见
- 进行必要的修改
- 推送更新

```bash
# 修改后推送
git add .
git commit -m "fix: 根据审查意见修改"
git push origin feature/your-feature
```

### 6. 合并

维护者会在审查通过后合并你的 PR。

## 开发工作流

### 分支策略

- `main` - 主分支，始终保持可部署状态
- `feature/*` - 功能分支
- `fix/*` - Bug 修复分支
- `docs/*` - 文档更新分支

### 版本发布

项目使用语义化版本：

- `MAJOR.MINOR.PATCH`
- `1.0.0` → `1.0.1` (补丁)
- `1.0.0` → `1.1.0` (次要版本)
- `1.0.0` → `2.0.0` (主要版本)

## 社区

### 获取帮助

- **GitHub Issues** - 报告问题和功能请求
- **GitHub Discussions** - 一般讨论和问答
- **Discord** - 实时聊天和社区交流

### 保持联系

- 关注项目更新
- 参与讨论
- 帮助其他贡献者

## 致谢

感谢所有为项目做出贡献的人！

你的贡献将被记录在：
- GitHub 贡献者列表
- 项目 CHANGELOG
- 发布说明

## 下一步

- 查看 [架构设计](./01-architecture.md)
- 了解 [测试指南](./03-testing.md)
- 浏览 [开放的 Issues](https://github.com/your-org/your-repo/issues)

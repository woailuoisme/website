---
sidebar_position: 3
sidebar_label: 测试指南
title: 测试指南
description: 如何运行和编写测试
---

# 测试指南

本指南介绍项目的测试策略和如何编写测试。

## 测试类型

### 1. 构建测试

验证项目能够成功构建：

```bash
# 运行构建
pnpm build

# 检查构建输出
ls -la build/
```

### 2. 链接检查

检查文档中的链接是否有效：

```bash
# 构建时会自动检查内部链接
pnpm build

# 使用 broken-link-checker 检查外部链接
npx broken-link-checker http://localhost:3000 -ro
```

### 3. 格式检查

检查代码格式和规范：

```bash
# 运行 ESLint
pnpm lint

# 自动修复
pnpm lint:fix

# 检查 Markdown 格式
pnpm format:check

# 自动格式化
pnpm format
```

## 测试环境设置

### 安装测试依赖

```bash
pnpm add -D jest @testing-library/react @testing-library/jest-dom
```

### Jest 配置

创建 `jest.config.js`：

```javascript
module.exports = {
  testEnvironment: 'jsdom',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.test.js'],
  moduleNameMapper: {
    '^@site/(.*)$': '<rootDir>/$1',
    '^@theme/(.*)$': '<rootDir>/node_modules/@docusaurus/theme-classic/src/theme/$1',
  },
  transform: {
    '^.+\\.(js|jsx|ts|tsx)$': ['babel-jest', {presets: ['@docusaurus/core/lib/babel/preset']}],
  },
};
```

## 编写测试

### 组件测试

测试 React 组件：

```javascript
// src/components/__tests__/CustomButton.test.js
import React from 'react';
import {render, screen, fireEvent} from '@testing-library/react';
import CustomButton from '../CustomButton';

describe('CustomButton', () => {
  test('renders button with text', () => {
    render(<CustomButton>Click me</CustomButton>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  test('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<CustomButton onClick={handleClick}>Click me</CustomButton>);
    
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  test('applies custom className', () => {
    render(<CustomButton className="custom">Click me</CustomButton>);
    expect(screen.getByText('Click me')).toHaveClass('custom');
  });
});
```

### 配置测试

测试配置文件：

```javascript
// __tests__/config.test.js
const config = require('../docusaurus.config');

describe('Docusaurus Config', () => {
  test('has required fields', () => {
    expect(config.title).toBeDefined();
    expect(config.url).toBeDefined();
    expect(config.baseUrl).toBeDefined();
  });

  test('has correct i18n config', () => {
    expect(config.i18n.defaultLocale).toBe('zh-Hans');
    expect(config.i18n.locales).toContain('zh-Hans');
    expect(config.i18n.locales).toContain('en');
  });

  test('has navbar items', () => {
    expect(config.themeConfig.navbar.items).toBeDefined();
    expect(config.themeConfig.navbar.items.length).toBeGreaterThan(0);
  });
});
```

### 侧边栏测试

测试侧边栏配置：

```javascript
// __tests__/sidebars.test.js
const sidebars = require('../sidebars');

describe('Sidebars Config', () => {
  test('has docsSidebar', () => {
    expect(sidebars.docsSidebar).toBeDefined();
  });

  test('docsSidebar is an array', () => {
    expect(Array.isArray(sidebars.docsSidebar)).toBe(true);
  });

  test('has required categories', () => {
    const categories = sidebars.docsSidebar.map(item => item.label);
    expect(categories).toContain('快速开始');
    expect(categories).toContain('用户指南');
    expect(categories).toContain('API 文档');
  });
});
```

## 文档测试

### Markdown 语法检查

使用 markdownlint 检查 Markdown 语法：

```bash
# 安装
pnpm add -D markdownlint-cli

# 检查
pnpm markdownlint "docs/**/*.md"

# 配置 .markdownlint.json
{
  "default": true,
  "MD013": false,
  "MD033": false
}
```

### 前置元数据验证

验证文档的前置元数据：

```javascript
// scripts/validate-frontmatter.js
const fs = require('fs');
const path = require('path');
const matter = require('gray-matter');

function validateFrontmatter(filePath) {
  const content = fs.readFileSync(filePath, 'utf8');
  const {data} = matter(content);

  // 检查必需字段
  if (!data.sidebar_position) {
    console.error(`Missing sidebar_position in ${filePath}`);
    return false;
  }

  if (!data.title) {
    console.error(`Missing title in ${filePath}`);
    return false;
  }

  return true;
}

// 遍历所有文档
const docsDir = path.join(__dirname, '../docs');
// ... 实现遍历逻辑
```

## 持续集成

### GitHub Actions

创建 `.github/workflows/test.yml`：

```yaml
name: Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
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

      - name: Run linter
        run: pnpm lint

      - name: Run tests
        run: pnpm test

      - name: Build
        run: pnpm build

      - name: Check links
        run: |
          pnpm serve &
          sleep 5
          npx broken-link-checker http://localhost:3000 -ro
```

## 性能测试

### Lighthouse CI

配置 Lighthouse CI 进行性能测试：

```bash
# 安装
pnpm add -D @lhci/cli

# 配置 lighthouserc.js
module.exports = {
  ci: {
    collect: {
      startServerCommand: 'pnpm serve',
      url: ['http://localhost:3000'],
    },
    assert: {
      assertions: {
        'categories:performance': ['error', {minScore: 0.9}],
        'categories:accessibility': ['error', {minScore: 0.9}],
        'categories:best-practices': ['error', {minScore: 0.9}],
        'categories:seo': ['error', {minScore: 0.9}],
      },
    },
  },
};

# 运行
pnpm lhci autorun
```

## 测试最佳实践

### 1. 测试命名

使用描述性的测试名称：

```javascript
// ✅ 好的
test('renders error message when API call fails', () => {});

// ❌ 不好的
test('test1', () => {});
```

### 2. 测试隔离

每个测试应该独立：

```javascript
// ✅ 好的
describe('UserList', () => {
  beforeEach(() => {
    // 每个测试前重置状态
    jest.clearAllMocks();
  });

  test('test 1', () => {});
  test('test 2', () => {});
});
```

### 3. 测试覆盖率

追求有意义的测试覆盖率：

```bash
# 运行测试并生成覆盖率报告
pnpm test --coverage

# 查看覆盖率报告
open coverage/lcov-report/index.html
```

### 4. 快照测试

谨慎使用快照测试：

```javascript
test('renders correctly', () => {
  const {container} = render(<MyComponent />);
  expect(container.firstChild).toMatchSnapshot();
});
```

## 调试测试

### 使用 console.log

```javascript
test('debug test', () => {
  const result = someFunction();
  console.log('Result:', result);
  expect(result).toBe(expected);
});
```

### 使用 debug

```javascript
import {render, screen} from '@testing-library/react';

test('debug test', () => {
  const {debug} = render(<MyComponent />);
  debug(); // 打印当前 DOM
});
```

### VS Code 调试

配置 `.vscode/launch.json`：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Jest Debug",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["--runInBand", "--no-cache"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    }
  ]
}
```

## 常见问题

### 测试超时

增加超时时间：

```javascript
test('long running test', async () => {
  // ...
}, 10000); // 10 秒超时
```

### 模拟模块

```javascript
jest.mock('@docusaurus/useDocusaurusContext', () => ({
  __esModule: true,
  default: () => ({
    siteConfig: {
      title: 'Test Site',
    },
  }),
}));
```

## 下一步

- 查看 [架构设计](./01-architecture.md)
- 了解 [贡献指南](./02-contributing.md)
- 开始编写测试！

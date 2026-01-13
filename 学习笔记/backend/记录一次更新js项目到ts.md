好的！我帮你把你之前提问的问题和我给的回复，以及最终总结的知识库内容，系统地整理归纳成一个结构清晰、完整的项目升级与工具配置文档。内容涵盖你的问题、我给的详细解答和总结，方便你后续查阅和复用。

---

# 前端项目从 JS 升级到 TS 及 ESLint/Prettier/Husky 配置全流程知识库

---

## 目录

1. 项目背景
2. TypeScript 环境搭建及配置
3. ESLint + Prettier 集成配置
4. Husky + lint-staged 提交前自动格式化与检查
5. Vite 配置注意点及常见问题
6. 常见问题及解决方案汇总
7. 总结

---

## 1. 项目背景

你在 `feature` 分支将项目从纯 JavaScript 升级到 TypeScript，同时引入了代码质量与风格检查工具 ESLint 和 Prettier，并使用 Husky + lint-staged 实现提交前自动格式化与校验。升级完成后合并回 `dev` 分支。

---

## 2. TypeScript 环境搭建及配置

### 安装依赖

```
pnpm add -D typescript @types/node
```

### 初始化与配置 `tsconfig.json`

初始执行：

```
pnpm exec tsc --init
```

修改为适合 Vite + Vue 的配置：

```
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "nodenext",
    "moduleResolution": "nodenext",
    "strict": true,
    "jsx": "preserve",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
    "types": ["vite/client"]
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue", "vite.config.ts"]
}
```

**说明**：`module` 和 `moduleResolution` 设为 `nodenext` 是解决 TS 无法正确解析 `.ts` 文件及路径别名的关键。

### 修改文件后缀和语法

- `.js` 改成 `.ts`
- Vue 文件 `<script lang="ts">`
- 路径导入用别名（@）

---

## 3. ESLint + Prettier 集成配置

### 安装相关依赖

```
pnpm add -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-vue eslint-config-prettier eslint-plugin-prettier prettier
```

### 配置示例 `.eslintrc.js`

```
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
    es2021: true,
  },
  parser: "vue-eslint-parser",
  parserOptions: {
    parser: "@typescript-eslint/parser",
    ecmaVersion: 12,
    sourceType: "module",
  },
  extends: [
    "plugin:vue/vue3-essential",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  plugins: ["@typescript-eslint", "vue", "prettier"],
  rules: {
    "prettier/prettier": "error",
  }
}
```

### Prettier 配置文件 `.prettierrc`

```
{
  "singleQuote": true,
  "semi": false,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

### 解决格式化冲突

- 使用 `eslint-config-prettier` 关闭 ESLint 中与 Prettier 冲突的规则。
- 保证 `.eslintrc.js` 中有 `"plugin:prettier/recommended"` 或手动添加 prettier 相关规则。

---

## 4. Husky + lint-staged 实现提交前格式化

### 安装并初始化 Husky 和 lint-staged

```
pnpm add -D husky lint-staged

pnpm dlx husky-init && pnpm install
```

- 会自动生成 `.husky/pre-commit` 和 `prepare` 脚本。

### 配置 `package.json` 中 lint-staged

```
"lint-staged": {
  "*.{ts,tsx,vue}": [
    "eslint --fix",
    "prettier --write",
    "git add"
  ]
}
```

### `.husky/pre-commit` 内容示例

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

pnpm exec lint-staged
```

### 常见问题

- 报错 `Command "lint-staged" not found`，确认 `lint-staged` 是否安装，且 `.husky/pre-commit` 中执行命令是否正确。
- 执行 `pnpm dlx husky-init` 重置 husky。

---

## 5. Vite 配置注意点

### 示例 `vite.config.ts`

```
import { defineConfig } from "vite"
import vue from "@vitejs/plugin-vue"
import tailwindcss from '@tailwindcss/vite'

const host = process.env.TAURI_DEV_HOST

export default defineConfig({
  plugins: [
    vue({
      template: {
        compilerOptions: {
          isCustomElement: (tag) => tag.startsWith('md-'),
        },
      },
    }),
    tailwindcss(),
  ],
  resolve: {
    alias: {
      "@": "/src",
    },
  },
  clearScreen: false,
  server: {
    port: 1420,
    strictPort: true,
    host: host || false,
    hmr: host ? { protocol: "ws", host, port: 1421 } : undefined,
    watch: { ignored: ["**/src-tauri/**"] },
  },
})
```

### 注意事项

- Vite 配置应为同步函数导出，避免 async 导致类型错误。
- 路径别名和 TS 配置中的路径要对应。
- 忽略 `src-tauri` 目录防止不必要的监控。

---

## 6. 常见问题汇总与解决方案

|   |   |
|---|---|
|问题描述|解决方案|
|`import logger from '@/utils/logger'` 报错找不到|确认 tsconfig.json 中路径别名配置正确，使用 `"moduleResolution": "nodenext"`，保证 ts 能解析别名。|
|`.ts` 文件引入报错，需要改 `.js` 才能不报错|检查 `tsconfig` 和 Vite 配置是否正确，确保模块解析策略一致。|
|ESLint 报错找不到模块或解析失败|安装并配置 `@typescript-eslint/parser`，确保 parser 指向 tsconfig。|
|Husky 无法触发 git hook 或 lint-staged 命令找不到|确认 `husky install` 已执行，hook 脚本有执行权限，且依赖安装完整。|
|Vite 配置使用异步导出导致类型错误|改用同步函数形式导出配置文件。|
|ESLint 和 Prettier 格式冲突|安装并启用 `eslint-config-prettier` 和 `eslint-plugin-prettier`，关闭冲突规则。|

---

## 7. 总结

- **升级 TypeScript** 解决类型安全，增强代码可维护性。
- **ESLint + Prettier** 保持代码风格统一，避免格式问题。
- **Husky + lint-staged** 自动化提交代码检查和格式化，保障代码质量。
- **Vite 配置** 需合理，避免异步配置导致的类型不匹配。
- **路径别名和模块解析** 需同步配置，避免引入问题。
- **持续迭代与调试** 过程中遇到的问题可总结并纳入知识库。

---

如果你想，我可以帮你生成以上提到的关键配置文件样板，以及针对你项目结构定制的配置示例。需要的话告诉我！
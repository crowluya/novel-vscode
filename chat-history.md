# Novel VSCode 项目聊天记录

## 项目背景
这是一个基于 [novel](https://github.com/steven-tey/novel) 的 VSCode 插件，基于 [hello-world-react-vite](https://github.com/microsoft/vscode-webview-ui-toolkit-samples/tree/main/frameworks/hello-world-react-vite) 示例开发。

## 主要任务
将项目修改为支持 VSCode 1.70 版本并打包。

## 执行步骤

### 1. 安装依赖
首先安装了 pnpm 7.x 版本：
```bash
npm install -g pnpm@7
```

### 2. 更新项目配置
修改了以下文件：

#### package.json 更新
- 将 VSCode 引擎版本从 `^1.75.0` 更新到 `^1.70.0`
- 更新 `@types/vscode` 依赖版本到 `^1.70.0`
- 添加了 `activationEvents` 配置

#### webview-ui/package.json 更新
- 更新 `@vscode/webview-ui-toolkit` 到 `^1.0.0`

### 3. 构建过程
1. 删除旧的依赖：
```bash
Remove-Item -Recurse -Force node_modules, webview-ui/node_modules
```

2. 安装主项目依赖：
```bash
pnpm install
```

3. 安装 webview-ui 依赖：
```bash
cd webview-ui && pnpm install
```

4. 构建项目：
```bash
pnpm build
```

5. 打包插件：
```bash
vsce package --no-dependencies
```

## 构建结果
成功生成了插件文件 `novel-vscode-0.4.0.vsix`，大小约为 568.65 KB。

## 安装方法
可以通过以下方式安装插件：

1. 在 VSCode 1.70 中：
   - 按 `Ctrl+Shift+P`
   - 输入 "Install from VSIX"
   - 选择 `novel-vscode-0.4.0.vsix` 文件

2. 或者通过命令行：
```bash
code --install-extension novel-vscode-0.4.0.vsix
```

## 注意事项
1. 构建过程中出现了一些警告：
   - 一些 chunk 大小超过 500 KiB
   - 一些 peer dependencies 缺失
   但这些不会影响插件的基本功能。

2. 项目使用了较老的依赖版本：
   - TypeScript 4.1.3
   - esbuild 0.19.2
   - @types/vscode 1.70.0

## 最终配置

### package.json
```json
{
  "name": "novel-vscode",
  "icon": "assets/icon.png",
  "displayName": "novel-vscode",
  "description": "A quick integration of Novel.sh editor to VSCode",
  "version": "0.4.0",
  "publisher": "bennykok",
  "private": true,
  "engines": {
    "vscode": "^1.70.0"
  },
  "activationEvents": [
    "onCustomEditor:novel.editMarkdown"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/BennyKok/novel-vscode"
  },
  "main": "./out/main.js",
  "contributes": {
    "commands": [
      {
        "command": "novel.editWith",
        "title": "Edit file with Novel"
      },
      {
        "command": "novel.editBoldFont",
        "title": "Bold Font"
      }
    ],
    "keybindings": [
      {
        "command": "novel.editBoldFont",
        "key": "ctrl+b",
        "mac": "cmd+b",
        "when": "activeCustomEditorId === 'novel.editMarkdown'"
      }
    ],
    "customEditors": [
      {
        "viewType": "novel.editMarkdown",
        "displayName": "Markdown (Novel)",
        "selector": [
          {
            "filenamePattern": "{*.md,*.mdx}"
          }
        ],
        "priority": "default"
      }
    ]
  },
  "scripts": {
    "install:all": "pnpm install && cd webview-ui && npm install",
    "start:webview": "cd webview-ui && pnpm run start",
    "build:webview": "cd webview-ui && pnpm run build",
    "build": "pnpm build:webview && pnpm compile && npm run esbuild-base -- --minify",
    "vscode:prepublish": "pnpm build",
    "esbuild-base": "esbuild ./src/extension.ts --bundle --outfile=out/main.js --external:vscode --format=cjs --platform=node",
    "compile": "tsc -p ./",
    "watch": "tsc -watch -p ./",
    "pretest": "pnpm run compile && npm run lint",
    "lint": "eslint src --ext ts",
    "package": "pnpm vsce package --no-dependencies",
    "publish-vsce": "pnpm vsce publish --no-dependencies"
  },
  "devDependencies": {
    "@types/glob": "^7.1.3",
    "@types/node": "^12.11.7",
    "@types/vscode": "^1.70.0",
    "@typescript-eslint/eslint-plugin": "^4.14.1",
    "@typescript-eslint/parser": "^4.14.1",
    "esbuild": "^0.19.2",
    "eslint": "^7.19.0",
    "glob": "^7.1.6",
    "prettier": "^2.2.1",
    "typescript": "^4.1.3",
    "vscode-test": "^1.5.0"
  }
}
```

### webview-ui/package.json
```json
{
  "name": "hello-world",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "@tiptap/core": "^2.1.7",
    "@tiptap/extension-table": "^2.1.7",
    "@tiptap/extension-table-cell": "^2.1.7",
    "@tiptap/extension-table-header": "^2.1.7",
    "@tiptap/extension-table-row": "^2.1.7",
    "@vscode/webview-ui-toolkit": "^1.0.0",
    "markdown-it-container": "^3.0.0",
    "markdown-it-mark": "^3.0.1",
    "next-themes": "^0.2.1",
    "novel": "^0.1.20",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "tiptap-markdown": "^0.8.2"
  },
  "devDependencies": {
    "@types/react": "^18.2.21",
    "@types/react-dom": "^18.2.7",
    "@types/vscode-webview": "^1.57.0",
    "@vitejs/plugin-react": "^1.0.7",
    "typescript": "^4.4.4",
    "vite": "^2.9.13"
  }
} 
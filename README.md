# [novel](https://github.com/steven-tey/novel)-vscode

A vscode extension for [novel](https://github.com/steven-tey/novel)

Based on [hello-world-react-vite](https://github.com/microsoft/vscode-webview-ui-toolkit-samples/tree/main/frameworks/hello-world-react-vite) for vscode

## Installation

Download on [vscode marketplace](https://marketplace.visualstudio.com/items?itemName=bennykok.novel-vscode)

or

Building `vsix`

```
pnpm package
```

## Features

- Edit markdown file directly in vscode with novel editor `[ctrl/cmd]+shift+p` -&gt; `Edit Markdown with Novel`
- Debounced Auto save

https://github.com/BennyKok/novel-vscode/assets/18395202/8f2d400b-2a03-4ea3-96a2-6ce1eb9869b8

Upon installation, novel-vscode will opens up by default for md/mdx files, to revert back, right click on md file -&gt; open with -&gt; configure default

https://github.com/BennyKok/novel-vscode/assets/18395202/407e6651-efa8-45a8-9529-d3a2083fc4bb

## Development

```bash
# Navigate into sample directory
cd novel-vscode

# Install dependencies for both the extension and webview UI source code
pnpm install:all

# Build webview UI source code
pnpm build:webview

# Open sample in VS Code
code .
```

Once the sample is open inside VS Code you can run the extension by doing the following:

1. Press `F5` to open a new Extension Development Host window
2. Inside the host window, open the command palette (`Ctrl+Shift+P` or `Cmd+Shift+P` on Mac) and type `Edit Markdown with Novel`1. Press `F5` to open a new Extension Development Host window
2. Inside the host window, open the command palette (`Ctrl+Shift+P` or `Cmd+Shift+P` on Mac) and type `Edit Markdown with Novel`

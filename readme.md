# learn github workflow

这个仓库用于学习和记录 GitHub Actions 的基本用法。

## 目录说明

GitHub Actions 的配置文件放在仓库根目录下的 `.github/workflows/` 目录中。

典型结构：

```text
.
├── .github/
│   └── workflows/
│       └── demo.yml
└── readme.md
```

说明：

- `.github/workflows/` 是 GitHub Actions 约定目录。
- 每个 `.yml` 或 `.yaml` 文件对应一个 workflow。
- 文件名可以自定义，例如 `demo.yml`、`ci.yml`、`deploy.yml`。
- workflow 提交到远程仓库后，可在 GitHub 页面 `Actions` 标签页查看运行记录。

## 最小示例

在 `.github/workflows/demo.yml` 中添加：

```yaml
name: GitHub Actions Demo

on:
  push:
    branches:
      - master
  workflow_dispatch:

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Print message
        run: echo "Hello GitHub Actions"
```

这个 workflow 会在两种情况下运行：

- 向 `master` 分支 push 代码时自动运行。
- 在 `Actions` 标签页手动点击运行。

## 常用触发方式

### push 触发

```yaml
on:
  push:
    branches:
      - master
```

### pull request 触发

```yaml
on:
  pull_request:
    branches:
      - master
```

### 手动触发

```yaml
on:
  workflow_dispatch:
```

### 定时触发

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

## jobs 和 steps

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run command
        run: echo "build project"
```

## 查看运行结果

1. 打开 GitHub 仓库页面。
2. 点击顶部的 `Actions` 标签页。
3. 选择对应 workflow。
4. 点击某一次运行记录。
5. 展开 job 和 step 查看日志。

## 常见问题

### Actions 没有运行

检查以下内容：

- workflow 文件是否放在 `.github/workflows/` 目录下。
- 文件后缀是否为 `.yml` 或 `.yaml`。
- YAML 缩进是否正确。
- `on` 触发条件是否匹配当前操作。
- workflow 文件是否已经 push 到 GitHub 远程仓库。

### 找不到命令或依赖

GitHub Actions 每次运行都是新的 runner 环境，需要在 workflow 中安装依赖。例如 Node.js 项目通常需要：

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: 20

- name: Install dependencies
  run: npm install
```

### 默认分支不是 master

如果仓库默认分支是 `main`，需要把示例中的 `master` 改成 `main`。

## 学习链接

- [GitHub Actions 入门教程](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)
- [GitHub Actions 官方文档](https://docs.github.com/actions)

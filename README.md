# Learn Hugo

Hugo 静态网站生成器学习站点 —— 从安装到部署的完整中文教程。

## 站点信息

- 主题：[Ananke](https://github.com/thegeeklab/hugo-geekdoc)（用户指南主题）
- 语言：简体中文（`zh-Hans-CN`）
- Hugo 版本：≥ 0.145.0（extended）

## 教程目录

按推荐阅读顺序排列：

| # | 文章 | 主题 |
|---|------|------|
| 1 | [Hugo 安装与环境配置](content/posts/hugo-install.md) | 安装、环境变量、版本选择 |
| 2 | [Hugo 快速开始](content/posts/hugo-quickstart.md) | 创建站点、安装主题、本地预览 |
| 3 | [Hugo 配置文件详解](content/posts/hugo-config.md) | `hugo.toml`、菜单、参数 |
| 4 | [Hugo 内容管理与 Front Matter](content/posts/hugo-content.md) | Front Matter、archetypes、页面 bundle |
| 5 | [Hugo 模板与布局系统](content/posts/hugo-templates.md) | 模板查找顺序、baseof、partial |
| 6 | [Hugo 短代码](content/posts/hugo-shortcodes.md) | 内置短代码、自定义短代码 |
| 7 | [Hugo 分类与标签](content/posts/hugo-taxonomies.md) | taxonomies、自定义分类法 |
| 8 | [Hugo 站点部署](content/posts/hugo-deploy.md) | GitHub Pages、Netlify、CI/CD |

## 本地运行

```bash
# 启动开发服务器（默认 http://localhost:1313）
hugo server

# 构建生产版本到 public/
hugo --gc --minify
```

> 主题通过 git submodule 引入，首次克隆请加 `--recursive`：
> ```bash
> git clone --recursive <repo-url>
> # 已有仓库补齐子模块：
> git submodule update --init --recursive
> ```

## 目录结构

```
learn-hugo/
├── archetypes/         # 内容模板（hugo new 默认使用）
│   └── default.md
├── content/            # Markdown 内容
│   └── posts/          # 8 篇教程
├── themes/ananke/      # 主题（git submodule）
├── hugo.toml           # 站点配置
└── .github/workflows/  # GitHub Pages 自动部署
```

## 部署

推送到 `master` 分支后会自动通过 GitHub Actions 构建并部署到 GitHub Pages
（见 `.github/workflows/hugo.yaml`）。

+++
date = '2025-06-02T00:00:00+08:00'
draft = false
title = 'Hugo 快速开始'
description = '通过实际步骤创建第一个 Hugo 站点，安装主题，添加内容并在本地预览。'
tags = ['快速开始', '入门']
categories = ['基础']
+++

用 5 分钟走完一遍完整流程：创建站点、安装 Ananke 主题、新增文章、本地实时预览。

<!--more-->

## 创建新站点

使用 `hugo new site` 命令创建一个新的 Hugo 项目：

```bash
hugo new site learn-hugo
cd learn-hugo
```

这个命令会生成以下目录结构：

```
learn-hugo/
├── archetypes/     # 内容模板
├── assets/         # 需要处理的资源文件（Sass、JS 等）
├── content/        # Markdown 内容文件
├── data/           # 数据文件（JSON、YAML、TOML）
├── hugo.toml       # 站点配置文件
├── i18n/           # 国际化翻译文件
├── layouts/        # 自定义布局模板
├── static/         # 静态文件（图片、CSS、JS 等）
└── themes/         # 主题目录
```

## 安装主题

Hugo 主题存储在 `themes/` 目录下。推荐使用 Git Submodule 方式安装主题：

```bash
# 初始化 Git 仓库
git init

# 安装 Ananke 主题
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

然后在 `hugo.toml` 中指定主题：

```toml
theme = 'ananke'
```

## 添加第一篇内容

使用 `hugo new` 命令创建新文章：

```bash
hugo new posts/my-first-post.md
```

Hugo 会自动从 `archetypes/default.md` 复制模板，生成带 front matter 的新文件：

```markdown
+++
date = '2025-06-02T08:00:00+08:00'
draft = false
title = 'My First Post'
description = ''
tags = []
categories = []
+++

## 在这里开始写作

你的 Markdown 内容写在这里...
```

## 启动本地开发服务器

```bash
hugo server -D
```

参数说明：

- `-D` 或 `--buildDrafts`：包含草稿文章
- `--port 1313`：指定端口（默认 1313）
- `--bind 0.0.0.0`：允许局域网访问
- `--disableFastRender`：禁用快速渲染（修改了模板时使用）

运行后访问 `http://localhost:1313/` 即可预览站点。Hugo 支持热重载，修改内容后浏览器会自动刷新。

## 构建生产版本

```bash
hugo
# 或带优化参数
hugo --minify --gc
```

构建产物会输出到 `public/` 目录，可以直接部署到任何静态文件服务器。

## 站点配置文件

新建站点的 `hugo.toml` 基本结构：

```toml
baseURL = 'https://example.com/'
languageCode = 'zh-Hans-CN'
title = '我的网站'
theme = 'ananke'
```

## 总结

至此你已经完成了：
1. ✅ 创建 Hugo 站点
2. ✅ 安装主题
3. ✅ 添加第一篇内容
4. ✅ 本地预览
5. ✅ 构建生产版本

下一步可以深入学习 [Hugo 配置详解](/posts/hugo-config/)。

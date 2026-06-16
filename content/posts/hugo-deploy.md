+++
date = '2025-06-08T00:00:00+08:00'
draft = false
title = 'Hugo 站点部署'
description = '将 Hugo 站点部署到 GitHub Pages、Netlify、Vercel 等平台，包括 CI/CD 自动构建配置。'
tags = ['部署', 'CI/CD']
categories = ['进阶']
+++

把构建好的站点部署到 GitHub Pages、Netlify、Vercel，并用 GitHub Actions 配置自动构建与发布。

<!--more-->

## 构建生产站点

部署前先构建优化版本：

```bash
hugo --minify --gc --baseURL "https://your-domain.com/"
```

参数说明：

- `--minify`：压缩 HTML/CSS/JS
- `--gc`：清理缓存
- `--baseURL`：覆盖配置文件中的 baseURL

生成的静态文件在 `public/` 目录中，可以直接部署。

## 部署到 GitHub Pages

### 方式一：GitHub Actions（推荐）

在 `.github/workflows/deploy.yml` 中配置自动化部署：

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: [master]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.145.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb \
            https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
            && sudo dpkg -i ${{ runner.temp }}/hugo.deb

      - name: Install Dart Sass
        run: sudo snap install dart-sass

      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### GitHub 仓库设置

1. 进入仓库 **Settings → Pages**
2. Source 选择 **GitHub Actions**
3. 推送代码到指定分支，自动触发部署

### 方式二：手动部署 gh-pages 分支

```bash
# 构建站点
hugo --minify

# 将 public/ 内容推送到 gh-pages 分支
cd public
git init
git add .
git commit -m "Deploy"
git remote add origin https://github.com/username/repo.git
git push -f origin master:gh-pages
```

### 方式三：使用 gh-pages 工具

```bash
npm install -g gh-pages
hugo --minify
gh-pages -d public
```

## 部署到 Netlify

Netlify 支持 Hugo 零配置部署。

### 连接 Git 仓库

1. 注册 [Netlify](https://www.netlify.com/)
2. 点击 **Add new site → Import an existing project**
3. 选择 Git 仓库

### Netlify 配置

创建 `netlify.toml`：

```toml
[build]
  publish = "public"
  command = "hugo --gc --minify"

[build.environment]
  HUGO_VERSION = "0.145.0"

[context.deploy-preview]
  command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.branch-deploy]
  command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"
```

### 自定义域名

在 Netlify 控制台添加自定义域名，并配置 DNS：

```
类型: CNAME
名称: @
值:   your-site.netlify.app
```

Netlify 自动提供 HTTPS 证书。

## 部署到 Vercel

Vercel 同样支持 Hugo 零配置部署。

1. 注册 [Vercel](https://vercel.com/)
2. 导入 Git 仓库
3. Vercel 自动检测 Hugo 项目并配置构建

如需自定义配置，创建 `vercel.json`：

```json
{
  "buildCommand": "hugo --gc --minify",
  "outputDirectory": "public",
  "installCommand": "brew install hugo"
}
```

## 部署到 Cloudflare Pages

1. 注册 [Cloudflare Pages](https://pages.cloudflare.com/)
2. 连接 Git 仓库
3. 配置构建：
   - **构建命令**：`hugo --gc --minify`
   - **输出目录**：`public`
   - **环境变量**：`HUGO_VERSION = 0.145.0`

## 部署到自建服务器

### 使用 rsync

```bash
hugo --minify
rsync -avz --delete public/ user@server:/var/www/html/
```

### 使用 Nginx 配置

```
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/html;

    location / {
        try_files $uri $uri/ =404;
    }

    # 404 页面
    error_page 404 /404.html;
}
```

## 构建优化技巧

```bash
# 生产构建的完整命令
hugo \
    --minify \           # 压缩输出
    --gc \               # 清理缓存
    --cleanDestinationDir \  # 清空目标目录
    --baseURL "https://your-domain.com/"
```

## 总结

至此你已经掌握了 Hugo 的完整使用流程：

1. ✅ [安装环境]({{< ref "posts/hugo-install.md" >}})
2. ✅ [快速开始]({{< ref "posts/hugo-quickstart.md" >}})
3. ✅ [配置文件]({{< ref "posts/hugo-config.md" >}})
4. ✅ [内容管理]({{< ref "posts/hugo-content.md" >}})
5. ✅ [模板系统]({{< ref "posts/hugo-templates.md" >}})
6. ✅ [短代码]({{< ref "posts/hugo-shortcodes.md" >}})
7. ✅ [分类标签]({{< ref "posts/hugo-taxonomies.md" >}})
8. ✅ 站点部署

推荐部署方案：

| 平台 | 优势 | 适合人群 |
|------|------|----------|
| GitHub Pages | 免费、与 GitHub 深度集成 | 开源项目、个人博客 |
| Netlify | 功能丰富、预览部署 | 注重体验的开发者 |
| Vercel | 极速构建、全球 CDN | 前端开发者 |
| Cloudflare Pages | 全球边缘网络 | 追求极致性能 |

祝贺你完成 Hugo 学习之旅！🎉

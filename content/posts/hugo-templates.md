+++
date = '2025-06-05T00:00:00+08:00'
draft = false
title = 'Hugo 模板与布局系统'
description = '深入理解 Hugo 模板引擎、模板查找顺序、baseof 继承、partial 复用和 block 定义。'
tags = ['模板', '布局']
categories = ['核心']
+++

理清 Hugo 的模板查找顺序、`baseof.html` 继承机制、partial 复用与 block 覆盖，看懂主题是如何被组织起来的。

<!--more-->

## 模板引擎概述

Hugo 使用 Go 的 `html/template` 包作为模板引擎，语法类似于 Jekyll 的 Liquid 或 Hexo 的 EJS，但有自己的特点。

## 模板查找顺序

Hugo 为每种页面类型按照特定优先级查找模板。以首页为例：

```
layouts/
├── index.html          # 最高优先级
├── home.html           # 次优先级
├── list.html           # 通用列表模板
└── _default/
    ├── baseof.html     # 基础框架
    └── list.html       # 默认列表模板
```

### 页面模板查找顺序

- **首页**：`index.html` → `home.html` → `list.html` → `_default/list.html`
- **单页**：`post/single.html` → `_default/single.html`
- **列表页**：`section/posts.html` → `_default/list.html`
- **分类页**：`taxonomy/tag.html` → `_default/list.html`

## baseof 模板（基础框架）

`layouts/_default/baseof.html` 定义了所有页面共享的 HTML 框架：

```html
<!DOCTYPE html>
<html lang="{{ site.LanguageCode }}">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ block "title" . }}{{ .Site.Title }}{{ end }}</title>
    {{ partialCached "head/css.html" . }}
</head>
<body>
    {{ partial "header.html" . }}
    
    <main>
        {{ block "main" . }}{{ end }}
    </main>
    
    {{ partial "footer.html" . }}
</body>
</html>
```

> 关键概念：`block` 定义了可被子模板覆盖的区域。

## 单页模板

`layouts/_default/single.html`：

```html
{{ define "main" }}
<article>
    <header>
        <h1>{{ .Title }}</h1>
        <time datetime="{{ .Date.Format "2006-01-02" }}">
            {{ .Date.Format "2006-01-02" }}
        </time>
    </header>
    
    <div class="content">
        {{ .Content }}
    </div>
    
    <footer>
        {{ with .Params.tags }}
        <div class="tags">
            {{ range . }}
            <a href="{{ "/tags/" | relLangURL }}{{ . | urlize }}">{{ . }}</a>
            {{ end }}
        </div>
        {{ end }}
    </footer>
</article>
{{ end }}
```

## 列表模板

`layouts/_default/list.html`：

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>

{{ .Content }}

<div class="post-list">
    {{ range .Pages }}
    <article>
        <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
        <time>{{ .Date.Format "2006-01-02" }}</time>
        <p>{{ .Summary }}</p>
    </article>
    {{ end }}
</div>
{{ end }}
```

## Partial 模板（复用片段）

`partial` 是 Hugo 中代码复用的核心机制。将可复用的 UI 组件放在 `layouts/partials/` 下。

### 创建 Partial

`layouts/partials/header.html`：

```html
<header>
    <nav>
        <a href="{{ .Site.Home.RelPermalink }}" class="logo">
            {{ .Site.Title }}
        </a>
        <ul>
            {{ range .Site.Menus.main }}
            <li>
                <a href="{{ .URL }}">{{ .Name }}</a>
            </li>
            {{ end }}
        </ul>
    </nav>
</header>
```

### 使用 Partial

```html
{{ partial "header.html" . }}
{{ partial "sidebar.html" . }}
```

### 带参数传递

```html
{{ partial "card.html" (dict "title" .Title "image" .Params.hero) }}
```

### PartialCached（缓存优化）

```html
<!-- 缓存整个站点的 footer，提升构建性能 -->
{{ partialCached "footer.html" . }}
```

## 页面变量速查

| 变量 | 说明 |
|------|------|
| `.Title` | 页面标题 |
| `.Content` | 渲染后的正文 HTML |
| `.Summary` | 摘要 |
| `.Date` | 发布日期 |
| `.RelPermalink` | 相对永久链接 |
| `.Permalink` | 绝对永久链接 |
| `.Params.xxx` | 自定义 front matter 参数 |
| `.Site.Title` | 站点标题 |
| `.Site.Pages` | 所有页面 |
| `.Pages` | 当前页面的子页面 |

## 分页

列表模板中使用分页：

```html
{{ $paginator := .Paginate .Pages }}
{{ range $paginator.Pages }}
    <!-- 渲染每篇文章 -->
{{ end }}

<!-- 分页导航 -->
{{ template "_internal/pagination.html" . }}
```

## 下一步

掌握模板系统后，继续学习 [Hugo 短代码](/posts/hugo-shortcodes/)。

+++
date = '2025-06-07T00:00:00+08:00'
draft = false
title = 'Hugo 分类与标签（Taxonomies）'
description = '学习 Hugo 的分类系统，包括标签（tags）和分类（categories）的配置与使用，以及自定义分类法。'
tags = ['分类', '标签', 'Taxonomies']
categories = ['核心']
+++

了解 Hugo 默认的 tags（标签）与 categories（分类）两种分类法，并学会自定义新的分类维度来组织内容。

<!--more-->

## 什么是 Taxonomy

Taxonomy（分类法）是 Hugo 对内容进行分类和分组的核心机制。它允许你按照自定义维度组织文章，例如按标签、分类、系列等。

## 默认分类法

Hugo 默认提供两种分类法：

- **tags**（标签）：文章的关键字标注
- **categories**（分类）：文章的大类归属

### 在文章中使用

在 Front Matter 中定义：

```markdown
+++
title = 'Hugo 入门教程'
tags = ['hugo', 'ssg', '入门']
categories = ['技术', '前端']
+++
```

### 分类页面 URL

Hugo 自动为每个分类创建页面：

- `/tags/hugo/` — 所有标记为 "hugo" 的文章列表
- `/categories/技术/` — 所有 "技术" 分类下的文章列表

## 配置分类法

在 `hugo.toml` 中自定义分类法：

```toml
[taxonomies]
  tag = 'tags'
  category = 'categories'
  series = 'series'
  author = 'authors'
```

这样配置后，你可以在文章中定义自定义分类维度：

```markdown
+++
title = 'Hugo 入门教程'
tags = ['hugo', 'ssg']
categories = ['技术']
series = ['Hugo 学习系列']
author = 'Qingchen'
+++
```

### 添加分类到 Front Matter 模板

更新 `archetypes/default.md`：

```markdown
+++
date = '{{ .Date }}'
draft = false
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
description = ''
tags = []
categories = []
series = []
+++
```

## 分类模板

### 分类列表页模板

`layouts/_default/terms.html` — 显示所有标签的列表：

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>
<div class="terms">
    {{ $type := .Type }}
    {{ range $key, $value := .Data.Terms.Alphabetical }}
        {{ $name := .Name }}
        {{ $count := .Count }}
        <a href="{{ "/" | relLangURL }}{{ $type | urlize }}/{{ $name | urlize }}" 
           class="term-item">
            {{ $name }} <sup>{{ $count }}</sup>
        </a>
    {{ end }}
</div>
{{ end }}
```

### 单个分类页面模板

`layouts/_default/taxonomy.html` — 显示属于某个标签的文章列表：

```html
{{ define "main" }}
<h1>标签: {{ .Title }}</h1>
{{ .Content }}
<div class="post-list">
    {{ range .Pages }}
    <article>
        <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
        <time>{{ .Date.Format "2006-01-02" }}</time>
    </article>
    {{ end }}
</div>
{{ end }}
```

## 使用 .GetTerms

在模板中获取文章的关联标签：

```html
{{ range .GetTerms "tags" }}
    <a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a>
{{ end }}
```

在单页模板中展示当前文章的标签：

```html
<div class="tags">
    {{ range .GetTerms "tags" }}
    <a href="{{ .RelPermalink }}" class="tag">#{{ .LinkTitle }}</a>
    {{ end }}
</div>
```

## 站点级分类查询

### 获取所有标签

```html
<ul>
{{ range .Site.Taxonomies.tags }}
    <li><a href="{{ .Page.RelPermalink }}">{{ .Page.Title }}</a> ({{ .Count }})</li>
{{ end }}
</ul>
```

### 获取热门标签

```html
{{ range first 10 .Site.Taxonomies.tags.ByCount }}
    <a href="{{ .Page.RelPermalink }}" style="font-size: {{ mul .Count 0.5 }}rem;">
        {{ .Page.Title }} ({{ .Count }})
    </a>
{{ end }}
```

## 在文章间创建关联

使用分类法创建文章间的引用关系：

```html
<!-- 相关文章推荐 -->
<h3>相关文章</h3>
<ul>
    {{ $currentPage := . }}
    {{ range .Site.RegularPages.Related $currentPage | first 3 }}
    <li>
        <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    </li>
    {{ end }}
</ul>
```

## 最佳实践

1. **标签要精准**：每个文章用 3-5 个关键词标签
2. **分类不宜过多**：2-3 个大类即可，避免分类碎片化
3. **命名一致性**：同一概念始终使用相同的标签名
4. **合理使用自定义分类**：如 `series` 用于系列文章

## 下一步

分类系统让站点内容更有组织性。最后来看看 [Hugo 站点部署](/posts/hugo-deploy/)。

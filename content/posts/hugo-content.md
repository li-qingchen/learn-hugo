+++
date = '2025-06-04T00:00:00+08:00'
draft = false
title = 'Hugo 内容管理与 Front Matter'
description = '学习 Hugo 的内容组织方式、Front Matter 字段、archetypes 模板机制以及页面 bundle 概念。'
tags = ['内容管理', 'Front Matter']
categories = ['核心']
+++

掌握 Hugo 的内容组织方式：Front Matter 常用字段、archetypes 模板机制、以及 Leaf Bundle 与 Branch Bundle 的区别。

<!--more-->

## 内容目录结构

Hugo 的所有内容都放在 `content/` 目录下，目录结构直接映射为网站的 URL 结构：

```
content/
├── _index.md           # 首页内容
├── about.md            # 访问 /about/
├── posts/
│   ├── _index.md       # 文章列表页 /posts/
│   ├── hello.md        # 访问 /posts/hello/
│   └── hugo/
│       └── intro.md    # 访问 /posts/hugo/intro/
```

## Front Matter

Front Matter 是 Markdown 文件顶部的元数据区域，Hugo 支持三种格式。

### TOML 格式（推荐）

```markdown
+++
date = '2025-06-04T08:00:00+08:00'
draft = false
title = '文章标题'
description = '文章描述'
slug = 'custom-slug'
tags = ['hugo', 'tutorial']
categories = ['技术']
weight = 10
+++

正文内容...
```

### YAML 格式

```yaml
---
date: '2025-06-04'
draft: false
title: '文章标题'
---
```

### 常用 Front Matter 字段

| 字段 | 说明 | 示例 |
|------|------|------|
| `title` | 文章标题 | `'Hugo 入门'` |
| `date` | 发布日期 | `'2025-06-04T08:00:00+08:00'` |
| `draft` | 是否为草稿 | `true` / `false` |
| `description` | 文章描述（SEO） | `'Hugo 入门教程'` |
| `slug` | 自定义 URL 路径 | `'hugo-guide'` |
| `tags` | 标签 | `['hugo', 'ssg']` |
| `categories` | 分类 | `['技术']` |
| `weight` | 排序权重 | `10` |
| `aliases` | 重定向别名 | `['/old-url/']` |
| `lastmod` | 最后修改时间 | `'2025-06-04'` |
| `type` | 内容类型 | `'post'` |
| `layout` | 自定义布局 | `'wide'` |

## Archetypes（内容模板）

Archetypes 定义了创建新内容时的默认模板。默认模板位于 `archetypes/default.md`：

```markdown
+++
date = '{{ .Date }}'
draft = false
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
description = ''
tags = []
categories = []
+++
```

### 自定义 Archetype

可以为不同内容类型创建特定模板。例如为博客文章创建 `archetypes/posts.md`：

```markdown
+++
date = '{{ .Date }}'
draft = false
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
description = ''
tags = ['untagged']
categories = ['未分类']
+++

## 引言

## 正文

## 总结
```

使用自定义 archetype：

```bash
hugo new posts/my-article.md
```

## Page Bundles（页面束）

Hugo 支持两种内容组织方式：

### Leaf Bundle（叶子束）

以 `index.md` 命名的目录，所有资源文件放在同一目录：

```
content/posts/my-post/
├── index.md           # 内容文件
├── hero.jpg           # 配图
├── diagram.png        # 图示
└── data.csv           # 数据文件
```

文章内引用图片：

```markdown
![Hero](./hero.jpg)
```

### Branch Bundle（分支束）

以 `_index.md` 命名的目录，可以包含子页面：

```
content/docs/
├── _index.md          # 文档首页
├── install.md         # 安装指南
└── usage.md           # 使用指南
```

## 摘要（Summary）

Hugo 自动从内容中提取摘要：

- 默认取前 70 个单词
- 或使用 `<!--more-->` 手动分隔：

```markdown
这里是摘要内容。

<!--more-->

这里是正文，不会出现在摘要中。
```

## 下一步

学习完内容管理，可以深入探索 [Hugo 模板与布局系统](/posts/hugo-templates/)。

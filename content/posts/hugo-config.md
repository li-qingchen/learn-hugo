+++
date = '2025-06-03T00:00:00+08:00'
draft = false
title = 'Hugo 配置文件详解'
description = '全面了解 hugo.toml 配置文件的各项配置项，包括站点参数、菜单、多语言支持等。'
tags = ['配置', '进阶']
categories = ['基础']
+++

了解 `hugo.toml` 的核心字段：站点信息、菜单导航、参数（params）、多语言配置，以及如何按环境拆分配置文件。

<!--more-->

## 配置文件概述

Hugo 的配置文件位于项目根目录，支持三种格式：

- `hugo.toml`（推荐）
- `hugo.yaml`
- `hugo.json`

Hugo 会自动检测并加载其中一种。本文使用 TOML 格式。

## 基础配置

```toml
# 站点基础 URL，用于生成绝对链接
baseURL = 'https://example.com/'

# 语言代码，影响日期格式等
languageCode = 'zh-Hans-CN'

# 站点标题
title = '我的网站'

# 主题名称
theme = 'ananke'

# 分页设置
paginate = 10
```

## 自定义参数

使用 `[params]` 定义站点级别的自定义参数，主题通常会读取这些参数：

```toml
[params]
  # 站点描述
  description = '一个关于技术的博客'

  # 作者信息
  author = 'Qingchen'

  # 社交链接
  [params.anchored]
    github = 'https://github.com/li-qingchen'
    twitter = 'https://twitter.com/xxx'

  # 页脚版权信息
  copyright = '© 2025 Qingchen'
```

## 菜单配置

Hugo 支持定义多个菜单，主题通过 `site.Menus.main` 等变量访问：

```toml
[[menu.main]]
  name = '首页'
  url = '/'
  weight = 1

[[menu.main]]
  name = '文章'
  url = '/posts/'
  weight = 2

[[menu.main]]
  name = '关于'
  url = '/about/'
  weight = 3
```

支持嵌套菜单：

```toml
[[menu.main]]
  name = '分类'
  url = '/categories/'
  weight = 4
  # 子菜单
  [[menu.main]]
    name = '技术'
    url = '/categories/tech/'
    parent = '分类'
```

## 多语言配置

Hugo 原生支持多语言站点：

```toml
defaultContentLanguage = 'zh-cn'

[languages]
  [languages.zh-cn]
    languageName = '中文'
    title = '我的网站'
    weight = 1

  [languages.en]
    languageName = 'English'
    title = 'My Site'
    weight = 2
```

多语言内容通过文件名后缀区分：

```
content/
├── posts/
│   ├── hello.md        # 默认语言（中文）
│   └── hello.en.md     # 英文版本
```

## 模块配置

Hugo Modules 是管理主题和组件的推荐方式：

```toml
[module]
  [[module.imports]]
    path = 'github.com/theNewDynamic/gohugo-theme-ananke'
```

使用模块化方式后，不再需要在 `themes/` 目录下手动管理主题。

## 构建选项

```toml
# 是否忽略缓存
ignoreCache = false

# 是否禁用别名（redirects）
disableAliases = true

# 是否控制字数统计行为
summaryLength = 70
```

## 完整配置示例

下面是一个生产中使用的配置示例：

```toml
baseURL = 'https://qingchen177.github.io/learn-hugo/'
languageCode = 'zh-Hans-CN'
title = 'Qingchen'
theme = 'ananke'

paginate = 10
summaryLength = 70

[params]
  author = 'Qingchen'
  description = 'Hugo 学习站点 - 从入门到部署'

[[menu.main]]
  name = '文章'
  url = '/posts/'
  weight = 1
```

## 下一步

掌握了配置文件后，可以继续学习 [内容管理与 Front Matter](/posts/hugo-content/)。

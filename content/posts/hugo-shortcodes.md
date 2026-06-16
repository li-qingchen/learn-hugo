+++
date = '2025-06-06T00:00:00+08:00'
draft = false
title = 'Hugo 短代码（Shortcodes）'
description = '学习 Hugo 内置短代码的使用方法，以及如何创建自定义短代码来扩展 Markdown 的功能。'
tags = ['短代码', 'Shortcodes']
categories = ['核心']
+++

用短代码在 Markdown 中嵌入 figure 图片、YouTube 视频、高亮提示等复杂内容，并学会编写自己的短代码。

<!--more-->

## 短代码是什么

短代码（Shortcodes）是 Hugo 的核心特性之一，让你可以在 Markdown 内容中嵌入 HTML 模板，实现 Markdown 本身不支持的复杂内容展示。

短代码的使用语法：

```markdown
{{</* shortcode-name param1 param2 */>}}
```

## 内置短代码

Hugo 提供了多个内置短代码，无需额外配置即可使用。

### figure（图片）

带标题的图片展示：

```markdown
{{</* figure
    src="/images/architecture.png"
    alt="系统架构图"
    title="图1：系统架构"
    caption="系统整体架构示意图"
    width="600px"
*/>}}
```

### gist（GitHub Gist）

```markdown
{{</* gist username gist-id */>}}
```

### highlight（代码高亮）

```markdown
{{</* highlight go "linenos=table,hl_lines=2 4-6" */>}}
package main

func main() {
    println("Hello, Hugo!")
}
{{</* /highlight */>}}
```

### youtube（视频嵌入）

```markdown
{{</* youtube dQw4w9WgXcQ */>}}
```

### tweet（推文嵌入）

```markdown
{{</* tweet user="username" id="tweet-id" */>}}
```

### instagram / vimeo

```markdown
{{</* instagram post-id */>}}
{{</* vimeo video-id */>}}
```

### ref / relref（内部链接）

自动生成正确的内部链接路径：

```markdown
[安装 Hugo]({{</* ref "posts/hugo-install" */>}})
[快速开始]({{</* relref "posts/hugo-quickstart" */>}})
```

## 自定义短代码

在 `layouts/shortcodes/` 目录下创建 HTML 模板文件。

### 简单短代码

`layouts/shortcodes/note.html`：

```html
<div class="note" style="padding: 1rem; background: #e8f4fd; border-left: 4px solid #2196f3; margin: 1rem 0;">
    <strong>📝 注意：</strong> {{ .Inner | markdownify }}
</div>
```

在 Markdown 中使用：

```markdown
{{</* note */>}}
这是一个需要注意的重要信息。
{{</* /note */>}}
```

### 带参数的短代码

`layouts/shortcodes/img.html`：

```html
{{ $src := .Get "src" }}
{{ $alt := .Get "alt" }}
{{ $width := .Get "width" | default "100%" }}
<figure>
    <img src="{{ $src }}" alt="{{ $alt }}" style="width: {{ $width }}; border-radius: 8px;">
    {{ with .Get "caption" }}
    <figcaption>{{ . }}</figcaption>
    {{ end }}
</figure>
```

使用：

```markdown
{{</* img src="/images/photo.jpg" alt="照片" width="600px" caption="一张美丽的照片" */>}}
```

### 命名参数 vs 位置参数

位置参数用法：

```markdown
{{</* myshortcode "参数1" "参数2" */>}}
```

短代码模板中获取：

```html
{{ .Get 0 }}  <!-- 获取第一个位置参数 -->
{{ .Get 1 }}  <!-- 获取第二个位置参数 -->
```

命名参数用法（推荐）：

```markdown
{{</* myshortcode name="Hugo" version="0.145.0" */>}}
```

短代码模板中获取：

```html
{{ .Get "name" }}
{{ .Get "version" }}
```

### 高级短代码示例

`layouts/shortcodes/tabs.html`：

```html
{{ $tabCount := .Get "count" | default 3 }}
<div class="tabs">
    <div class="tab-buttons">
        {{ range seq $tabCount }}
        {{ $idx := sub . 1 }}
        <button onclick="switchTab({{ $idx }})">
            {{ $.Get (printf "tab%d" $idx) }}
        </button>
        {{ end }}
    </div>
    {{ range seq $tabCount }}
    {{ $idx := sub . 1 }}
    <div class="tab-content" id="tab-{{ $idx }}">
        {{ $.Get (printf "content%d" $idx) | markdownify }}
    </div>
    {{ end }}
</div>
```

## 短代码嵌套

短代码可以相互嵌套：

```markdown
{{</* note */>}}
查看安装说明：{{</* ref "posts/hugo-install" */>}}
{{</* /note */>}}
```

注意：内部短代码需要使用 `{{</* ref "..." */>}}` 语法，而不是嵌套 `{{%/* figure */%}}` 形式。

## 最佳实践

1. **优先使用内置短代码**：Hugo 内置短代码经过充分测试且性能优异
2. **合理使用 Inner 内容**：需要包裹内容的短代码用 `{{</* shortcode */>}}...{{</* /shortcode */>}}` 语法
3. **参数使用命名方式**：提高可读性和维护性
4. **避免过度使用**：短代码会增加构建复杂度，适可而止

## 下一步

短代码为 Markdown 内容注入了编程能力。接下来学习 [Hugo 分类与标签]({{< ref "posts/hugo-taxonomies.md" >}})。

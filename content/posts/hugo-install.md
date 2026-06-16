+++
date = '2025-06-01T00:00:00+08:00'
draft = false
title = 'Hugo 安装与环境配置'
description = '详细介绍 Hugo 在不同操作系统上的安装方法，以及开发环境的基本配置。'
tags = ['安装', '入门']
categories = ['基础']
+++

本篇介绍在 macOS、Windows、Linux 上安装 Hugo 的常用方式，以及如何选择 extended（扩展）版本与标准版本。

<!--more-->

## Hugo 是什么

Hugo 是一个用 Go 语言编写的静态网站生成器，以构建速度快著称。它将 Markdown 内容文件与模板结合，生成纯静态 HTML 页面，非常适合搭建博客、文档站点和公司官网。

## 安装 Hugo

### macOS

使用 Homebrew 安装：

```bash
brew install hugo
```

验证安装：

```bash
hugo version
# 输出示例: hugo v0.145.0+extended darwin/arm64 ...
```

### Linux

使用包管理器安装或下载预编译二进制：

```bash
# Ubuntu/Debian 使用 snap
snap install hugo

# 或使用 apt（版本可能较旧）
sudo apt install hugo
```

也可以从 [GitHub Releases](https://github.com/gohugoio/hugo/releases) 下载预编译二进制文件。

### Windows

使用包管理器安装：

```powershell
# 使用 Chocolatey
choco install hugo-extended

# 使用 Scoop
scoop install hugo-extended

# 使用 Winget
winget install Hugo.Hugo.Extended
```

### 验证安装

```bash
hugo version
# 检查 Hugo 版本信息

hugo help
# 查看所有可用命令
```

## Hugo 版本选择

建议安装 **extended 版本**（包含 Sass/SCSS 支持）：

- `hugo`：标准版本
- `hugo_extended`：扩展版本（推荐）

大多数现代 Hugo 主题都需要 extended 版本，因为它支持 Sass/SCSS 编译。

## 基本命令速览

| 命令 | 说明 |
|------|------|
| `hugo new site <name>` | 创建新站点 |
| `hugo new <path>` | 基于 archetype 创建新内容 |
| `hugo server` | 启动本地开发服务器 |
| `hugo build` | 构建站点（输出到 public/） |
| `hugo version` | 查看版本 |
| `hugo help` | 查看帮助 |

## 下一步

安装完成后，可以进入下一篇文章 [Hugo 快速开始]({{< ref "posts/hugo-quickstart.md" >}})，创建你的第一个 Hugo 站点。

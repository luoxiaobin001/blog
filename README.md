# 我的 Hugo 博客

这是一个基于 [Hugo](https://gohugo.io/) 静态网站生成器和 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题构建的个人博客。

## 📋 目录结构

```
.
├── archetypes/          # 文章模板
│   └── default.md       # 默认文章模板
├── content/             # 博客内容
│   ├── posts/           # 文章目录
│   └── about/           # 关于页面
├── static/              # 静态资源
│   └── images/          # 图片资源
├── themes/              # 主题目录
│   └── PaperMod/        # PaperMod 主题（子模块）
├── .github/
│   └── workflows/       # GitHub Actions 工作流
│       └── deploy.yml   # 自动部署配置
├── hugo.toml            # Hugo 配置文件
└── README.md            # 项目说明
```

## 🚀 快速开始

### 前置要求

- [Hugo](https://gohugo.io/getting-started/installing/) (推荐最新版本)
- [Git](https://git-scm.com/)

### 本地运行

1. **克隆仓库**
   ```bash
   git clone https://github.com/luoxiaobin001/blog.git
   cd blog
   ```

2. **初始化子模块**
   ```bash
   git submodule update --init --recursive
   ```

3. **启动本地服务器**
   ```bash
   hugo server -D
   ```

4. **访问博客**
   
   打开浏览器访问 `http://localhost:1313`

### 创建新文章

```bash
hugo new posts/my-new-post.md
```

编辑生成的文件，修改 front matter 并添加内容。

## 📝 文章格式

每篇文章使用 Markdown 格式，并在开头包含 front matter：

```markdown
+++
date = '2024-05-04T12:00:00+08:00'
draft = false
title = '文章标题'
tags = ['标签 1', '标签 2']
categories = ['分类名称']
author = '作者名'
description = '文章描述'
+++

文章内容...
```

## 🎨 主题配置

博客使用 PaperMod 主题，主要配置在 `hugo.toml` 中：

- **菜单设置**: 首页、文章、标签、分类
- **显示选项**: 阅读时间、字数统计、代码复制按钮等
- **社交链接**: GitHub、Twitter、RSS
- **搜索功能**: 内置 Fuse.js 搜索
- **评论系统**: 支持 Giscus（需配置）

详细配置请参考 [PaperMod 官方文档](https://adityatelange.github.io/hugo-PaperMod/)。

## 🌐 部署

博客通过 GitHub Actions 自动部署到 GitHub Pages：

1. 推送代码到 `main` 分支
2. GitHub Actions 自动构建
3. 部署到 GitHub Pages

访问地址：https://luoxiaobin001.github.io/blog/

## 📊 内容分类

- **技术文章**: 编程语言、开发工具、架构设计
- **读书笔记**: 技术书籍、人文社科类书籍
- **生活思考**: 社会观察、文化反思
- **前沿探索**: AI、量子计算等新技术

## 🛠️ 技术栈

- **静态网站生成器**: Hugo
- **主题**: PaperMod
- **托管平台**: GitHub Pages
- **CI/CD**: GitHub Actions
- **语言**: Markdown, TOML

## 📄 许可证

MIT License

## 🤝 贡献

欢迎通过 Issue 或 Pull Request 提供反馈和建议！

## 📬 联系方式

- GitHub: [@luoxiaobin001](https://github.com/luoxiaobin001)
- Email: [你的邮箱]

---

Made with ❤️ using Hugo and PaperMod

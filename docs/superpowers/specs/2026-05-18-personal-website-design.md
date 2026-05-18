# 个人 AI 学习网页 — 设计文档

## 概述

搭建一个个人网站，用于分享 AI 学习心得、实验记录和技术思考。网站由 Claude Code 协助维护，内容包括 Markdown 编写的文章，通过 Astro 构建为静态站点，部署到 Cloudflare Pages。

## 技术栈

- **框架**: Astro（静态站点生成）
- **样式**: 全局 CSS（无 UI 框架依赖）
- **评论**: Waline（LeanCloud 后端，支持微信/GitHub/QQ/手机号登录）
- **代码高亮**: Shiki（Astro 内置）
- **部署**: Cloudflare Pages（Git 推送自动部署）
- **内容**: Markdown 文件（`src/content/posts/`）

## 架构

```
个人网页部署/
├── src/
│   ├── pages/
│   │   ├── index.astro          # 首页 — 文章列表 + Hero
│   │   ├── posts/[slug].astro   # 文章详情 + Waline 评论区
│   │   ├── tags/[tag].astro     # 标签筛选页
│   │   └── about.astro          # 关于我
│   ├── content/posts/           # Markdown 文章
│   ├── components/
│   │   ├── Header.astro
│   │   ├── PostCard.astro
│   │   ├── TagBadge.astro
│   │   ├── Hero.astro
│   │   ├── CommentSection.astro
│   │   └── Footer.astro
│   ├── layouts/BaseLayout.astro
│   └── styles/global.css
├── public/                      # favicon, 图片等
├── astro.config.mjs
├── package.json
└── .gitignore
```

## 路由

| 路径 | 页面 | 说明 |
|---|---|---|
| `/` | 首页 | Hero + 文章卡片列表，按时间倒序 |
| `/posts/[slug]` | 文章详情 | Markdown 渲染 + Waline 评论区 |
| `/tags/[tag]` | 标签页 | 特定标签的文章列表 |
| `/about` | 关于 | 个人信息和网站介绍 |

## 视觉设计

- **主色调**: 米色暖调 — 底色 `#fefcf5`，卡片白底 `#fffdf7` 搭配米色边框 `#efe4cf`
- **文字色**: 标题 `#4a3724`，正文 `#5c4a2e`，辅助文字 `#8b7355` / `#9b8c7a`
- **强调色**: 金色 `#8b6914`，标签背景 `#f5efe0`
- **排版**: 内容区最大宽度 640px，居中显示，阅读舒适
- **代码块**: 浅米色背景 `#faf6ed`，边框 `#efe4cf`

## 页面组件

### 首页
- **Header**: Logo + 导航（首页 / 标签 / 关于）
- **Hero**: 居中标题"探索 AI 的旅程" + 副标题
- **文章卡片列表**: 每张卡片包含标签徽章、标题、摘要、日期

### 文章详情
- **Header**: 同上，加返回按钮
- **文章内容**: Markdown 渲染，代码块 Shiki 高亮
- **评论区**: Waline 组件，支持微信/GitHub/QQ/手机号登录

### 标签页
- 按标签筛选的文章列表，复用 PostCard 组件

## 内容管理流程

1. 用户告知文章主题或要点
2. Claude Code 编写 Markdown 文件到 `src/content/posts/`
3. 本地 `astro dev` 预览确认
4. Git commit + push 到 GitHub
5. Cloudflare Pages 自动构建部署

## 部署

- **平台**: Cloudflare Pages
- **构建命令**: `npm run build`
- **输出目录**: `dist/`
- **域名**: 先用 `*.pages.dev`，后续可绑定自定义域名

## 待配置项（部署时完成）

- Cloudflare Pages 连接 GitHub 仓库
- LeanCloud 应用创建（Waline 数据存储）
- Waline 服务端部署（Cloudflare Workers，与 Pages 同一平台）

# 个人 AI 学习网页 — 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 基于 Astro 搭建个人 AI 学习网站，米色暖调主题，Markdown 内容管理，Waline 评论区，部署到 Cloudflare Pages。

**Architecture:** 纯静态站点，Astro 在构建时将 Markdown 内容和 Astro 组件渲染为静态 HTML/CSS。Waline 作为客户端组件嵌入文章详情页。内容通过 Git 推送触发 Cloudflare Pages 自动部署。

**Tech Stack:** Astro, Waline (@waline/client), Shiki (Astro 内置), Cloudflare Pages

---

### Task 1: 项目脚手架

**Files:**
- Create: `package.json`
- Create: `astro.config.mjs`
- Create: `tsconfig.json`
- Create: `.gitignore` (已存在，追加条目)

- [ ] **Step 1: 初始化 npm 项目并安装 Astro**

```bash
cd "/Users/caogong/我的云端硬盘/ai/个人网页部署" && npm create astro@latest . -- --skip-houston --template minimal --install --typescript strict
```

- [ ] **Step 2: 安装 Waline 客户端**

```bash
npm install @waline/client
```

- [ ] **Step 3: 验证项目可构建**

```bash
npm run build
```
Expected: 构建成功，`dist/` 目录生成。

- [ ] **Step 4: 更新 .gitignore**

追加以下内容到 `.gitignore`：
```
dist/
node_modules/
.superpowers/
.DS_Store
```

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "chore: scaffold Astro project with Waline dependency"
```

---

### Task 2: 全局样式 — 米色暖调主题

**Files:**
- Create: `src/styles/global.css`

- [ ] **Step 1: 编写全局 CSS 变量和基础样式**

```css
:root {
  --color-bg: #fefcf5;
  --color-card: #fffdf7;
  --color-border: #efe4cf;
  --color-border-light: #f0e8d8;
  --color-heading: #4a3724;
  --color-text: #5c4a2e;
  --color-muted: #8b7355;
  --color-muted-light: #9b8c7a;
  --color-subtle: #bfae94;
  --color-accent: #8b6914;
  --color-tag-bg: #f5efe0;
  --color-code-bg: #faf6ed;
  --max-width: 640px;
  --font-serif: 'Georgia', 'Noto Serif SC', 'Source Han Serif SC', serif;
  --font-sans: 'Inter', 'Noto Sans SC', 'Source Han Sans SC', -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', 'Source Code Pro', monospace;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 17px;
  line-height: 1.8;
}

body {
  font-family: var(--font-serif);
  background: var(--color-bg);
  color: var(--color-text);
  min-height: 100vh;
}

a {
  color: var(--color-accent);
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

h1, h2, h3, h4, h5, h6 {
  color: var(--color-heading);
  font-family: var(--font-sans);
  line-height: 1.4;
}

h2 { font-size: 1.5rem; margin: 2rem 0 0.75rem; }
h3 { font-size: 1.25rem; margin: 1.5rem 0 0.5rem; }

p { margin-bottom: 1rem; }

pre {
  background: var(--color-code-bg);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 1rem 1.25rem;
  overflow-x: auto;
  margin: 1.25rem 0;
  font-size: 0.875rem;
  line-height: 1.7;
}

code {
  font-family: var(--font-mono);
  font-size: 0.85em;
}

:not(pre) > code {
  background: var(--color-code-bg);
  padding: 0.15em 0.4em;
  border-radius: 4px;
  border: 1px solid var(--color-border-light);
  color: var(--color-accent);
}

blockquote {
  border-left: 3px solid var(--color-accent);
  padding: 0.5rem 0 0.5rem 1.25rem;
  margin: 1.25rem 0;
  color: var(--color-muted);
  background: var(--color-code-bg);
  border-radius: 0 8px 8px 0;
}

img {
  max-width: 100%;
  border-radius: 8px;
}

.container {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 1.25rem;
}
```

- [ ] **Step 2: Commit**

```bash
git add src/styles/global.css
git commit -m "feat: add cream-warm global theme styles"
```

---

### Task 3: BaseLayout 布局组件

**Files:**
- Create: `src/layouts/BaseLayout.astro`

- [ ] **Step 1: 编写 BaseLayout**

```astro
---
import '../styles/global.css';

export interface Props {
  title: string;
  description?: string;
}

const { title, description } = Astro.props;
---

<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description || title} />
    <title>{title}</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Noto+Sans+SC:wght@400;500;600&family=Noto+Serif+SC:wght@400;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
  </head>
  <body>
    <slot />
  </body>
</html>
```

- [ ] **Step 2: Commit**

```bash
git add src/layouts/BaseLayout.astro
git commit -m "feat: add BaseLayout with Google Fonts and metadata"
```

---

### Task 4: Header 导航组件

**Files:**
- Create: `src/components/Header.astro`

- [ ] **Step 1: 编写 Header**

```astro
---
const navItems = [
  { label: '首页', href: '/' },
  { label: '标签', href: '/tags' },
  { label: '关于', href: '/about' },
];

const currentPath = Astro.url.pathname;
---

<header class="header">
  <div class="container header-inner">
    <a href="/" class="logo">🧠 AI 学习笔记</a>
    <nav class="nav">
      {navItems.map(item => (
        <a
          href={item.href}
          class={`nav-link${currentPath === item.href ? ' active' : ''}`}
        >
          {item.label}
        </a>
      ))}
    </nav>
  </div>
</header>

<style>
  .header {
    padding: 1rem 0;
    border-bottom: 1px solid var(--color-border-light);
    background: var(--color-card);
  }
  .header-inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: var(--max-width);
  }
  .logo {
    font-family: var(--font-sans);
    font-weight: 600;
    font-size: 1rem;
    color: var(--color-heading);
    text-decoration: none;
  }
  .logo:hover {
    text-decoration: none;
  }
  .nav {
    display: flex;
    gap: 1.25rem;
  }
  .nav-link {
    font-family: var(--font-sans);
    font-size: 0.85rem;
    color: var(--color-muted);
    text-decoration: none;
    transition: color 0.2s;
  }
  .nav-link:hover,
  .nav-link.active {
    color: var(--color-accent);
    text-decoration: none;
  }
</style>
```

- [ ] **Step 2: Commit**

```bash
git add src/components/Header.astro
git commit -m "feat: add Header navigation component"
```

---

### Task 5: Footer 组件

**Files:**
- Create: `src/components/Footer.astro`

- [ ] **Step 1: 编写 Footer**

```astro
<footer class="footer">
  <div class="container">
    <p>&copy; {new Date().getFullYear()} AI 学习笔记. Powered by Astro & Cloudflare Pages.</p>
  </div>
</footer>

<style>
  .footer {
    padding: 2rem 0;
    margin-top: 4rem;
    border-top: 1px solid var(--color-border-light);
    text-align: center;
  }
  .footer p {
    font-family: var(--font-sans);
    font-size: 0.8rem;
    color: var(--color-subtle);
    margin: 0;
  }
</style>
```

- [ ] **Step 2: Commit**

```bash
git add src/components/Footer.astro
git commit -m "feat: add Footer component"
```

---

### Task 6: Hero 组件

**Files:**
- Create: `src/components/Hero.astro`

- [ ] **Step 1: 编写 Hero**

```astro
---
export interface Props {
  title?: string;
  subtitle?: string;
}

const {
  title = '探索 AI 的旅程',
  subtitle = '记录学习过程中的思考、实验与心得',
} = Astro.props;
---

<section class="hero">
  <div class="container">
    <h1 class="hero-title">{title}</h1>
    <p class="hero-subtitle">{subtitle}</p>
  </div>
</section>

<style>
  .hero {
    padding: 3rem 0 2rem;
    text-align: center;
  }
  .hero-title {
    font-size: 1.75rem;
    font-weight: 600;
    color: var(--color-heading);
    margin: 0 0 0.5rem;
  }
  .hero-subtitle {
    font-family: var(--font-sans);
    font-size: 0.95rem;
    color: var(--color-muted);
    margin: 0;
  }
</style>
```

- [ ] **Step 2: Commit**

```bash
git add src/components/Hero.astro
git commit -m "feat: add Hero component"
```

---

### Task 7: TagBadge 组件

**Files:**
- Create: `src/components/TagBadge.astro`

- [ ] **Step 1: 编写 TagBadge**

```astro
---
export interface Props {
  tag: string;
}

const { tag } = Astro.props;
---

<a href={`/tags/${tag}`} class="tag-badge">#{tag}</a>

<style>
  .tag-badge {
    display: inline-block;
    padding: 0.15em 0.7em;
    background: var(--color-tag-bg);
    border-radius: 12px;
    font-family: var(--font-sans);
    font-size: 0.75rem;
    color: var(--color-accent);
    text-decoration: none;
    transition: background 0.2s;
  }
  .tag-badge:hover {
    background: var(--color-border);
    text-decoration: none;
  }
</style>
```

- [ ] **Step 2: Commit**

```bash
git add src/components/TagBadge.astro
git commit -m "feat: add TagBadge component"
```

---

### Task 8: PostCard 组件

**Files:**
- Create: `src/components/PostCard.astro`
- 依赖: TagBadge (Task 7)

- [ ] **Step 1: 编写 PostCard**

```astro
---
import type { CollectionEntry } from 'astro:content';
import TagBadge from './TagBadge.astro';

export interface Props {
  post: CollectionEntry<'posts'>;
}

const { post } = Astro.props;
const { title, date, tags, description } = post.data;
---

<article class="post-card">
  <div class="tags">
    {tags.map(tag => <TagBadge tag={tag} />)}
  </div>
  <h2 class="post-title">
    <a href={`/posts/${post.slug}`}>{title}</a>
  </h2>
  <p class="post-desc">{description}</p>
  <time class="post-date" datetime={date.toISOString()}>
    {date.toLocaleDateString('zh-CN', { year: 'numeric', month: 'long', day: 'numeric' })}
  </time>
</article>

<style>
  .post-card {
    background: var(--color-card);
    border: 1px solid var(--color-border);
    border-radius: 10px;
    padding: 1.5rem;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  }
  .tags {
    display: flex;
    gap: 0.4rem;
    flex-wrap: wrap;
    margin-bottom: 0.75rem;
  }
  .post-title {
    font-size: 1.15rem;
    margin: 0 0 0.35rem;
  }
  .post-title a {
    color: var(--color-heading);
    font-weight: 500;
    text-decoration: none;
  }
  .post-title a:hover {
    color: var(--color-accent);
    text-decoration: none;
  }
  .post-desc {
    font-size: 0.85rem;
    color: var(--color-muted-light);
    margin: 0 0 0.75rem;
  }
  .post-date {
    font-family: var(--font-sans);
    font-size: 0.75rem;
    color: var(--color-subtle);
  }
</style>
```

- [ ] **Step 2: Commit**

```bash
git add src/components/PostCard.astro
git commit -m "feat: add PostCard component"
```

---

### Task 9: 内容集合配置

**Files:**
- Create: `src/content/config.ts`
- Create: `src/content/posts/` 目录（示例文章在 Task 10）

- [ ] **Step 1: 编写内容集合 schema**

```typescript
import { defineCollection, z } from 'astro:content';

const postsCollection = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    date: z.date(),
    tags: z.array(z.string()).default([]),
    description: z.string(),
  }),
});

export const collections = {
  posts: postsCollection,
};
```

- [ ] **Step 2: Commit**

```bash
git add src/content/config.ts
git commit -m "feat: define posts content collection schema"
```

---

### Task 10: 示例文章

**Files:**
- Create: `src/content/posts/hello-world.md`

- [ ] **Step 1: 编写示例文章**

```markdown
---
title: "你好，AI 世界"
date: 2026-05-18
tags: ["随笔"]
description: "这是我的第一篇 AI 学习笔记，聊聊为什么开始这个博客。"
---

## 为什么开始写 AI 学习笔记？

人工智能正在以前所未有的速度改变世界。作为这个时代的见证者和参与者，我希望能记录下自己的学习历程。

### 写什么？

- **技术教程**：从零开始拆解 AI 概念
- **实验记录**：跑模型、调参数的实战心得
- **思考随笔**：关于 AI 发展的观察和想法

### 怎么做？

持续学习，持续记录。不追求完美，追求成长。

> 种一棵树最好的时间是十年前，其次是现在。

欢迎你和我一起探索 AI 的世界。
```

- [ ] **Step 2: 验证构建**

```bash
npm run build
```
Expected: 构建成功，无错误。

- [ ] **Step 3: Commit**

```bash
git add src/content/posts/hello-world.md
git commit -m "feat: add hello-world sample post"
```

---

### Task 11: 首页

**Files:**
- 替换: `src/pages/index.astro`
- 依赖: BaseLayout (Task 3), Header (Task 4), Footer (Task 5), Hero (Task 6), PostCard (Task 8)

- [ ] **Step 1: 编写首页**

```astro
---
import { getCollection } from 'astro:content';
import BaseLayout from '../layouts/BaseLayout.astro';
import Header from '../components/Header.astro';
import Hero from '../components/Hero.astro';
import PostCard from '../components/PostCard.astro';
import Footer from '../components/Footer.astro';

const posts = await getCollection('posts', ({ data }) => {
  return data.date <= new Date();
});

posts.sort((a, b) => b.data.date.getTime() - a.data.date.getTime());
---

<BaseLayout title="AI 学习笔记" description="记录 AI 学习过程中的思考、实验与心得">
  <Header />
  <Hero />
  <main class="container post-list">
    {posts.map(post => <PostCard post={post} />)}
    {posts.length === 0 && (
      <p class="empty">还没有文章，敬请期待。</p>
    )}
  </main>
  <Footer />
</BaseLayout>

<style>
  .post-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    padding-bottom: 2rem;
  }
  .empty {
    text-align: center;
    color: var(--color-muted-light);
    font-family: var(--font-sans);
    padding: 3rem 0;
  }
</style>
```

- [ ] **Step 2: 构建验证**

```bash
npm run build
```
Expected: 构建成功。检查 `dist/index.html` 存在且包含文章卡片。

- [ ] **Step 3: Commit**

```bash
git add src/pages/index.astro
git commit -m "feat: add homepage with post list"
```

---

### Task 12: 文章详情页

**Files:**
- Create: `src/pages/posts/[slug].astro`
- 依赖: BaseLayout, Header, Footer, TagBadge

- [ ] **Step 1: 编写文章详情页**

```astro
---
import { getCollection } from 'astro:content';
import BaseLayout from '../../layouts/BaseLayout.astro';
import Header from '../../components/Header.astro';
import Footer from '../../components/Footer.astro';
import TagBadge from '../../components/TagBadge.astro';
import CommentSection from '../../components/CommentSection.astro';

export async function getStaticPaths() {
  const posts = await getCollection('posts');
  return posts.map(post => ({
    params: { slug: post.slug },
    props: { post },
  }));
}

const { post } = Astro.props;
const { title, date, tags } = post.data;
const { Content } = await post.render();
---

<BaseLayout title={`${title} - AI 学习笔记`} description={post.data.description}>
  <Header />
  <article class="container article">
    <header class="article-header">
      <div class="article-tags">
        {tags.map(tag => <TagBadge tag={tag} />)}
      </div>
      <h1>{title}</h1>
      <div class="article-meta">
        <time datetime={date.toISOString()}>
          {date.toLocaleDateString('zh-CN', { year: 'numeric', month: 'long', day: 'numeric' })}
        </time>
        <span class="reading-time">约 {Math.ceil(post.body.split(/\s+/).length / 300)} 分钟阅读</span>
      </div>
    </header>
    <div class="article-content">
      <Content />
    </div>
    <CommentSection client:load />
    <footer class="article-back">
      <a href="/">← 返回首页</a>
    </footer>
  </article>
  <Footer />
</BaseLayout>

<style>
  .article {
    padding-top: 2.5rem;
    padding-bottom: 2rem;
  }
  .article-header {
    margin-bottom: 2rem;
    padding-bottom: 1.5rem;
    border-bottom: 1px solid var(--color-border-light);
  }
  .article-tags {
    display: flex;
    gap: 0.4rem;
    flex-wrap: wrap;
    margin-bottom: 0.75rem;
  }
  .article-header h1 {
    font-size: 1.6rem;
    margin: 0 0 0.5rem;
  }
  .article-meta {
    display: flex;
    gap: 1rem;
    font-family: var(--font-sans);
    font-size: 0.8rem;
    color: var(--color-subtle);
  }
  .reading-time::before {
    content: '·';
    margin-right: 0.5rem;
  }
  .article-content {
    margin-bottom: 3rem;
  }
  .article-back {
    margin-top: 2rem;
    padding-top: 1.5rem;
    border-top: 1px solid var(--color-border-light);
    font-family: var(--font-sans);
    font-size: 0.9rem;
  }
</style>
```

- [ ] **Step 2: 构建验证**

```bash
npm run build
```
Expected: 构建成功。检查 `dist/posts/hello-world/index.html` 存在。

- [ ] **Step 3: 开发服务器预览**

```bash
npx astro dev --port 4321 &
sleep 3
```
访问 `http://localhost:4321/posts/hello-world` 确认页面正常渲染。

- [ ] **Step 4: Commit**

```bash
git add src/pages/posts/
git commit -m "feat: add article detail page with content rendering"
```

---

### Task 13: Waline 评论区组件

**Files:**
- Create: `src/components/CommentSection.astro`

说明：Waline 需 LeanCloud 配置才能真实运行。此任务创建组件框架，部署前填入实际 serverURL。

- [ ] **Step 1: 编写 CommentSection**

```astro
---
// Waline 服务端地址 — 部署到 Cloudflare Workers 后获得
// 占位值，部署时替换为实际地址
const walineServerURL = import.meta.env.PUBLIC_WALINE_SERVER_URL || 'https://your-waline.workers.dev';
---

<div id="waline-comments" class="comments"></div>

<style>
  .comments {
    margin-top: 3rem;
    padding-top: 2rem;
    border-top: 1px solid var(--color-border-light);
  }
</style>

<script>
  import { init } from '@waline/client';

  import '@waline/client/style';

  const walineServerURL = import.meta.env.PUBLIC_WALINE_SERVER_URL;

  if (walineServerURL && walineServerURL !== 'https://your-waline.workers.dev') {
    init({
      el: '#waline-comments',
      serverURL: walineServerURL,
      lang: 'zh-CN',
      login: 'enable',
      search: false,
      pageSize: 10,
      dark: false,
      locale: {
        placeholder: '写下你的想法…',
        sofa: '还没有评论，来抢沙发吧',
      },
    });
  } else {
    document.getElementById('waline-comments')!.innerHTML =
      '<p style="text-align:center;color:var(--color-muted-light);font-family:var(--font-sans);font-size:0.85rem">评论区即将上线</p>';
  }
</script>
```

- [ ] **Step 2: 创建环境变量模板**

创建 `src/env.d.ts`（如未自动生成）：
```typescript
/// <reference path="../.astro/types.d.ts" />
/// <reference types="astro/client" />

interface ImportMetaEnv {
  readonly PUBLIC_WALINE_SERVER_URL: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

- [ ] **Step 3: Commit**

```bash
git add src/components/CommentSection.astro src/env.d.ts
git commit -m "feat: add Waline comment section component"
```

---

### Task 14: 标签筛选页

**Files:**
- Create: `src/pages/tags/index.astro`
- Create: `src/pages/tags/[tag].astro`
- 依赖: BaseLayout, Header, Footer, PostCard

- [ ] **Step 1: 编写标签总览页**

```astro
---
import { getCollection } from 'astro:content';
import BaseLayout from '../../layouts/BaseLayout.astro';
import Header from '../../components/Header.astro';
import Footer from '../../components/Footer.astro';

const posts = await getCollection('posts');
const tagMap = new Map<string, number>();

for (const post of posts) {
  for (const tag of post.data.tags) {
    tagMap.set(tag, (tagMap.get(tag) || 0) + 1);
  }
}

const allTags = [...tagMap.entries()]
  .sort((a, b) => b[1] - a[1]);
---

<BaseLayout title="标签 - AI 学习笔记">
  <Header />
  <main class="container">
    <h1 class="page-title">标签</h1>
    <div class="tag-cloud">
      {allTags.map(([tag, count]) => (
        <a href={`/tags/${tag}`} class="tag-item">
          #{tag}
          <span class="tag-count">{count}</span>
        </a>
      ))}
    </div>
    {allTags.length === 0 && (
      <p class="empty">还没有标签。</p>
    )}
  </main>
  <Footer />
</BaseLayout>

<style>
  .page-title {
    font-size: 1.5rem;
    margin: 2.5rem 0 1.5rem;
    text-align: center;
  }
  .tag-cloud {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
    justify-content: center;
  }
  .tag-item {
    display: inline-flex;
    align-items: center;
    gap: 0.3rem;
    padding: 0.4em 1em;
    background: var(--color-tag-bg);
    border-radius: 16px;
    font-family: var(--font-sans);
    font-size: 0.9rem;
    color: var(--color-accent);
    text-decoration: none;
    transition: background 0.2s;
  }
  .tag-item:hover {
    background: var(--color-border);
    text-decoration: none;
  }
  .tag-count {
    font-size: 0.75rem;
    color: var(--color-muted-light);
  }
  .empty {
    text-align: center;
    color: var(--color-muted-light);
    font-family: var(--font-sans);
    padding: 3rem 0;
  }
</style>
```

- [ ] **Step 2: 编写特定标签页**

```astro
---
import { getCollection } from 'astro:content';
import BaseLayout from '../../layouts/BaseLayout.astro';
import Header from '../../components/Header.astro';
import Footer from '../../components/Footer.astro';
import PostCard from '../../components/PostCard.astro';

export async function getStaticPaths() {
  const posts = await getCollection('posts');
  const tagSet = new Set<string>();
  for (const post of posts) {
    for (const tag of post.data.tags) {
      tagSet.add(tag);
    }
  }
  return [...tagSet].map(tag => ({ params: { tag } }));
}

const { tag } = Astro.params;

const allPosts = await getCollection('posts');
const posts = allPosts
  .filter(post => post.data.tags.includes(tag!))
  .sort((a, b) => b.data.date.getTime() - a.data.date.getTime());
---

<BaseLayout title={`#${tag} - AI 学习笔记`}>
  <Header />
  <main class="container">
    <h1 class="page-title">#{tag}</h1>
    <p class="page-subtitle">{posts.length} 篇文章</p>
    <div class="post-list">
      {posts.map(post => <PostCard post={post} />)}
    </div>
    <footer class="back-link">
      <a href="/tags">← 查看所有标签</a>
    </footer>
  </main>
  <Footer />
</BaseLayout>

<style>
  .page-title {
    font-size: 1.5rem;
    margin: 2.5rem 0 0.25rem;
    text-align: center;
  }
  .page-subtitle {
    text-align: center;
    font-family: var(--font-sans);
    font-size: 0.85rem;
    color: var(--color-muted-light);
    margin: 0 0 2rem;
  }
  .post-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .back-link {
    margin-top: 2rem;
    text-align: center;
    font-family: var(--font-sans);
    font-size: 0.85rem;
  }
</style>
```

- [ ] **Step 3: 构建验证**

```bash
npm run build
```
Expected: 构建成功。检查 `dist/tags/index.html` 和 `dist/tags/随笔/index.html` 存在。

- [ ] **Step 4: Commit**

```bash
git add src/pages/tags/
git commit -m "feat: add tag overview and tag filter pages"
```

---

### Task 15: 关于页面

**Files:**
- Create: `src/pages/about.astro`
- 依赖: BaseLayout, Header, Footer

- [ ] **Step 1: 编写关于页面**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
---

<BaseLayout title="关于 - AI 学习笔记">
  <Header />
  <main class="container about">
    <h1>关于我</h1>
    <div class="about-content">
      <p>
        这里是 AI 学习笔记，一个记录我在人工智能领域学习历程的个人空间。
      </p>
      <p>
        我会在这里分享：
      </p>
      <ul>
        <li>学习 AI 技术的心得体会</li>
        <li>实践项目和实验记录</li>
        <li>对行业发展的观察与思考</li>
      </ul>
      <p>
        如果你对 AI 也感兴趣，欢迎在文章评论区交流。
      </p>
    </div>
  </main>
  <Footer />
</BaseLayout>

<style>
  .about {
    padding-top: 2.5rem;
    max-width: var(--max-width);
  }
  .about h1 {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    text-align: center;
  }
  .about-content {
    font-size: 0.95rem;
    line-height: 1.9;
  }
  .about-content ul {
    padding-left: 1.5rem;
    margin-bottom: 1rem;
  }
  .about-content li {
    margin-bottom: 0.25rem;
  }
</style>
```

- [ ] **Step 2: 构建验证**

```bash
npm run build
```
Expected: 构建成功。`dist/about/index.html` 存在。

- [ ] **Step 3: Commit**

```bash
git add src/pages/about.astro
git commit -m "feat: add about page"
```

---

### Task 16: Astro 配置和最终构建

**Files:**
- Modify: `astro.config.mjs`

- [ ] **Step 1: 确认 astro.config.mjs 配置正确**

```javascript
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://your-site.pages.dev',
  output: 'static',
  markdown: {
    shikiConfig: {
      theme: 'github-light',
      wrap: true,
    },
  },
});
```

- [ ] **Step 2: 完整构建验证**

```bash
npm run build
```
Expected: 构建成功，无任何错误或警告。所有页面生成完毕。

- [ ] **Step 3: Commit**

```bash
git add astro.config.mjs
git commit -m "chore: finalize astro config for production build"
```

---

### Task 17: Git 初始化与 GitHub 推送

**前置条件:** 用户在 GitHub 上创建了仓库 `https://github.com/<username>/<repo>.git`

- [ ] **Step 1: 初始化 Git（如尚未初始化）**

```bash
cd "/Users/caogong/我的云端硬盘/ai/个人网页部署"
git init
git checkout -b main
```

- [ ] **Step 2: 添加远程仓库并推送**

```bash
# 替换 <username> 和 <repo> 为实际值
git remote add origin https://github.com/<username>/<repo>.git
git add -A
git commit -m "feat: complete personal AI learning blog — Astro + Waline"
git push -u origin main
```
Expected: 推送成功。如果用户尚未创建 GitHub 仓库，先通过 `gh repo create` 创建。

- [ ] **Step 3: Commit**

本次推送即为最终提交，无需额外 commit。

---

### Task 18: Cloudflare Pages 部署

**前置条件:** GitHub 仓库已推送，用户有 Cloudflare 账号。

- [ ] **Step 1: 在 Cloudflare Dashboard 配置 Pages**

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 Workers & Pages → Pages → Connect to Git
3. 选择 GitHub 仓库
4. 构建设置：
   - Framework preset: **Astro**
   - Build command: `npm run build`
   - Build output directory: `dist`
5. 点击 Save and Deploy

- [ ] **Step 2: 验证部署**

部署完成后，访问 `https://<project>.pages.dev` 确认网站正常显示。

- [ ] **Step 3: 配置环境变量（Waline）**

在 Cloudflare Pages → Settings → Environment variables 中添加：
- `PUBLIC_WALINE_SERVER_URL` = Waline Cloudflare Workers 地址（需先按 Task 13 说明部署 Waline 服务端）

---

### 后续任务（不在本次计划范围内）

- Waline 服务端部署（需先注册 LeanCloud + 部署到 Cloudflare Workers）
- 自定义域名配置
- 后续文章由 Claude Code 维护添加

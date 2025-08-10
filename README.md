# Blog – Hugh’s Notes

> Live site: **https://blog.indiecraft.app**
>  中文入口：**https://blog.indiecraft.app/zh/**

Personal tech blog powered by **Hugo (extended) + PaperMod**, deployed on **Netlify**.
 多语言（English/中文）、Markdown 写作体验，带 **Pagefind 搜索**、**Giscus 评论**、**Mermaid 流程图**、**RSS**、以及基础 **SEO**（canonical、hreflang、JSON-LD）。

------

## Tech Stack / 技术栈

- **Static site generator**: Hugo (extended)
- **Theme**: [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
- **Deploy**: Netlify
- **Search**: [Pagefind](https://pagefind.app/)
- **Comments**: [Giscus](https://giscus.app/)
- **Diagrams**: Mermaid (````mermaid` fenced code)
- **Analytics**: (optional) Plausible / GA4
- **i18n**: `/en`, `/zh`

------

## Quick Start / 快速开始

### Requirements

- Hugo extended（建议与生产一致）
- Node.js ≥ 18（用于 Pagefind 构建）

### Install & Run / 安装与预览

```
bash


复制编辑
# clone
git clone https://github.com/HughLee824/blog.git
cd blog

# install pagefind (devDependency)
npm i

# local preview
hugo server -D --disableFastRender
# http://localhost:1313
```

### Build / 构建

```
bash


复制编辑
npm run build
# 等价于: hugo --gc --minify && pagefind --site public
```

> 生产（Netlify）构建命令：`npm ci && npm run build`
>  构建产物输出目录：`public/`

------

## Content Structure / 内容结构

```
bash


复制编辑
content/
├─ en/
│  └─ posts/
│     └─ <slug>/
│        ├─ index.md     # Page Bundle (推荐): 正文
│        └─ cover.jpg    # 同目录资源
└─ zh/
   └─ posts/
      └─ <slug>/
         ├─ index.md
         └─ cover.jpg
```

- **Page Bundle**：图片/附件与 `index.md` 同目录，Markdown 中相对路径引用。
- **多语言**：中英同一篇建议 **相同 `slug`**，Hugo 会识别为互为翻译。

------

## Writing Posts / 写作

- 直接创建或复制到对应目录；或使用 archetype：

  ```
  bash
  
  
  复制编辑
  hugo new en/posts/my-post/index.md
  hugo new zh/posts/my-post/index.md
  ```

- Front matter 最小示例：

  ```
  yaml
  
  
  复制编辑
  title: "Swift 6 Migration Pitfalls You Should Know"
  description: "Real-world pitfalls, isolation traps, and mitigation strategies."
  slug: "swift-6-migration-pitfalls"
  date: 2025-08-10T10:00:00-07:00
  draft: false
  images: ["/images/swift6-pitfalls-og.png"]  # 可选: OG 图
  ```

------

## Search (Pagefind)

- 搜索页：`/en/search/`、`/zh/search/`
- 构建：`npm run build` 会在 `public/pagefind/` 生成索引
- 本地预览搜索：`hugo server --renderToDisk` 或先 `npm run build` 再开本地服务

------

## Comments (Giscus)

`config.toml`（用 giscus 向导获取 `repoID`/`categoryID`）：

```
toml


复制编辑
[params.giscus]
  repo = "HughLee824/blog-comments"
  repoID = "R_kgDxxxxxxxx"
  category = "General"
  categoryID = "DIC_kwDxxxxxxxx"
  mapping = "pathname"
  reactionsEnabled = "1"
  emitMetadata = "0"
  inputPosition = "bottom"
  theme = "preferred_color_scheme"
  lang = "en"
```

模板：`layouts/partials/comments.html`（主题会自动 include）。
 **首次使用**：作者登录页面评论区 → Sign in → 创建对应 Discussion。

------

## Mermaid Diagrams / Mermaid 流程图

- `layouts/_markup/render-codeblock-mermaid.html` 将 ````mermaid` 渲染为 `<div class="mermaid">`

- `layouts/partials/extend_footer.html` 按需注入脚本（仅本页含 mermaid 时）：

  ```
  html
  
  
  复制编辑
  {{ if .Page.Store.Get "hasMermaid" }}
    <script type="module">
      import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
      mermaid.initialize({ startOnLoad: false, theme: 'default' });
      mermaid.run({ querySelector: '.mermaid' });
    </script>
  {{ end }}
  ```

- Markdown 示例：

  ````
  md
  
  
  复制编辑
  ```mermaid
  flowchart TD
    A[Start] --> B{Working?}
    B -- Yes --> C[Ship it]
    B -- No  --> D[Debug]
  ```
  ````

------

## SEO & i18n

- `enableRobotsTXT = true`、`enableGitInfo = true`
- 主页输出：`["HTML","RSS","JSON"]`
- **hreflang**：`extend_head.html` 输出 `/en` 与 `/zh` 的互链
- **canonical**：自指；可用 `canonicalURL` 覆盖（用于转载场景）
- **JSON-LD**：Article + Breadcrumb（均在 `extend_head.html`）
- **Sitemap / RSS**：`/sitemap.xml`、`/en/index.xml`、`/zh/index.xml`
- **Search Console**：上线后在 GSC 提交 sitemap，并对新文 “Request indexing”

------

## Repost & License / 转载与许可

- Shortcode：`layouts/shortcodes/reprint.html`

- 样式：`assets/css/extended/reprint.css`

- 文章末尾加入：`{{< reprint >}}`（已在 `archetypes/posts.md` 默认包含）

- 站点默认（`config.toml`）：

  ```
  toml
  
  
  复制编辑
  [params]
    author = "Hugh"
    license = "CC BY-NC-SA 4.0"
    license_url = "https://creativecommons.org/licenses/by-nc-sa/4.0/"
  ```

------

## Netlify

- 构建命令：`npm ci && npm run build`
- 发布目录：`public`
- 关键头部（详见 `netlify.toml`）：
  - **CSP**：放行 `giscus.app`、`cdn.jsdelivr.net`、`pagefind.app`
  - **Cache-Control**：HTML 短缓存；静态资源一年期 `immutable`
  - **Redirects**：根路径按语言 302 → `/zh/`，否则 301 → `/en/`

------

## License

- **Content（文章）**: CC BY-NC-SA 4.0（除非另有说明）
- **Code（本仓库配置/脚本）**: MIT

------

## Acknowledgements

- PaperMod theme
- Hugo & community
- Pagefind, Giscus, Mermaid


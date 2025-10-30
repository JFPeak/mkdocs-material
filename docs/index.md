#  记录Work，记录Study，记录Life

本博客基于 [**Material for MkDocs**](https://squidfunk.github.io/mkdocs-material/) 进行部署在github。写博客不是为了别人的掌声，而是因为自己的需要，通过写博客，将零散的知识进行梳理和总结，方便日后回顾和学习；将美好的事物记录下来，避免遗忘。
## 选择 MkDocs的原因
最终决定使用 [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)，核心原因可以概括为以下：
1. **Python 驱动，静态站点**  
   基于 Python 的静态站点生成器，开箱即用，能把 Markdown 变成专业级技术文档站点。

2. **Markdown 友好，零前端门槛**  
   纯 Markdown 写作即可，无需懂 HTML/CSS/JS；配合 Python-Markdown 扩展，支持提示块、页签、公式、任务列表等高级元素。

3. **全文搜索，毫秒级响应**  
   内置离线搜索，自动生成索引，无需后端即可实现全文检索。

4. **高扩展，深度可定制**  
   丰富的官方与社区插件生态，配合 YAML 配置即可随意开关；主题色、字体、图标、布局均可通过参数调整。

5. **一键部署，多端托管**  
   生成纯静态文件，GitHub Pages、Netlify、Cloudflare、阿里云 OSS 等平台即可上线使用。

## MkDocs-Material和MkDocs的关系
  MkDocs（Markdown Documents）见名知意就是为 Markdown 而生，是一个快速、简单的静态网站生成器，用于将 Markdown 文档组织起来构建成有层次的文档站点。它基于 Python 编写，与其他常见的静态网站生成器相比，无需繁琐的环境配置，所有配置都用只有简单的一个YML配置文件管理。
  MkDocs-Material（正确名称为 Material for MkDocs）是一个基于 Google Material Design 设计规范的 MkDocs 主题，用于快速构建美观、响应式、功能丰富的静态文档网站。因此，Material for MkDocs是构建在MkDocs之上的，需要先安装MkDocs才能使用。
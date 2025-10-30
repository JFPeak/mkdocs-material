---
typora-copy-images-to: ./image
---

# 【教程一】Mkdocs-material部署博客至GitHub pages

## 一、参考资料
1. https://www.youtube.com/watch?v=xlABhbnNrfI
2. https://blog.csdn.net/2403_88426397/article/details/145471125
3. http://www.cuishuaiwen.com:8000/zh/PROJECT/TECH-BLOG/mkdocs_and_material/#_7
4. https://www.cnblogs.com/chinjinyu/p/17610438.html

## 二、本地安装部署
Windows系统预先从官网安装好[Python3](https://www.python.org/downloads/windows/)和[Git](https://git-scm.com/install/windows)，还有[Vscode](https://code.visualstudio.com/download)。
以管理员身份打开vscode软件，在**合适的文件夹**内打开终端，在终端中输入：

```shell
# 转入虚拟环境
\venv\Scripts\activate

# 安装mkdocs-material（执行一次即可）
pip install mkdocs-material

# 创建新站点（执行一次即可）
mkdocs new .

# 本地启动服务
mkdocs serve
# 即可通过http://localhost:8000 访问
```
## 三、进一步配置
vscode安装YamL by Red Hat 扩展插件，用以防止yaml格式错误导致的bug以及查看各key可对应的value等。

VS Code左下角⚙️->设置->右上角“打开设置(json)”。编辑settings.json，加入以下：

```json
{
...
    "redhat.telemetry.enabled": true,
  "yaml.schemas": {
    "https://squidfunk.github.io/mkdocs-material/schema.json": "mkdocs.yml"
  },
  "yaml.customTags": [
    "!ENV scalar",
    "!ENV sequence",
    "!relative scalar",
    "tag:yaml.org,2002:python/name:material.extensions.emoji.to_svg",
    "tag:yaml.org,2002:python/name:material.extensions.emoji.twemoji",
    "tag:yaml.org,2002:python/name:pymdownx.superfences.fence_code_format"
  ]
}
```
## 四、配置Material主题
即修改`·~/mkdocs.yml`文件的内容。
这个部分应该是花费时间和心思最多的部分，因为Material主题提供了非常多的配置选项，我们可以根据自己的需求进行配置。

```yml
site_name: JF's DigitalDocs

theme:
  name: material
  languages: zh
  features: # 自定义功能
    #- content.action.edit # 编辑按钮，似乎没啥用
    #- content.action.view # 查看按钮，似乎没啥用
    - content.code.annotate  # 代码注释
    - content.code.copy  # 复制代码按钮
    #- content.tooltips
    #- navigation.tabs # 顶级索引被作为tab
    - navigation.sections # 导航栏的section
    - navigation.footer # 底部导航栏
    - navigation.indexes # 索引按钮可以直接触发文件，而不是只能点击其下属选项浏览，这个功能可以给对应的section提供很好的预览和导航功能
    - navigation.top # 开启顶部导航栏
    - navigation.tracking # 导航栏跟踪
    - search.highlight # 搜索高亮
    - search.share # 搜索分享
    - search.suggest # 搜索建议
    - toc.follow # 目录跟踪-页面右侧的页面内容目录

  palette:  # 主题颜色方案
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: teal
      accent: deep orange
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: black
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
  font:  # 字体
    text: "'Open Sans', 'Helvetica Neue', Arial, 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif"
    code: "Consolas, Courier, courier new, stkaiti, kaiti, simkai, monospace"
  

markdown_extensions:  # markdown语法扩展
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - admonition
  - pymdownx.arithmatex:
      generic: true
  - footnotes
  - pymdownx.details
  - pymdownx.superfences
  - pymdownx.mark
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - md_in_html
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format

copyright: |
  &copy; 2023 ~ now | 🚀 Reinforce Future by <a href="https://github.com/JFPeak"  target="_blank" rel="noopener">JF_Peak</a>
```
##  五、使用Git & Github 并部署至github page
### （一）创建GitHub Actions 的自动化脚本
在当前工作目录下创建`.github/workflows/ci.yml` 文件并粘贴示例代码：
```yml
name: ci
on:  # 当有人往这两个分支推送代码时自动运行
  push:
    branches:
      - master
      - main
permissions:  # 给 workflow 开“写”权限，后面才能把生成的站点推回 gh-pages 分支
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force  # 关键一步
      # 调用 MkDocs 内置的 gh-deploy 命令，把 site/ 目录生成的静态文件强行推送到同名仓库的 gh-pages 分支。
```
解释：只要把代码 push 到 main（或 master）分支，该脚本就会自动把 MkDocs 生成的静态网站部署到 GitHub Pages，完全不用手动操作。GitHub Pages 默认读取 gh-pages 分支，于是你的网站瞬间上线，地址通常是`https://<github用户名>.github.io/<仓库名>/`
### （二）创建GitHub仓库和上传ssh公钥

1、创建 GitHub 仓库（网页操作）

- 登录 [https://github.com](https://github.com/)

- 右上角「＋」→ New repository

- 填 3 个空即可
   Repository name: `mkdocs-material` （随便起）
   Description:   可选
   Public / Private: 自选
   **Initialize 三行全部空着**（不要勾选 README、.gitignore、License）→ Create repository

创建完页面会给出两行快速指引，先别关，待会儿用。
2、本机生成 SSH 密钥

```
ssh-keygen -t ed25519 -C "you@example.com"
```
一路回车，默认保存到~/.ssh/id_ed25519（私钥）和 id_ed25519.pub（公钥）
3、把公钥上传到 GitHub
GitHub 头像 → Settings → SSH and GPG keys → New SSH key
Title：随便写，如 Laptop-2025
Key：粘贴刚才创建的公钥内容 → Add SSH key

### （三）⭐️通过git命令push上github
依次执行以下命令。main如果不行，可以换成master，因为Git 2.28 之前默认用 master，现在推荐 main。
```shell
git init
# 把当前目录变成 Git 仓库，生成 .git 隐藏文件夹。（执行一次即可）

git add .
# 把当前目录下所有文件放进“暂存区”，准备提交

git commit -m "first commit"
# 把暂存区的内容正式写进本地仓库的历史记录。

git remote add origin git@github.com:JFPeak/mkdocs-material.git
# 把“本地仓库”与“GitHub 上的远程仓库”建立一条名叫 origin 的关联。（执行一次即可，后续只需 push/pull）

git branch -M main
# 把当前分支强制改名/创建为 main，并切换过去。（执行一次即可）

git push -u origin main
# 把本地 main 分支的提交推送到 GitHub，同时把上游跟踪关系设好后，以后直接 git push 即可
```
执行结束后，查看GitHub仓库，发现多了一个 gh-pages 分支，这个分支里存放的就是 site 文件夹中的内容。

最后一步，配置GitHub Pages的构建和部署分支：将 仓库Settings -> Pages -> Build and deployment -> Branch 设置为 gh-pages/(root)，点击 Save 保存设置。

![image-20251030105915403](.\image\image-20251030105915403.png)

⚠️注意：

- （只做一次）新仓库/新客户端的操作：`git init`、 `git remote add` 、 `git branch -M main`


- 日常发文章循环：git add . → git commit → git push


## 六、后续如何发布文章

MkDocs的文章以markdown文件的形式存储在./docs文件夹下，其中 index.md 为主页。

发布新文章时，在docs目录下新建 `<文章名>.md` 文件并打开编辑内容

然后修改 `mkdocs.yml` 文件，可将新文章加到导航栏中

```
nav:
  - 主页: index.md
  - <标题>: <article>.md
```
标题可以不设置，MkDocs按照： nav配置的标题 > 文章中定义的标题(h1) > 文件名 的顺序进行推断

如果要使用多级导航，则可以这样配置
```
nav:
  - 主页: index.md
  - <分类>:
    - <标题>: <aritle>.md
```
然后我们在编写完文章后，一般至少都要执行这些命令才能发布
```
git add .

git commit -m 'message'

git push
```
## 结语

至此，我们就完成了一个简洁的技术博客的搭建。后续可再对博客配置进行优化。主要是对Material主题。

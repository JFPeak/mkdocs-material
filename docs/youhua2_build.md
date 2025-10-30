# 【教程三】如何将语雀笔记搬到GithubPage上
## 参考资料
1. https://shenwy.com/
2. https://elog.1874.cool/yuque-pwd/start
## 大致思路（未实践）
将语雀笔记搬到GitHub Pages上需要先将语雀笔记导出为Markdown格式，然后使用合适的静态网站生成器（用mkdocs）将Markdown文件转换为HTML页面，最后将生成的文件部署到GitHub Pages。
1. 在语雀中导出笔记
进入你的语雀知识库。
找到并点击“新建文档”按钮旁边的下拉箭头，选择“导入…”。
在弹出的窗口中，选择“Markdown”格式。
选择你想要导出的笔记，点击“开始导入”。
导入完成后，你的笔记将以Markdown文件的形式保存在本地。
2. 使用静态网站生成器构建网站
选择并安装静态网站生成器：根据你的喜好，选择一个生成器，例如Hexo或Jekyll，并按照官方文档进行安装。
创建项目：使用生成器的命令创建一个新的项目。
导入Markdown文件：将你从语雀导出的Markdown文件复制到生成器项目的source/_posts（或相应的位置）文件夹中。
构建网站：运行生成器的构建命令，它会将你的Markdown文件转换成HTML页面。
3. 部署到GitHub Pages
将网站部署到GitHub Pages：按照你所使用的静态网站生成器的部署指南，将构建后的文件部署到你的GitHub Pages仓库。
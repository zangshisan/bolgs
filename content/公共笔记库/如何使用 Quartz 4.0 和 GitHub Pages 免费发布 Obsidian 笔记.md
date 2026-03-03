##### 步骤 0. 先决条件

- 在继续之前，您需要安装以下软件：
```
NodeJS v18.14+ （使用 node -v 检查您的版本）
NPM v9.3.1+（使用 npm -v 检查您的版本）
Git（使用 git --version 检查您的版本）
Obsidian
```
`
> [!info]- 2026/03/02 更新
> nodeb版本 v22+

##### 步骤 1. 下载并安装 Quartz

- 打开终端并运行以下命令
```
git clone https://github.com/jackyzha0/quartz.git  
# 下载 Quartz 存储库的副本，并将其存储在当前文件夹的 quartz 中

cd quartz  
# 进入 quartz 

npm i
# 安装 Quartz 依赖项 

npx quartz create
# 创建一个新的 Quartz 项目
```

-  选择初始化选项
```
# 全部回车默认就好了。
- Empty Quartz
- Copy an existing folder
- Symlink an existing folder
# 初始化目录中的内容
- Treat links as absolute path
- Treat links as shortest path
- Treat links as relative paths
# Obsidian 路径设置
You're all set! Not sure what to try next? Try:
- Customizing Quartz a bit more by editing `quartz.config.ts`
- Running `npx quartz build --serve` to preview your Quartz locally
- Hosting your Quartz online (see: https://quartz.jzhao.xyz/hosting)
# 设置完成

```


##### 步骤 2. 设置 GitHub 存储库

- 在 GitHub.com 上创建一个新存储库。不要使用 README 、许可证或 gitignore 文件初始化新存储库
- 在 GitHub.com 的“快速设置”页面上存储库的顶部，单击剪贴板以复制远程存储库 URL
```
`git remote -v  # 列出所有被跟踪的仓库# origin  https://github.com/jackyzha0/quartz.git (fetch)# origin  https://github.com/jackyzha0/quartz.git (push)# upstream        https://github.com/jackyzha0/quartz.git (fetch)# upstream        https://github.com/jackyzha0/quartz.git (push) 

git remote remove origin  # 此命令删除 origin 远程仓库 

git remote add origin <URL>  # 添加刚复制的个人仓库URL 

npx quartz sync --no-pull  # 将对本地仓库所做的更改推送到远程仓库上的

# 末尾带有绿色 Done!` 
```


##### 步骤 3. 创建 Obsidian 库

- 在 Obsidian 中将 quartz 里的 content 文件夹作为 Obsidian 库打开，然后就可以按照以往习惯写文档
- index.md 是网站首页不要删除（不能删，名称也不能改，要不然会没有样式，显示XML页面）
```
npx quartz sync  # 每次更改后记得更新推送到远程仓库上
```

##### 步骤 4. 在线托管您的保管库

- 在本地 Quartz 中，按以下路径创建一个新文件，并且保存以下内容
- `quartz/.github/workflows/deploy.yml`

```
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v3
        with:
          node-version: 18.14
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2

```
- 前往 GitHub 仓库，点击 Settings,滑动找到Default branch，点击↔将v4设置默认分支；
- 点击右侧菜单Pages>Source 下拉菜单，选择 GitHub Actions
- 最后提交更改，网站将部署到 `<github-username>.github.io/<repository-name>`

> [!info]- 2026/03/02 配置更新-手动搜索更新下-或用下面完整版
> uses: actions/checkout@v3 ---> uses: actions/checkout@v4
> uses: actions/setup-node@v3 ---> uses: actions/setup-node@v4
> node-version: 18.14 ---> node-version: 22
> uses: actions/upload-pages-artifact@v2 --->  uses: actions/upload-pages-artifact@v3
> uses: actions/deploy-pages@v2 --->  uses: actions/deploy-pages@v4

> [!tip] 懒人版本：复制粘贴即可
```
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - v4

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

```
npx quartz sync   # 每次更改后记得更新推送到远程仓库上
```

> [!NOTE] ：相关Obsidian文章参考：
>
> - [推荐一款强大的笔记管理软件：Obsidian](https://zhuanlan.zhihu.com/p/668713110)
>  - [Obsidian快速上手指南-免费的markdown双链笔记软件](https://zhuanlan.zhihu.com/p/682396800)
>  - [Obsidian中有哪些好用的插件值得推荐？](https://www.zhihu.com/question/497487995/answer/3445481654)


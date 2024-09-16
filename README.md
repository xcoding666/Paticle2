[README.md](https://github.com/user-attachments/files/17008213/README.md)
---
title: 使用前必看
tags: 标签 #可以给文章打标签
categories: 分类 #可以给文章分类
---
欢迎来到[Paticle2](https://github.com/xcoding666/particle2)!请给我的项目star!

此项目整合了hexo的运行环境,只需要 `npm install`就能安装依赖.

请在使用前看以下内容,否则将无法正常使用!!!

<!--more-->

# 部署教程

需要安装软件:[Git](https://github.com/git-for-windows/git/releases/download/v2.46.0.windows.1/Git-2.46.0-64-bit.exe "点我一键下载")和[Node.js](https://nodejs.org/dist/v20.13.1/node-v20.13.1-x64.msi "点我一键下载")

安装完成后,打开 `cmd`,输入:

```bash
npm install hexo-cli -g
```

安装完成后,`cd`到项目所在位置,输入:

```bash
npm install
```

需要改动的文件有2个 `_config.yml`:一个在根目录,一个在 `themes/particle2`中.

先看根目录内的 `_config.yml`.

记住: 每个冒号前一定要有空格,内容和注释用空格分隔,注释用 `#`标明.要修改的内容是黑体字,注释是灰色字.

第1-8行

```yaml
title: 网站标题(Xcoding666) #网站标题
subtitle: 网站副标题(A Developer of Particle2) #网站副标题
description: 网站小标题(My Github name is Xcoding666) #网站小标题
keywords:
author: Xcoding666 #网站作者
language: zh
timezone: ''
url: https://example.com # 请在此处填写你的Github网站地址,否则无法正常部署到Github,如果不需要,请填写你自己的地址.
```

第73-77行

```yaml
# 部署到Github,如果不需要,请忽略.
deploy:
  type: '' #如果需要,请在前面单引号中填写git.
  repo: #你的Gituhb仓库地址
  branch: main
```

再看 `themes/particle2/_config.yml`.

第34-51行.

```yaml
# 个人信息
card:
    enable: true
    description: | #下面一行写你的描述
      我的描述......
    iconLinks:
    friendLinks:
        作者主页: https://xcoding666.github.io
        # 你可以参照示例链接(link1)更改或增加链接.

# 页脚设置
# 考虑到博客部署在服务器并使用自己域名的情况,按规定需要在网站下边添加备案消息.
# 如没有需要显示备案消息的可以关闭(已经默认关闭,打开请把enbale=false改为true).
footer:
    since: 2014 #年份
    #ICP备案信息
    ICP:
        enable: false
        code: #ICP备案号
        link: #ICP备案链接
```

头像和背景可以修改 `themes/particle2/source/images`中的 `background.jpg`和 `avataar.jpg.`

# 写作教程

在 `source/_posts`中新建Markdown文件,要包含以下文件头:

```yaml
---
title: 标题
tags: 标签 #可以给文章打标签
categories: 分类 #可以给文章分类
---
```

然后用标准Markdown语法写作.用 `<!--more-->`缩略内容.

# 部署到Github

在Github绑定Git的SSHkey后,使用 `hexo d`一键部署.

# 在 GitHub Pages 上部署 Hexo

使用 [GitHub Actions](https://docs.github.com/zh/actions) 部署 GitHub Pages. 此方法适用于公开或私人储存库. 若你不希望将源文件夹上传到 GitHub,请参阅 [一键部署](https://hexo.io/zh-cn/docs/github-pages#一键部署).

1. 建立名为 ***username*.github.io**的储存库. 若之前已将 Hexo 上传至其他储存库,将该储存库重命名即可.
2. 将 Hexo 文件夹中的文件 push 到储存库的默认分支. 默认分支通常名为 **main** ,旧一点的储存库可能名为 **master** .

* 将 `main` 分支 push 到 GitHub：

  ```bash
  git push -u origin main
  ```
* 默认情况下 `public/` 不会被上传(也不该被上传),确保 `.gitignore` 文件中包含一行 `public/`. 整体文件夹结构应该与 [示例储存库](https://github.com/hexojs/hexo-starter) 大致相似.

3. 使用 `node --version` 指令检查你电脑上的 Node.js 版本. 记下主要版本（例如,`v20.y.z`）
4. 在储存库中前往 **Settings** > **Pages** > **Source** . 将 source 更改为  **GitHub Actions** ,然后保存.
5. 在储存库中建立 `.github/workflows/pages.yml`,并填入以下内容 (将 `20` 替换为上个步骤中记下的版本)：

.github/workflows/pages.yml

```.github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 20
        uses: actions/setup-node@v4
        with:
          # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
          # Ref: https://github.com/actions/setup-node#supported-version-syntax
          node-version: "20"
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

6. 部署完成后,前往  *username* .github.io 查看网页.

若你使用了一个带有 `CNAME` 的自定义域名,你需要在 `source/` 文件夹中新增 `CNAME` 文件. [更多信息](https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

## 项目页面

如果您希望在 GitHub 上有一个项目页面：

1. 导航到 GitHub 上的存储库. 转到 **Settings** 选项卡. 建立名为 `<repository 的名字>` 的储存库,这样你的博客网址为 `<你的 GitHub 用户名>.github.io/<repository 的名字>`,repository 的名字可以任意,例如 blog 或 hexo.
2. 编辑你的 `_config.yml`,将 `url:` 更改为 `<你的 GitHub 用户名>.github.io/<repository 的名字>`.
3. 在 GitHub 仓库的设置中,导航至 **Settings** > **Pages** > **Source** . 将 source 更改为  **GitHub Actions** ,然后保存.
4. Commit 并 push 到默认分支上.
5. 部署完成后,前往  *username* .github.io/*repository* 查看网页.

## 一键部署

以下教学改编自 [一键部署](https://hexo.io/zh-cn/docs/one-command-deployment).

1. 安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git).
2. 在 `_config.yml` 中添加以下配置（如果配置已经存在,请将其替换为如下）:

|

```
deploy:
  type: git
  repo: https://github.com/<username>/<project>
  # example, https://github.com/hexojs/hexojs.github.io
  branch: gh-pages
```

|  |
| - |

3. 执行 `hexo clean && hexo deploy` .
4. 浏览  *username* .github.io,检查你的网站能否运作.

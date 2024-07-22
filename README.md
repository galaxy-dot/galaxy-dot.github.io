# Hexo + NexT + Travis-CI/Github Action 建站历程 - 首发 2022 年 2 月 19 日

## 更新日志

- 2022.05.12 添加如何使用`Github Action`进行自动部署。
- 2022.07.09 使用自定义字体

## 安装 hexo

参考[官方文档](https://hexo.io/docs/)即可。

注意事项：

1. node 版本最好设置为 12。
2. 安装完成后，执行`hexo --version`查看各种 package 版本号及是否安装完成。

## 安装 NexT 主题

这部分的坑比较多，大致可以分为三步。

1. 哪个 NexT 主题版本
2. 怎么安装选定的 NexT 主题
3. 怎么使用选定的 NexT 主题

### 1. 哪个 NexT 版本

参考[discussion 条目](https://github.com/next-theme/hexo-theme-next/issues/4)，发现 NexT 有很多个版本，而且彼此不兼容。如果你是新建站，那推荐直接用最新 NexT 版本（在本文编辑的时候是 v8.2.0）。

我的这个站点用的是 NexT v8.2.0，源码的 github repository 地址是(https://github.com/next-theme/hexo-theme-next)。

### 2. 怎么安装选定的 NexT 主题

这里以安装 https://github.com/next-theme/hexo-theme-next 为例。其他版本的安装步骤应该是一样的，只需替换相应的 github repository 地址。

一个命令就可以完成。在 blog 根目录下执行  
`git submodule add https://github.com/next-theme/hexo-theme-next themes/next`

这个 command 的意思是：把 github 地址 https://github.com/next-theme/hexo-theme-next 以 `git submodule` 的形式添加到 `themes/next` 文件夹内。

注意事项：

1. 文件夹命名必须是`themes/next`。
2. 如果你在这一步遇到问题，比如无法添加 submodule 等情况，很可能是因为之前添加过。最高效的解决办法是删除已经添加的 submodule，然后重新执行上面的命令。删除 submodule 的具体步骤参考[delete_git_submodule.md](https://gist.github.com/myusuf3/7f645819ded92bda6677)，简单翻译。

```sh
删除 .gitmodules 文件中的对应的submodule部分。
添加改变 git add .gitmodules。
删除 .git/config 文件中的对应的submodule部分。
执行 git rm --cached <submodule的路径> (我们的情况下这个路径是themes/next，注意没有在next后没有/).
执行 rm -rf .git/modules/<submodule的路径> (我们的情况下这个路径是themes/next，注意没有在next后没有/).
执行 git commit -m "Removed submodule"。
执行 rm -rf path_to_submodule 来删除已经不被git 跟踪的 submodule 文件。
```

3. 如果 themes/next 文件夹内的 git log 包含了你自己的 git commit，运行下面 2 条 cmd 来重置 HEAD。

```
git fetch origin
git reset --hard origin/main
```

### 3. 怎么使用选定的 NexT 主题

因为主题是以 submodule 的形式添加的，因此无法把主题文件夹中的修改 push 到 github 上。这里不要看民间的各种攻略，几乎都不对，最高效的办法是直接参考[官方文档](https://theme-next.js.org/docs/getting-started/configuration.html)。

我的解决方法如下：

1. 在 blog 根目录下建立了一个新的主题文件，命名为`_config.next.yml`。
2. 把 `themes/next`目录下的 `_config.yml`内容全部复制到`_config.next.yml`。
3. 修改 blog 根目录下的`_config.yml`的`theme`值为`theme: next`。
4. 执行`hexo generate`，应该就能看到大大的 NEXT 啦。

## 怎么把本地 blog 上传到 github 以及完成自动部署

这里分为 2 大步骤：  
步骤 1: 把本地文件上传到 github 上   
步骤 2: 利用一些 CI/CD 工具/平台，比如`Travis-CI`, `Github Action` 等来完成自动部署。

在开始步骤 1 之前，需要先了解一下[不同种类的 github pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites)。总结来说，一般个人账户下有 2 类不同的 github pages，参考下表。

| 类别       | github 仓库地址                                                 | host 静态页面的 branch |
| ---------- | --------------------------------------------------------------- | ---------------------- |
| user 类    | `https://github.com/ <你的 github 用户名>/<你的 github 用户名>` | gh-pages                 |
| project 类 | `https://github.com/ <你的 github 用户名>/<你的 项目名>`        | gh-pages               |

我的这个 site 是 user 类。我用 `main` 分支来存储 blog 源码，用 `gh-pages` 分支来储存生成的静态页面 。  
接着我们就可以遵循正常 git push 流程把本地文件 push 到相应的 branch 上。

关于步骤 2，我最开始使用了`Travis-CI`， 后来因为 credit 超了，转而用`Github Action`。以下分别记录这 2 个工具的设置。

### 如何连接 `Travis-CI` 完成自动部署

~~`Travis-CI` 完成自动部署文件可以参考[官方文档](https://docs.travis-ci.com/user/deployment/pages/)，也可以参考这个 repo 里对应的`.travis.yml`文件。注意要修改 branch hexo 到你对应的 branch。~~

~~因为我们已经利用的 `travis-ci` 来完成部署，因此可以 comment 掉 blog 根目录下`_config.yml`文件中`# Deployment` 部分。~~

因为Travis 免费使用到期，无法继续使用，因此我研究了如何用 Github Action 部署，见下一部分。

### 如何连接 `Github Action` 完成自动部署

主要参考以下官方文档和民间攻略：

- [GitHub Pages action 官方文档](https://github.com/marketplace/actions/github-pages-action#%EF%B8%8F-set-another-github-pages-branch-publish_branch) - 参考了第一部分， username.github.io 类 repo 的设置

- [hexo github pages 官方文档](https://hexo.io/docs/github-pages) - 参考了 Set Another GitHub Pages Branch publish_branch，example yml 文件，submodule 设置

- [使用 GitHub Actions 自动部署 Hexo 博客 - 上篇](https://oreo.life/deploy-hexo-with-github-actions-1/#%E8%BD%AE%E5%AD%90%E5%86%8D%E9%80%A0-%E4%BD%BF%E7%94%A8-github-actions-%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2-hexo-%E5%8D%9A%E5%AE%A2---%E4%B8%8A%E7%AF%87) - 参考了 submodule 设置

具体设置文件可以参考这个 repo 里对应的`.github/workflows/deploy.yml`文档。
分解为5步：
1. clone 博客源码。注意要添加`submodules: 'true'`因为我们的主题是利用`submodule`引入的，这样才可以把主题一起clone到Github Actions的运行环境中。
```
    - name: Checkout code
      uses: actions/checkout@v2
      with:
        submodules: 'true'
```
2. 配置 Node.js 环境
```
    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'  # Use the version compatible with your Hexo setup
```
3. 配置 Hexo 环境
```
    - name: Install Hexo and plugins
      run: npm install
```
4. 使用 hexo generate 生成静态网页
```
    - name: Clean and generate static files
      run: |
        npx hexo generate
        echo "Generated files in public directory:"
        ls -la public
```
5. 将 public/ 目录下的静态网页部署到 GitHub Pages

一些重点注意事项罗列如下
- 在repo里`Settings` -> `Pages` -> `Build and deployment` -> `source` 选择 `Deploy from a branch`，默认 branch 名为 `gh-pages`.


### TODO：project 类 Github Action 设置，等以后用到了再写。

## 各种 feature 设置

- 添加 post 内的[Creative commons](https://theme-next.js.org/docs/theme-settings/index.html?highlight=creative+commons+license+section)
- 预览 Preamble Text[Preamble Text](https://theme-next.js.org/docs/theme-settings/posts.html?highlight=homepage)
- 添加图片或者其他资源[Asset Folders | Hexo - Static Site Generator | Tutorial 9](https://www.youtube.com/watch?v=feIDVQ2tz0o&t=89s)
- 使用自定义字体[Misc Theme Settings](https://theme-next.js.org/docs/theme-settings/miscellaneous)

## 勘误

如有错误，请指正。

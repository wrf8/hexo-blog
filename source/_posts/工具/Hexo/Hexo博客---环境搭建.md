---
title: Hexo博客 --- 环境搭建
date: 2021-03-06 19:02:20
categories:
- 工具
- Hexo
---

## 1.关于博客
常见的写博客的途径有：
- `借助平台`，比如掘金、CSDN、博客园、简书、有道云笔记等，这些平台的用户交互做的很好，但是缺乏个性化，有的更甚至需要购买会员，才能解锁所有功能。
- `自己购买域名和服务器`，虽然完全可以按照自己的想法来搭建，但是搭建成本比较高，还需要定期去维护，对于大多数来说，一可能没有这样的精力和时间，二没有这样的技术能力。
- `开源框架+GitHub`，这种方式就比较方便了，不用我们自己维护更新，网上有很多的搭建和个性化定制博客的教程，我们按照教程就可以慢慢搭建成我们自己想要的博客。

> 之前写文章用的比较多的是印象笔记，较早时候感觉印象笔记还是挺好的，但是随着后来自己要求的慢慢变高，就觉得印象笔记里面编辑出来的文章样式不满足自己的需求，再加上创建带目录的文章竟然需要购买会员。然后就萌生自己搭建博客的想法，然后就从网上查了相关资料和介绍，在前人的基础上搭建了一个简单的博客(`Hexo+GitHub`)

本篇博客旨在记录自己的搭建过程，方便以后的查看和修改，当然还有一些优化项和功能后续会慢慢更新。

## 2.关于Hexo
[Hexo](https://hexo.io/zh-cn/)基于`Node.js`，使用 `Markdown`（或其他渲染引擎）解析文章，在几秒内，可利用靓丽的主题生成静态网页，而且只需两三条命令即可将生成的网页上传到`github`等代码管理托管平台，简单、快速，是搭建博客的首选框架。
> `Hexo`的作者是来自台湾的 [Tommy Chen](https://zespia.tw/)，对中文的支持很友好，可以选择中文进行查看。

Hexo特点:
- 部署方便并且速度
- 支持 `Markdown`语法
- 使用极简命令即可部署到`GitHub Pages`
- 高扩展性、可个性化定制
- 兼容于 `Windows`, `Mac` & `Linux`

## 3.搭建
本人使用Mac电脑，以此只记录了Mac电脑下的搭建过程

### 3.1 安装Git
什么是`Git`？关于`Git`，廖雪峰老师的[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)讲的特别好，大家可以去看看，也可以直接查看 [Git官网 Pro Git Book](https://git-scm.com/book/zh/v2)中的讲解。而我们为了把本地的网页文件上传到`github`上面去，需要用到分布式版本控制工具`Git`。

在 `Mac` 上安装 `Git` 有多种方式。最简单的方法是安装 `Xcode Command Line Tools`。 Mavericks（10.9）或更高版本的系统中，在 `Terminal` 里尝试首次运行 `git` 命令即可。
```
$ git --version
```
如果没有安装过命令行开发者工具，将会提示你安装。

如果你想安装更新的版本，可以使用二进制安装程序。 官方维护的 macOS Git 安装程序可以在 Git 官方网站下载，网址为 https://git-scm.com/download/mac。

```
git --version
```
> 如果你想安装更新的版本，可以使用二进制安装程序。官方维护的 `macOS Git` 安装程序可以在 `Git` 官方网站下载，网址为 [https://git-scm.com/download/mac](https://git-scm.com/download/mac)

### 3.2 安装Node.js
打开 [Node.js](https://nodejs.org/en/download/) 下载页面，直接选择 `macOS Installer` 下载，然后双击已下载的`pkg`文件安装即可。
![](/images/工具/Hexo/hexo_node_js_download.png)
安装完成之后，可通过以下命令来检查是否安装成功:
```
node -v
```

### 3.3 安装Hexo
安装完 `Git` 和 `Node.js`之后，即可使用 `npm` 安装 `Hexo`。终端输入如下命令：
```
sudo npm install -g hexo-cli
```
安装完成之后输入 `hexo -v` 验证是否安装成功。
> Hexo官网上的安装命令是 `npm install -g hexo-cli`，但是实际安装的时候需要加上 `sudo`，否则会因为权限问题报错而导致安装失败

#### 3.3.1 初始化
使用终端 `cd` 到一个文件目录，执行以下命令（命令中的 `Blog` 是要创建的博客文件夹）:
```
hexo init Blog
```
使用终端 `cd` 到 `Blog` 文件夹下，执行以下命令，安装必备的组件：
```
cd Blog
npm install
```
安装完成之后，`Blog` 目录下有：
- `node_modules`: 依赖包
- `public`: 存放生成的页面
- `scaffolds`: 生成文章的一些模板
- `source`: 用来存放你的文章
- `themes`: 安装的主题
- `_config.yml`: 博客的配置文件

#### 3.3.2 本地预览
经过上面步骤之后，基本的博客系统就已经搭建好了，而且自带了 `landscape` 主题，执行如下命令，开启本地 `Hexo` 服务器:
```
sudo npm install hexo-server  //安装hexo server
hexo generate //或者 hexo g，生成静态页面
hexo server //或者 hexo s，本地预览
```
然后浏览器打开[http://localhost:4000/](http://localhost:4000/)，就可以看到我们的博客啦，效果如下：
![](/images/工具/Hexo/hexo_theme_preview_landscape.png)

### 3.4 将Hexo部署到GitHub Pages
#### 3.4.1 创建仓库
登录 [GitHub](https://github.com/) 账号，新建名称为 `<你的 GitHub 用户名>.github.io` 的仓库，具体如下图所示：
![](/images/工具/Hexo/hexo_github_repository_creat.png)

#### 3.4.2 开启GitHub Pages
![](/images/工具/Hexo/hexo_github_repository_setting.png)
点击 `setting` ,打开创建的仓库的设置页面，滑动到 `GitHub Pages` 位置，进行如下设置，这样才能通过 `<你的 GitHub 用户名>.github.io`来访问
![](/images/工具/Hexo/hexo_github_repository_pages.png)

#### 3.4.3 生成SSH添加到GitHub
`GitHub` 使用 `SSH` 公钥进行认证，关于 `SSH` 相关内容可以直接查看另一篇博客[Terminal---生成SSH公钥](2021/03/07/工具/Terminal/Terminal---生成SSH公钥)

在 `GitHub` 中，点击头像打开 `Setting` 页面，将 `SSH` 添加到 `SSH and GPG keys`中，如下图所示：
![](/images/工具/Hexo/hexo_github_ssh.png)
然后在终端输入 `ssh -T git@github.com`，如果出现 `Hi xxx! You've successfully authenticated...`，那么恭喜你，添加成功了。

#### 3.4.4 将Hexo部署到GitHub Pages
打开 `Blog` 根目录下的 `_config.yml` 文件，这是博客的配置文件，在这里可以修改博客配置的各种相关信息，修改 `deploy` 相关信息如下：
```
deploy:
  type: git
  repository: https://github.com/xxx/xxx.github.io.git //此地址为自己github的仓库地址
  branch: master
```
> 修改 `_config.yml` 文件时，所有的冒号 `:` 后面都要加一个空格，否则执行命令时会报错。

配置之后，`Hexo` 和 `GitHub` 就能关联起来，部署时，就可以将生成的静态网页上传到上面填写的仓库中。

然后就可以通过下面命令，将本地 `Hexo` 博客部署到 `GitHub` 上：
```
npm install hexo-deployer-git --save //安装 deploy-git
hexo clean  //清除之前生成的静态文件
hexo g  //生成静态页面至public目录
hexo d  //将.deploy目录部署到GitHub
```
至此博客已部署完成，可以通过 `https://xxx.github.io/` 来访问你的博客啦！！！

## 4.个性化定制
### 4.1 基本配置
在文件根目录下的 `_config.yml` ，就是整个hexo框架的配置文件，可在文件中设置博客站点的标题、副标题等相关信息。
#### 4.1.1 新建page
如果新建page页，可以使用
```
hexo new page newpage
```
系统会自动在`source`文件夹下创建一个`newpage`文件夹，以及`newpage`文件夹中的`index.md`，这样访问的`newpage`对应的链接就是[http://xxx.xxx/newpage](http://xxx.xxx/newpage)

#### 4.1.2 新建分类categories页
`categories` 页是用来展示所有分类的页面，如果在你的博客 `source` 目录下还没有 `categories/index.md` 文件，那么你就需要新建一个，命令如下：
```
hexo new page "categories"
```
编辑你刚刚新建的页面文件 `/source/categories/index.md`，至少需要以下内容：
```
---
title: categories
date: 2021-03-03 08:25:30
type: "categories"
layout: "categories"
---
```

#### 4.1.3 新建标签tags页
`tags` 页是用来展示所有标签的页面，如果在你的博客 `source` 目录下还没有 `tags/index.md` 文件，那么你就需要新建一个，命令如下：
```
hexo new page "tags"
```
编辑你刚刚新建的页面文件 `/source/tags/index.md`，至少需要以下内容：
```
---
title: tags
date: 2021-03-03 08:25:30
type: "tags"
layout: "tags"
---
```

#### 4.1.4 新建关于我about页
`about` 页是用来展示关于我和我的博客信息的页面，如果在你的博客 `source` 目录下还没有 `about/index.md` 文件，那么你就需要新建一个，命令如下：
```
hexo new page "about"
```
编辑你刚刚新建的页面文件 `/source/about/index.md`，至少需要以下内容：
```
---
title: about
date: 2021-03-03 08:25:30
type: "about"
layout: "about"
---
```

#### 4.1.5 添加404页面
原来的主题没有`404`页面，我们加一个。首先在`/source/`目录下新建一个`404.md`，内容如下：
```
title: 404
date: 2019-08-5 16:41:10
type: "404"
layout: "404"
description: "Oops～，我崩溃了！找不到你想要的页面 :("
```
网上好多都说要在`/themes/matery/layout/`目录下新建一个`404.ejs`文件(注：我安装的主题在该目录下已有`404.ejs`文件，无需自己创建)
```
<style type="text/css">
    /* don't remove. */
    .about-cover {
        height: 75vh;
    }
</style>

<div class="bg-cover pd-header about-cover">
    <div class="container">
        <div class="row">
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
                <div class="brand">
                    <div class="title center-align">
                        404
                    </div>
                    <div class="description center-align">
                        <%= page.description %>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // 每天切换 banner 图.  Switch banner image every day.
    $('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)');
</script>
```

### 4.2 更换主题
hexo 搭建之后默认安装的主题是 `landscape`，更多风格主题查看 [Hexo官网Themes](https://hexo.io/themes/)，可以从里面选择自己喜欢的主题进行安装。

#### 4.2.1 主题推荐
- [NexT主题](https://github.com/next-theme/hexo-theme-next)：简洁美观，目前Github上Star最高的Hexo主题，支持几种不同的风格。
  ![](/images/工具/Hexo/hexo_theme_preview_next.png)
- [Metery主题](https://github.com/blinkfox/hexo-theme-matery)：简单漂亮，瀑布流式的文章列表，文章内容美观易读。
  ![](/images/工具/Hexo/hexo_theme_preview_matery.png)
- [Wikitten主题](https://github.com/zthxxx/hexo-theme-Wikitten)：简单，双列，分类管理，具备多层次分类知识，适用于个人Wiki知识管理。
  ![](/images/工具/Hexo/hexo_theme_preview_wikitten.png)

#### 4.2.2 主题安装
我们所有的主题都被放在根目录 `/themes` 文件夹下，可以将主题的的源码下载到 `/themes` 文件夹下，然后将 Hexo 根目录下的 `_config.yml` 中的主题设置为下载的主题：
```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: 主题 #更改为自己下载的主题
```
配置好后，执行 `hexo clean` 和 `hexo g & s` 就能看到博客已经更换成新安装的主题风格了。
---
title: Hexo博客 --- 写文章
date: 2021-03-06 19:03:50
categories:
- 工具
- Hexo
tags:
---

## 1.创建文章模版
### 1.1 文章模版
为了新建文章方便，我们可以修改一下文章模板，建议将`/scaffolds/post.md`修改为如下代码：
```
---
title: {{ title }}
date: {{ date }}
author: 
img: 
coverImg: 
top: false
cover: false
toc: true
mathjax: false
password:
summary:
tags:
categories:
---
```
这样新建文章后 一些`Front-matter`参数不用你自己补充了，修改对应信息就可以了

### 1.2 Front-matter
`Front-matter` 选项中的所有内容均为非必填的。但仍然建议至少填写 `title` 和 `date` 的值

|  配置选项  |    默认值 |   描述 |
| :-----: | :-------------: | :-------------: |
| title | Markdown的文件标题 | 文章标题 |
| date | 文件创建时的日期时间 | 发布时间 |
| author | 根 `_config.yml` 中的 `author` | 文章作者 |
| img | `featureImages` 中的某个值 | 文章特征图 |
| top | `true` | 推荐文章（文章是否置顶），如果 `top` 值为 `true`，则会作为首页推荐文章 |
| cover | `false` | 表示该文章是否需要加入到首页轮播封面中 |
| coverImg | 无 | 表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片 |
| password | 无 | 文章阅读密码，如果要对文章设置阅读验证密码的话，就可以设置 `password` 的值，该值必须是用 `SHA256` 加密后的密码，防止被他人识破。前提是在主题的 `config.yml` 中激活了 `verifyPassword` 选项|
| toc | `true` | 是否开启 `TOC`，可以针对某篇文章单独关闭 `TOC` 的功能。前提是在主题的 `config.yml` 中激活了 `toc` 选项 |
| mathjax | `false` | 是否开启数学公式支持 ，本文章是否开启 `mathjax`，且需要在主题的 `_config.yml` 文件中也需要开启才行 |
| summary | 无 | 文章摘要，自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories | 无 | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类 |
| tags | 无 | 文章标签，一篇文章可以多个标签 |

> 注意：
> 1. 如果 `img` 属性不填写的话，文章特色图会根据文章标题的 `hashcode` 的值取余，然后选取主题中对应的特色图片，从而达到让所有文章都的特色图各有特色。
> 2. `date` 的值尽量保证每篇文章是唯一的，因为本主题中 `Gitalk` 和 `Gitment` 识别 `id` 是通过 `date` 的值来作为唯一标识的。
> 3. 如果要对文章设置阅读验证密码的功能，不仅要在 `Front-matter` 中设置采用了 `SHA256` 加密的 `password` 的值，还需要在主题的 `_config.yml` 中激活了配置。有些在线的 `SHA256` 加密的地址，可供使用：开源中国在线工具、chahuo、站长工具。

## 2.创建文章
### 2.1 直接创建
可以通过以下命令创建文章，该文件所在目录为 `_posts/xxxx.md`
```
$ hexo new xxxx.md
```

### 2.2 按分类创建文章
直接通过 `hexo new post xxxx.md` 来创建的文章，所有都在 `_posts`目录下，这样很不好归类查找，如果想在 _posts 目录下创建子目录分类文章的话，可通过下面方式创建
```
$ hexo new post -p 目录1/目录2/.../文章.md
```

## 3.文章技巧
### 3.1 文章添加图片
#### 3.1.1 本地引用
1. 绝对路径
当 Hexo 项目中只用到少量图片时，可以将图片统一放在 `source/images` 文件夹中，通过 `markdown` 语法访问它们
```
![](/images/image.jpg)
```
2. 相对路径
直接放在文章自己的目录中，需要将hexo根目录下的 `_config.yml` 配置文件中的 `post_asset_folder` 设为 `true` ，然后执行：
```
$ cnpm install hexo-asset-image
```
此时再执行命令 `hexo n article_name` 创建新的文章，在 `source/_posts` 中会生成文章 `post_name.md` 和同名文件夹 `post_name`，我们文章中所使用到的图片资源均放在 `post_name` 中，这时就可以在文章中使用相对路径引用图片资源了：
```
![](img_name.jpg) //文章中的图片资源路径格式
```
#### 3.1.2 CDN
将图片上传到一些免费的`CDN`服务中。比如 `Cloudinary` 提供的图片CDN服务，在 `Cloudinary` 中上传图片后，会生成对应的 `url`地址，将地址直接拿来引用即可。




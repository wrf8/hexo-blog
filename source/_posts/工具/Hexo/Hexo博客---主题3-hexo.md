---
title: Hexo博客 --- 主题3-hexo
categories:
  - 工具
  - Hexo
abbrlink: 5d51cb1e
date: 2021-03-07 23:14:22
tags:
---

> 一千个读者心中有一千个哈姆雷特，同样的主题也可根据不同人的喜好演变成不同的风格，此博客是自己对主题3-hexo个性化定制的记录。

## 1.配置首页
首页文件位置为 `/layout/indexs.md`，可根据自己需要来配置相关信息。
### 

### 1.左侧栏
#### 1.1 修改左侧栏宽度
打开 `themes/_config.yml` 文件中，更改对应代码对应的宽度即可
```
width:
    lg: 120 # 1468px<屏幕宽度 左侧分类宽度
    md: 100 # 1024px<屏幕宽度<=1468px 左侧分类宽度
    sm: 100 # 426px<屏幕宽度<=1024px 左侧分类宽度（ipad）
```

#### 1.2 修改背景颜色
打开`themes/source/css/_partial/nav-left.styl`文件，将`.nav_left`的`background`设置成自己喜欢的颜色即可

#### 1.3 修改图片
`themes/source/img`文件中，`avatar.jpg`为头像，`weixin.jpg`和`alipay.jpg`分别对应打赏的微信和支付宝收款图片，直接将图片替换为自己的即可 

### 2.文章列表栏
#### 2.1 去除文章列表时间
打开`themes/layout/_partial/nav_right.ejs` 文件，将下面代码删除即可
```
<span class="post-date" title="<%= date(post.date, 'YYYY-MM-DD HH:mm:ss')%>"><%= date(post.date, 'YYYY/MM/DD') %></span>
```

### 3.关于写作
#### 3.1 字数统计
```
npm i --save hexo-wordcount
```
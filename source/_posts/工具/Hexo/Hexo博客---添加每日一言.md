---
title: Hexo博客 --- 添加每日一言或诗词
categories:
  - 工具
  - Hexo
abbrlink: 1bafec33
date: 2021-03-07 23:15:11
tags:
---

看到好多博客上都带有每日一言或者诗词功能，挺好看的，然后就按耐不住自己的小手手给自己的博客也安排上了。
## 1.每日一言
### 1.1 简介
> 一言指的就是一句话，可以是动漫中的台词，也可以是网络上的各种小段子。或是感动，或是开心，有或是单纯的回忆。
> <p align="right">一言官方网站</p>

实现每日一言功能，需借助 [一言网](https://links.jianshu.com/go?to=Hitokoto.cn)，创立于2016年，隶属于萌创Team，目前网站主要提供一句话服务。

### 1.2 使用
关于每日一言如何使用，一言网关于[语句接口](https://pa-1251215871.cos-website.ap-chengdu.myqcloud.com/sentence/)介绍的很详细，以下是以 `hexo-3-hexo`主题为例的使用说明：

首先打开主题的 `index.ejs`文件，路径`theme/3-hexo/layout/index.ejs`，将下面的代码添加到`<body>`之前:
```
<!-- 一言API -->
<!-- 现代写法，推荐 -->
<!-- 兼容低版本浏览器 (包括 IE)，可移除 -->
<script src="https://cdn.jsdelivr.net/npm/bluebird@3/js/browser/bluebird.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/whatwg-fetch@2.0.3/fetch.min.js"></script>
<!--End-->
<script>
  fetch('https://v1.hitokoto.cn')
    .then(function (res){
      return res.json();
    })
    .then(function (data) {
      var hitokoto = document.getElementById('hitokoto');
      hitokoto.innerText = data.hitokoto + '——【' + data.from + '】';
    })
    .catch(function (err) {
      console.error(err);
    })
</script>
```
然后再将下面的代码，添加到你想要添加的位置:
```
<p id="hitokoto">正在加载一言...</p>
```
最后执行hexo三步连操作即可看到效果。

## 2.今日诗词
[今日诗词API](https://www.jinrishici.com/doc/#about)，是一个可以返回一句古诗词名句的接口。它可以通过图片和 JSON 格式调用。今日诗词 API 根据不同地点、时间、节日、季节、天气、景观、城市进行智能推荐。
### 2.1 修改样式
修改今日诗词显示的样式，也可以不修改哟😜，在\themes\next\source\css\style.styl中适当位置添加以下样式代码：
```
/*诗*/
.poem-wrap {
    position: relative;
    width: 730px;
    max-width: 80%;
    border: 2px solid #797979;
    border-top: none;
    text-align: center;
    margin: 80px auto;
}
 
.poem-wrap h1 {
    font-size: 30px;
    position: relative;
    margin-top: -20px;
    display: inline-block;
    letter-spacing: 4px;
    color: #797979
}
 
.poem-wrap p {
    width: 70%;
    margin: auto;
    line-height: 30px;
    color: #797979;
}
 
.poem-wrap p#poem {
    font-size: 25px;
}
 
.poem-wrap p#info {
    font-size: 15px;
    margin: 15px auto;
}
 
.poem-border {
    position: absolute;
    height: 2px;
    width: 27%;
    background-color: #797979;
}
 
.poem-right {
    right: 0;
}
 
.poem-left {
    left: 0;
}
 
@media (max-width: 685px) {
    .poem-border {
        width: 18%;
    }
}
 
@media (max-width: 500px) {
    .poem-wrap {
        margin-top: 60px;
        margin-bottom: 20px;
        border-top: 2px solid #797979;
    }
 
    .poem-wrap h1 {
        margin: 20px 6px;
    }
 
    .poem-border {
        display: none;
    }
}
```
### 2.2 使用
在需要使用的 `.html` 文件或`.md`中添加:
```
<div class="poem-wrap">
  <div class="poem-border poem-left"></div>
  <div class="poem-border poem-right"></div>
    <h1>今日诗词</h1>
    <p id="poem">挑选中...</p>
    <p id="info">

  <script src="https://sdk.jinrishici.com/v2/browser/jinrishici.js" charset="utf-8"></script>
  <script type="text/javascript">
    jinrishici.load(function(result) {
      poem.innerHTML = result.data.content
      info.innerHTML = '【' + result.data.origin.dynasty + '】' + result.data.origin.author + '《' + result.data.origin.title + '》'
      document.getElementById("poem").value(poem);
      document.getElementById("info").value(info);  
  });
  </script>
</div>
```

### 2.3 效果
<div class="poem-wrap">
  <div class="poem-border poem-left"></div>
  <div class="poem-border poem-right"></div>
    <h1>念两句诗</h1>
    <p id="poem">挑选中...</p>
    <p id="info">

  <script src="https://sdk.jinrishici.com/v2/browser/jinrishici.js" charset="utf-8"></script>
  <script type="text/javascript">
    jinrishici.load(function(result) {
      poem.innerHTML = result.data.content
      info.innerHTML = '【' + result.data.origin.dynasty + '】' + result.data.origin.author + '《' + result.data.origin.title + '》'
      document.getElementById("poem").value(poem);
      document.getElementById("info").value(info);  
  });
  </script>
</div>



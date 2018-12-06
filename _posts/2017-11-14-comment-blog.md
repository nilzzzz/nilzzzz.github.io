---
layout: post
title: 在Jekyll博客添加评论系统：gitment
tag: 博客
---

之前个人博客搭建完成之后，一直想要用**多说**添加评论系统，但是多说在六月一要停止使用就没管了，<br/>
一直用**disqus**，国外比较火的评论系统，但是国内被强~<br/> **畅言**，sohu旗下的，但是个人博客备案才能使用，<br/>但github pages备案个人感觉略难，就不了了之；<br/>然后今天无意翻看别人的blog发现**来必力**，但是来自韩国（本来想说棒子国的，萨德没完事儿），拒绝使用。<br/>
综上，国内比较主流的评论系统，后面发现了**gitment**，一款由国内大神imsun开发的基于github issues的评论系统，具体介绍请前往项目主页（github.com/imsun/gitment）.<br/>

### 1.申请一个Github OAuth Application
Github头像下拉菜单 > Settings > 左边Developer settings下的OAuth Application > Register a new application，填写相关信息：<br/>
![此处输入图片的描述][1]

 1. Application name, Homepage URL, Application description 都可以随意填写
 2. Authorization callback URL 一定要写自己Github Pages的URL
 3. 填写完上述信息后按Register application按钮，得到Client ID和Client Secret

### 2.在jekyll博客调用gitment
如gitment项目页Readme所示，在你需要添加评论系统的地方，一般是_layout/目录下的 post.html, 添加一下代码

    <div id="gitmentContainer"></div>
    <link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
    <script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
    <script>
    var gitment = new Gitment({
        owner: 'Your GitHub username', //比如我的叫nilzzzz
        repo: 'The repo to store comments', //比如我的叫nilzzzz.github.io
        oauth: {
            client_id: 'Your client ID', //比如我的828***********
            client_secret: 'Your client secret', //比如我的49e************************
        },
    });
    gitment.render('gitmentContainer');
    </script>
为了灵活，我在_config.yml中配置好全局参数：<br/>

![此处输入图片的描述][2]

### 3.初始化评论

页面发布后，你需要访问页面并使用你的 GitHub 账号登录（请确保你的账号是第二步所填 repo 的 owner），点击初始化按钮。<br/>
**初始化：点击下初始化即可**

![此处输入图片的描述][3]

之后其他用户即可在该页面发表评论

> 异常：通常是repo或者owner配置不对，请细心检测Error：Not Found。还有本地调测会出现callback不对提示Error:
> Comments Not Initialized

### 巴拉巴拉
帅哥美女们留个言再走呗~ 不要钱~<br/>
有问题微博私或评论留言哟~比心~<br/>


  [1]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/comment-blog.png
  [2]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/comment-blog1.png
  [3]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/comment.png
---
layout: post
title: Python下载在线视频
tag: 在线视频下载 实用技能
---

### 问题描述

 1. 怎么用命令下载在线视频？
 2. 怎么用命令下载高清的视频？
 3. 怎么修改下载的视频存放位置？
 

> 我们知道有些网页上的视频是没有下载按钮的，甚至是需要付费下载的，最近课堂展示需要一段视频，从b站的下的只能用b站打开，翻墙去YouTube发现无法下载，各种录屏效果还很差，倒是发现一款录屏软件很nice，推荐服用	
> [FastStone Capture][1]，滚动截屏也很不错（跑题了...）

### 用Python下载在线视频
![此处输入图片的描述][2]

 1. 你需要配置Python的本地环境，安装Python3版本
 2. 使用快捷键：win+R,打开运行界面（cmd）
 3. 在cmd窗口输入：pip install you-get
 4.在cmd窗口输入：you-get+所需下载的视频链接，然后就自动下载，并且保存到电脑用户目录下了
> you-get 它是Python3的一个下载工具 我们可以使用you-get很轻松的下载到网络上的视频、图片及音乐。
> 目前you-get可以下载的网站有很多，比如国外的Youtube、Instagram等，国内的爱奇艺、优酷、乐视、哔哩哔哩等 
 
 ![此处输入图片的描述][3]



### 如何下载高清视频

> you-get -i +视频链接 可以查看视频的清晰度有哪些：超清、高清、标清

![此处输入图片的描述][4]

> 例如：you-get --format=hdflv +视频链接

![此处输入图片的描述][5]
 
  

> you-get -o +视频下载的文件路径+视频链接   就能在指定的文件夹内看到要下载的视频了

### 巴拉巴拉

> 微习惯就是你强迫自己每天做的**微不足道**的积极行动，微步骤每次都能有微效果，而习惯来自坚持，所以它俩是注定要在一起的。   
>                                                    --<<微习惯>>

 



  


  [1]: https://faststone-capture.en.softonic.com
  [2]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/video-1.png
  [3]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/video-2.png
  [4]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/video-3.png
  [5]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/video-4.png
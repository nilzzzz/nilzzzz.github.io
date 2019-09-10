---
layout: post
title: 推荐系统常用数据集介绍
tag: 推荐系统
---

### 什么是推荐系统

> 推荐系统也称为个性化推荐系统。它本质上是一种信息过滤系统，通过一定的算法在海量数据中过滤掉用户不太可能产生行为的物品，从而为用户推荐所需要的物品。

### 推荐系统常用数据集介绍
数据是推荐系统的基石，一个优质的数据集可以对推荐算法的验证起到积极的作用。

#### MovieLens数据集
MovieLens数据集是一个关于电影评分的数据集。<br/>
官网上提供了若干大小不一的数据集，下载地址为：[http://grouplens.org/datasets/movielens][1]

#### Book-Crossings数据集
该数据集是由Cai-Nicolas Ziegler 在Ron Hornbaker的许可下，从Book-Crossing社区收集的图书评分数据集。
它包含了278858个用户对271379本图书的1149780个评分数据。<br/>
数据集下载地址为：[http://www2.informatik.uni-freiburg.de/~cziegler/BX/][2]
 
#### Last.fm数据集
Last.fm是一个音乐网站，提供了音乐推荐的数据集。数据集中的每个用户都包含他们喜欢的艺术家列表和播放喜欢的艺术家音乐的播放次数，以及他们对艺术家所打的标签。<br/>

> <strong>提示：</strong>
> Last.fm数据集中包含了用户与用户之间的朋友关系，因此该数据集是一个具有用户社交网络信息的数据集。

可以从GroupLens网站（[https://grouplens.org/datasets/hetrec-2011][3])下载Last.fm数据集。

#### FourSquare数据集

> FourSquare是一家基于用户地理位置信息的手机服务网站，鼓励手机用户同他人分享自己当前所在地理位置等信息。2013年公开的FourSquare数据集包含2153469个用户、1143090个场馆、1021966个签到、27098488个社交连接及2809580个用户对场馆的评价。这些数据都是通过公共API从Foursquare应用程序中提取的。

> <strong>提示：</strong>
所有用户信息都是匿名的，即用户地理定位是匿名的。各用户用ID和地理空间位置表示，场馆信息也是匿名的。


#### Kaggle比赛之retailrocket数据集
retailrockeshu数据集是一个真实的电子商务网站用户的行为数据，没有进行任何内容转换。但是由于安全问题，对所有数据都进行了Hash处理。该数据适合于研究隐式反馈推荐系统。
retailrocket数据集的下载地址为：[http://www.kaggle.com/retailrocket/ecommerce-dataset][4]

### 场景分析
MovieLens数据集，主要适用于评分预测类的推荐场景。<br/>
Last.fm数据集，主要使用于基于标签的推荐场景，也适用于评分预测类和社交关系类的推荐场景。<br/>
FourSquare数据集，主要适用于基于位置的推荐场景，也适用于评分预测类推荐场景。<br/>
Kaggle retailrocket数据集，主要适用于隐私反馈推荐场景。<br/>
Book-Crossings数据集，主要适用于评分预测类的推荐场景。该数据的原数据中包含异常数据，旨在说明在使用数据之前需要进行数据有效性验证。<br/>

### 知识图谱
![此处输入图片的描述][5]


  [1]: http://grouplens.org/datasets/movielens
  [2]: http://www2.informatik.uni-freiburg.de/~cziegler/BX/
  [3]: https://grouplens.org/datasets/hetrec-2011
  [4]: http://www.kaggle.com/retailrocket/ecommerce-dataset
  [5]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/%E6%8E%A8%E8%8D%90%E7%B3%BB%E7%BB%9F-%E6%95%B0%E6%8D%AE%E9%9B%86.png
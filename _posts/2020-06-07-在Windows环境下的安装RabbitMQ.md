---
layout: post
title: 在Windows环境下的安装RabbitMQ
tag: RabbitMQ
---

## 在Windows环境下的安装RabbitMQ

需要保证erlang和RabbitMQ版本对应：https://www.rabbitmq.com/which-erlang.html

## 1.下载erlang 

原因在于RabbitMQ服务端代码是使用并发式语言erlang编写的

下载地址：http://www.erlang.org/downloads，双击.exe文件进行安装

安装完成之后创建一个名为ERLANG_HOME的环境变量，其值指向erlang的安装目录，同时将%ERLANG_HOME%\bin加入到Path中

最后打开命令行，输入erl，如果出现erlang的版本信息就表示erlang语言环境安装成功；

![image-20200522140625061](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/22/140626-957692.png)

问题：

如果erl安装不成功，除了没有配置环境外，还会出错的话且报找不到msvcr120.dll,需要重新安装服务 ，参考

https://blog.csdn.net/lsqingfeng/article/details/90063824?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase

使用DirectX软件进行系统修复

## 2.安装MQ

Windows环境下载地址：https://www.rabbitmq.com/install-windows.html#installer

![image-20200607110035403](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202006/07/110035-857328.png)

rabbitmq_managemen是管理后台的插件、我们要开启这个插件才能通过浏览器访问登录页面

打开cmd命令行窗口

1、进入到sbin目录下执行：

```
rabbitmq-plugins enable rabbitmq_management
```

2、开启服务执行：

```
rabbitmq-server
```

打开浏览器访问：http://localhost:15672

默认UserName:guest  

​		Password:guest

![image-20200607105849109](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202006/07/105849-872962.png)
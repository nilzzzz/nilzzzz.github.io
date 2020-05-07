---
layout: post
title: Pycharm远程调试docker containers
tag: Docker
---

### Pycharm远程调试docker containers

- 环境

  Ubuntu 16.04(远程服务器)

  Windows(本地)

  docker(远程服务器)

  openssh-server(远程服务器)

  Pycharm profession版(本地)

  Xshell(本地)

- 原理

  本地利用SSH链接远程服务器交互数据，在本地Pycharm上显示远程结果

- 配置流程

  1.在远程服务器创建docker container

  2.远程服务器ssh服务配置

  3.Pycharm链接远程服务器(文件同步)

  4.Pycharm链接远程的docker container (配置远程编译器)

### 一、远程服务器创建docker container

在这步之前，你应该安装好docker并且下载好了相应的image。(如果你有GPU，那么同时需要配置好cuda)

注意：配置容器8022的端口映射，我这里使用的是8022映射到8022

### 二、远程服务器ssh服务配置

 第一步，我们需要在远程服务器和远程服务器上的docker container都要安装上openssh-server

```
apt update && apt install openssh-server
```

第二步，安装完成以后需要配置ssh服务

```
# 次配置在docker container中完成
$ echo 'root:test' | chpasswd
# 将Root的密码修改为test
$ sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
# 允许使用root身份登录
$ sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
# 限制 PAM方式登录

$ echo "export VISIBLE=now" >> /etc/profile
```

![image-20200507083340412](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/083343-249857.png)



第三步，配置好ssh服务之后重启ssh服务

```
service ssh restart
```

![image-20200507083428346](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/083433-318138.png)

第四步，测试docker container中ssh服务端口在远程服务器上的映射

```
# 此操作在远程服务器
$ docker port <your container name> 8022
# 此操作将查看docker container中端口8022，在远程服务器上端口的映射

# 输出结果如下所示
0.0.0.0:8022
# 表明只要ssh链接远程服务器的8022端口，实际是链接docker container中的8022端口。
```

第五步，测试是否能够使用ssh链接docker container

```
ssh root@<你服务器的ip地址> -p 8022 # 密码就是刚刚重新设置的test
```

### Pycharm链接远程docker container(文件同步)

第一步，配置SFTP

在导航栏中 **Tools>Depolyment>Configuration**中添加配置SFTP。

![image-20200507084458103](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/084459-532995.png)

添加配置SFTP，点击弹窗左上角的**+**号。选择**SFTP**，根据自己的实际情况进行配置。这里的root密码就是之前设置好的**test**

![image-20200507084742553](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/084825-147015.png)

第三步，配置SFTP中的mapping

![image-20200507084929237](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/084933-744849.png)

都配置完之后。打开自动上传功能
**Tools>Depolyment>Automatic Upload(always)**
本地修改好代码只要按保存键就自动将本地代码上传至远程docker container中。

![image-20200507085019696](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/085032-390031.png)

### Pycharm链接远程docker container (配置远程编译器)

添加新编译器(远程docker container编译器)**File>Setting>Project>Project Interpreter**

![image-20200507090237105](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/07/090238-184723.png)

ps:可以通过`whereis python`找到环境路径

```
(base) root@4dd70914dd3b:/# whereis python
python: /usr/lib/python2.7 /opt/conda/bin/python3.7 /opt/conda/bin/python /opt/conda/bin/python3.7m-config /opt/conda/bin/python3.7m /opt/conda/bin/python3.7-config
```

参考博客：https://blog.csdn.net/github_33934628/article/details/80919646


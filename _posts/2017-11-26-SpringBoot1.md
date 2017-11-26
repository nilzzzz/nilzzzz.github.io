---
layout: post
title: SpringBoot热部署
tag: Spring Boot
---

### 热部署的使用场景
1.本地调试<br/>
2.线上发布

**思考：**<br/>
一些网站或者服务，<font color="red">全年不间断运行</font>，即使重新发布程序后也不需要重启服务，他们是怎么做到的呢？<br/>
**优点：**<br/>

 1. 无论本地还是线上，都适用
 2. 无需重启服务器<br/>
-提高开发、调试效率<br/>
-提升发布、运维效率，降低运维成本<br/>

### 热部署与热加载的关系
**Java热部署与热加载联系**<br/>

 1. 不重启服务器编译/部署项目
 2. 基于Java的类加载器实现

### 热部署与热加载的区别
**部署方式:** <br/>
1.热部署在服务器运行是重新部署项目<br/>
2.热加载在运行时重新加载class

**实现原理：** <br/>

 1. 热部署直接重新加载整个应用 <br/>
 释放内存，浪费时间<br/>
 2. 热加载在运行是重新加载class 定时检测class的时间戳变换<br/>

**使用场景：**<br/>

 1. 热部署更多的是在生产环境使用
 2. 热加载则更多的是在开发环境使用
热加载直接修改Java虚拟机中的字节码文件，难以监控和控制<br/>
热加载有个通俗的名字就是开发者模式<br/>

### 热部署原理解析
**Java类的加载过程**<br/>
初始化JVM -> 产生启动类加载器 -> (子类，自动加载)标准扩展类加载器->(子类，自动加载)系统类加载器->加载class文件(交给其父类加载)
**类加载的五个阶段**<br/>
加载->验证->准备->解析->初始化
**Java类加载器特点**<br/>

 1. 由AppClass Loader（系统类加载器）开始加载指定的类
 2. 类加载器将加载任务交给其父，如果其父找不到，再由自己去加载
 3. Bootstrap Loader（启动类加载器）是最顶级的类加载器

**Java类的热部署：**<br/>

 1.<font color="red"> 类的热加载</font>
 2. 配置Tomcat

**通过类的热加载实现热部署**<br/>

    @Override
    protected Class<?> findClass(String name)throws ClassNotFoundException{
    System.out.println("加载类==="+name);
    byte[] data=loadClassData(name);
    return this.defineClass(name,data,0,data.length);
    }
    
**通过配置Tomcat实现热部署**<br/>

 1. 直接把项目web文件夹放在webapps里
 2. 在tomcat\conf\server.xml中的<host></host>内部添加<context/>标签
 3. 在%tomcat_home%\conf\Catalina\localhost中添加一个XML
 （服务器会使用xml的名字作为path路径）
 

### Java类热加载案例分析
写一个Java类热加载的实际案例
要求：

 1. 类层次结构清晰、修改某一个Java类文件不需要重启服务或者重新编译运行程序
 2. 可适当的运用一些设计模式使代码结构更加清晰明了，比如工厂模式等。
 
实例源码：https://github.com/nilzxq/classLoader.git





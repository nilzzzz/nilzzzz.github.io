---
layout: post
title: spring boot入门hello world
tag: SpringBoot
---


### SpringBoot简介
[Spring Boot 百度百科][1]

> Spring Boot让我们的Spring应用变的更轻量化。比如：你可以仅仅依靠一个Java类来运行一个Spring引用。你也可以打包你的应用为jar并通过使用java-jar来运行你的Spring Web应用。

Spring Boot的主要优点： 
 1. 为所有Spring开发者更快的入门
 2. 开箱即用，提供各种默认配置来简化项目配置
 3. 内嵌式容器简化Web项目
 4. 没有冗余代码生成和XML配置的要求


 
### 一、系统要求

 1. Java 7及以上
 2. Spring Framework 4.1.5及以上
 3. 本文采用Java 1.8.0_73、Spring Boot 1.3.2调试通过。


### 二、快速入门
#### 2.1 创建一个Maven工程
名为”springboot-helloworld” 类型为<font color="red">Jar</font>工程项目

#### 2.2 pom文件引入依赖
   

     <parent>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-parent</artifactId>
    		<version>1.3.3.RELEASE</version>
    	</parent>
    	<dependencies>
    	  <!—SpringBoot web 组件 -->
    		<dependency>
    			<groupId>org.springframework.boot</groupId>
    			<artifactId>spring-boot-starter-web</artifactId>
    		</dependency>
    	</dependencies>



**spring-boot-starter-parent作用**<br/>
在pom.xml中引入spring-boot-start-parent,它可以提供dependency management,也就是说依赖管理，引入以后在申明其它dependency的时候就不需要version了，后面可以看到。<br/>
**spring-boot-starter-web作用**<br/>
springweb 核心组件<br/>
**spring-boot-maven-plugin作用**<br/>
 如果我们要直接Main启动spring，那么以下plugin必须要添加，否则是无法启动的。如果使用maven 的spring-boot:run的话是不需要此配置的。（我在测试的时候，如果不配置下面的plugin也是直接在Main中运行的。）<br/>
 
#### 2.3 编写HelloWorld服务
 
创建package命名为com.zxq.controller（根据实际情况修改)<br/>
创建HelloController类，内容如下

     @RestController
    @EnableAutoConfiguration
    public class HelloController {
    	@RequestMapping("/hello")
    	public String index() {
    		return "Hello World";
    	}	
    public static void main(String[] args) {
    		SpringApplication.run(HelloController.class, args);
    	}
    }
    
    
#### 2.4 @RestController
在上加上RestController 表示修饰该Controller所有的方法返回JSON格式,直接可以编写
Restful接口

#### 2.5 @EnableAutoConfiguration
注解:作用在于让 Spring Boot   根据应用所声明的依赖来对 Spring 框架进行自动配置<br/>
这个注解告诉Spring Boot根据添加的jar依赖猜测你想如何配置Spring。<br/>
由于spring-boot-starter-web添加了Tomcat和Spring MVC，所以auto-configuration将假定你正在开发一个web应用并相应地对Spring进行设置。<br/>
    
#### 2.6 SpringApplication.run(HelloController.class, args);
标识为启动类

#### 2.7 SpringBoot启动方式1
    Springboot默认端口号为8080
    @RestController
    @EnableAutoConfiguration
    public class HelloController {
    	@RequestMapping("/hello")
    	public String index() {
    		return "Hello World";
    }	
    public static void main(String[] args) {
    		SpringApplication.run(HelloController.class, args);
    	}
    }

启动主程序，打开浏览器访问http://localhost:8080/index，可以看到页面输出Hello World<br/>

  [1]: https://baike.baidu.com/item/Spring%20Boot/20249767?fr=aladdin
  
#### 2.8 SpringBoot启动方式2

@ComponentScan(basePackages = "com.zxq.controller")---控制器扫包范围

    @ComponentScan(basePackages = "com.itmayiedu.controller")
    @EnableAutoConfiguration
    public class App {
    	public static void main(String[] args) {
    		SpringApplication.run(App.class, args);
    	}
    }


### 三、Web开发
#### 3.1、静态资源访问
在我们开发Web应用的时候，需要引用大量的js、css、图片等静态资源。<br/>
默认配置<br/>
Spring Boot默认提供静态资源目录位置需置于classpath下，目录名需符合如下规则：<br/>
/static<br/>
/public<br/>
/resources<br/>
/META-INF/resources<br/>
举例：我们可以在src/main/resources/目录下创建static，在该位置放置一个图片文件。启<br/>动程序后，尝试访问http://localhost:8080/D.jpg。如能显示图片，配置成功。<br/>

#### 3.2、全局捕获异常
@ExceptionHandler 表示拦截异常<br/>
@ControllerAdvice 是 controller 的一个辅助类，最常用的就是作为全局异常处理的切面类<br/>
@ControllerAdvice 可以指定扫描范围<br/>
@ControllerAdvice 约定了几种可行的返回值，如果是直接返回 model 类的话，需要使用<br/> @ResponseBody 进行 json 转换<br/>
返回 String，表示跳到某个 view<br/>
返回 modelAndView<br/>
返回 model + @ResponseBody<br/>

    @ControllerAdvice
    public class GlobalExceptionHandler {
    	@ExceptionHandler(RuntimeException.class)
    	@ResponseBody
    	public Map<String, Object> exceptionHandler() {
    		Map<String, Object> map = new HashMap<String, Object>();
    		map.put("errorCode", "101");
    		map.put("errorMsg", "系統错误!");
    		return map;
    	}
    }
    
#### 3.3、渲染Web页面

渲染Web页面<br/>
在之前的示例中，我们都是通过@RestController来处理请求，所以返回的内容为json对象。那么如果需要渲染html页面的时候，要如何实现呢？<br/>
模板引擎<br/>
在动态HTML实现上Spring Boot依然可以完美胜任，并且提供了多种模板引擎的默认配置支持，所以在推荐的模板引擎下，我们可以很快的上手开发动态网站。<br/>
Spring Boot提供了默认配置的模板引擎主要有以下几种：<br/>

 - Thymeleaf
 - FreeMarker
 - Velocity
 - Groovy
 - Mustache

> Spring Boot建议使用这些模板引擎，避免使用JSP，若一定要使用JSP将无法实现Spring
> Boot的多种特性，具体可见后文：支持JSP的配置
> 当你使用上述模板引擎中的任何一个，它们默认的模板配置路径为：src/main/resources/templates。当然也可以修改这个路径，具体如何修改，可在后续各模板引擎的配置属性中查询并修改。


#### 3.4、使用Freemarker模板引擎渲染web视图
##### 3.4.1、pom文件引入：

    <!-- 引入freeMarker的依赖包. -->
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>
    
##### 3.4.2、后台代码
在src/main/resources/创建一个templates文件夹,后缀为*.ftl<br/>

    @RequestMapping("/index")
    	public String index(Map<String, Object> map) {
    	    map.put("name","美丽的天使...");
    		return "index";
    	}
    	
##### 3.4.1、前台代码

    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8" />
    <title></title>
    </head>
    <body>
    	  ${name}
    </body> 
    </html>

##### 3.4.4、Freemarker其他用法

    @RequestMapping("/index")
    	public String index(Map<String, Object> map) {
    	    map.put("name","###zxq###");
    	    map.put("sex",1);
      List<String> userlist=new ArrayList<String>();
    	    userlist.add("zxq");
    	    userlist.add("张三");
    	    userlist.add("李四");
    	    map.put("userlist",userlist);
    	    return "index";
    	}
    	
        	
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8" />
    <title>首页</title>
    </head>
    <body>
    	  ${name}
    	  <#if sex==1>
                男
          <#elseif sex==2>
                女
         <#else>
            其他      
    	  
    	  </#if>
    	  
    	 <#list userlist as user>
    	   ${user}
    	 </#list>
    </body> 
    </html>


##### 3.4.5、Freemarker配置

新建application.properties文件<br/>

    ########################################################
    ###FREEMARKER (FreeMarkerAutoConfiguration)
    ########################################################
    spring.freemarker.allow-request-override=false
    spring.freemarker.cache=true
    spring.freemarker.check-template-location=true
    spring.freemarker.charset=UTF-8
    spring.freemarker.content-type=text/html
    spring.freemarker.expose-request-attributes=false
    spring.freemarker.expose-session-attributes=false
    spring.freemarker.expose-spring-macro-helpers=false
    #spring.freemarker.prefix=
    #spring.freemarker.request-context-attribute=
    #spring.freemarker.settings.*=
    spring.freemarker.suffix=.ftl
    spring.freemarker.template-loader-path=classpath:/templates/
    #comma-separated list
    #spring.freemarker.view-names= # whitelist of view names that can be resolved


#### 3.5、使用JSP渲染Web视图

##### 3.5.1、pom文件引入以下依赖

    <parent>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-parent</artifactId>
    		<version>1.3.3.RELEASE</version>
    	</parent>
    	<dependencies>
    		<!-- SpringBoot 核心组件 -->
    		<dependency>
    			<groupId>org.springframework.boot</groupId>
    			<artifactId>spring-boot-starter-web</artifactId>
    		</dependency>
    		<dependency>
    			<groupId>org.springframework.boot</groupId>
    			<artifactId>spring-boot-starter-tomcat</artifactId>
    		</dependency>
    		<dependency>
    			<groupId>org.apache.tomcat.embed</groupId>
    			<artifactId>tomcat-embed-jasper</artifactId>
    		</dependency>
    	</dependencies>
    	
    	
##### 3.5.2、在application.properties创建以下配置

    spring.mvc.view.prefix=/WEB-INF/jsp/
    spring.mvc.view.suffix=.jsp
    
##### 3.5.3、后台代码

    @Controller
    public class IndexController {
    	@RequestMapping("/index")
    	public String index() {
    		return "index";
    	}
    }
    
    
    
### 巴拉巴拉
单身狗又穷又没颜<br/>
多敲代码多看报<br/>
恩，剁完手更有力气敲代码了，天啊撸<br/>
没控制自己，又看了一本课外书(专业书能不能看看啦？？？？？)，余华的《第七天》，第一次阅读余华的书，总听别人推荐《活着》，以后一定拜读。喜欢上这个作者了，通透哲思。<br/>
推荐歌曲《小蓝》，恩~没单曲循环多久大概五六天吧，这首歌的故事送给你，剩下的感悟自己揣着~~~<br/>



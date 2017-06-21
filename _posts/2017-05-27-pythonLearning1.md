---
layout: post
title: 爬虫
tag: python
---

### 爬虫简介

![此处输入图片的描述][2]
![此处输入图片的描述][3]
### 简单爬虫架构
![此处输入图片的描述][4]
### 简单爬虫架构-运行流程
![此处输入图片的描述][5]
### URL管理器
URL管理器：管理待抓取URL集合和已抓取URL集合
--防止重复抓取、防止循环抓取
![此处输入图片的描述][6]
![此处输入图片的描述][7]
### 网页下载器
网页下载器：将互联网上URL对应的网页下载到本地的工具
网页下载器会将URL对应的互联网网页以HTML的形式下载到本地，存储成一个本地文件或内存字符串

Python有哪几种网页下载器？
urllib2--python官方基础模块，支持直接URL下载，支持登录网页的cookie出来、代理处理等
requests--第三方包更强大

### 网页下载器--urllib2
urllib2下载网页方法1：最简洁方法
```
import urllib2
#直接请求
response=urllib2.urlopen('http://www.baidu.com')

#获取状态码，如果是200表示获取成功
print response.getcode()

#读取内容
cont=response.read()
```
urllib2下载网页方法2：添加data、http header
![此处输入图片的描述][8]
```
import urllib2

#创建Request对象
request=urllib2.Request(url)

#添加数据
request.add_data('a','1')

#添加http的header
request.add_header('User-Agent','Mozilla/5.0')

#发送请求获取结果
response=urllib2.urlopen(request)
```

urllib2下载网页方法3：添加特殊情景的处理器
例如：需要cookie
![此处输入图片的描述][9]
```
import urllib2,cookielib
#创建cookie容器
cj=cookielib.CookieJar()

#创建1个opener
opener=urllib2.build_opener(urllib2.HTTPCookieProcessor(cj))

#给urllib2安装opener
urllib2.install_opener(opener)

#使用带有cookie的urllib2访问网页
response=urllib2.urlopen("http://www.baidu.com/")
```
### 网页解析器
![此处输入图片的描述][10]
![此处输入图片的描述][11]
![此处输入图片的描述][12]

### 网页解析器-Beautiful Soup
Beautiful Soup
--Python第三方库用于从HTML或XML中提取数据
--官网：https://www.crummy.com/software/BeautifulS
<br/>oup/bs4/doc/index.zh.html

创建BeautifulSoup对象
```
from bs4 import BeautifulSoup
#根据HTML网页字符串创建BeautifulSoup对象
soup=BeautifulSoup(
                  html_doc,             #HTML文档字符串
                  'html.parser'         #HTML解析器
                  from_encoding='utf8'  #HTML文档的编码
)
```
搜索节点(find_all,find)
```
方法：find_all(name,attrs,string)
#查找所有标签为a的节点
soup.find_all('a')

#查找所有标签为a,链接符合/view/123.htm形式的节点
soup.find_all('a',href='/view/123.htm')
soup.find_all('a',href=re.compile(r'/view/\d+\.htm'))

#查找所有标签为div，class为abc，文字为Python的节点
soup.find_all('div',class_='abc',string='Python')
```

访问节点信息
```
#得到节点：<a href='1.html'>Python</a>
#获取查找到的节点的标签名称
node.name
#获取查找到的a节点的href属性
node['href']
#获取查找到的a节点的链接文字
node.get_text()
```

### BeautifulSoup实例测试
测试环境：eclipse+pydev+python2.7
配置中遇到的问题及解决办法：http://blog.csdn.net/xue_changkong<br/>
/article/details/46755639
```
#coding:utf8
import re
from bs4 import BeautifulSoup
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
soup=BeautifulSoup(html_doc,'html.parser',from_encoding='utf8')
print '获取所有的链接'
links=soup.find_all('a')
for link in links:
    print link.name,link['href'],link.get_text()

print '获取lacie的链接'
link_node=soup.find('a',href="http://example.com/lacie")
print link_node.name,link_node['href'],link_node.get_text()

print '正则匹配'
link_node=soup.find('a',href=re.compile(r"ill"))
print link_node.name,link_node['href'],link_node.get_text()

print '获取p段落文字'
p_node=soup.find('p',class_="title")
print p_node.name,p_node.get_text()
```

测试结果：

> 获取所有的链接 a http://example.com/elsie Elsie a http://example.com/lacie
> Lacie a http://example.com/tillie Tillie 获取lacie的链接 a
> http://example.com/lacie Lacie 正则匹配 a http://example.com/tillie Tillie
> 获取p段落文字 p The Dormouse's story


  
  [2]: http://omztq7zo1.bkt.clouddn.com/%E7%88%AC%E8%99%AB%E7%AE%80%E4%BB%8B.png
  [3]: http://omztq7zo1.bkt.clouddn.com/%E7%88%AC%E8%99%AB%E7%AE%80%E4%BB%8B2.png
  [4]: http://omztq7zo1.bkt.clouddn.com/%E7%88%AC%E8%99%AB%E6%9E%B6%E6%9E%84.png
  [5]: http://omztq7zo1.bkt.clouddn.com/%E8%BF%90%E8%A1%8C%E6%B5%81%E7%A8%8B.png
  [6]: http://omztq7zo1.bkt.clouddn.com/URL%E7%AE%A1%E7%90%86%E5%99%A8.png
  [7]: http://omztq7zo1.bkt.clouddn.com/url%E7%AE%A1%E7%90%86%E5%99%A8%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F.png
  [8]: http://omztq7zo1.bkt.clouddn.com/urllib2-2.png
  [9]: http://omztq7zo1.bkt.clouddn.com/urllib2-3.png
  [10]: http://omztq7zo1.bkt.clouddn.com/%E7%BD%91%E9%A1%B5%E8%A7%A3%E6%9E%90%E5%99%A8.png
  [11]: http://omztq7zo1.bkt.clouddn.com/%E7%BD%91%E9%A1%B5%E8%A7%A3%E6%9E%90%E5%99%A82.png
  [12]: http://omztq7zo1.bkt.clouddn.com/%E7%BD%91%E9%A1%B5%E8%A7%A3%E6%9E%90%E5%99%A83.png
---
layout: post
title: python爬虫基础之urllib包
tag: 爬虫
---


### urllib包

> python标准库中的socket模块主要应用于底层网络协议，编写程序要从底层开始构建，对于大多数程序员来说，网络编程都是针对应用协议进行的。

> urllib面向HTTP协议，而网络上的网站应用都是基于HTTP协议的。urllib主要处理URL，使用urllib操作URL可以像使用和打开本地文件一样的操作，非常简单。

    #encoding:utf-8
    import urllib.request
    print(urllib.request.urlopen('http://www.baidu.com').read().decode('UTF-8'))

### urllib包的主要模块

| 模块       |  解释 | 
| --------   | -----:  | 
| urllib.request | 用于打开URL网址|
| urllib.error  | 定义了常见的urllib.request会引发的异常|
| urllib.parse          | 用于解析URL|
| urllib.robotparser  | 用于解析robots.txt|

> robots.txt（统一小写）是一种存放于网站根目录下的ASCII编码的文本文件，它通常告诉网络搜索引擎的漫游器（又称网络蜘蛛），此网站中的哪些内容是不能被搜索引擎的漫游器获取的，哪些是可以被（漫游器）获取的。
> 因为一些系统中的URL是大小写敏感的，所以robots.txt的文件名应统一为小写。robots.txt应放置于网站的根目录下
> 
> urllib.request模块中提供了用于打开一个URL的函数urlopen()
> 
> urllib.urlopen(url[, data[, proxies]])
> :将返回一个HTTPResponse实例（类文件对象），可以像操作文件一样使用read,readline,close等方法对URL进行操作
> 
> 参数url表示远程数据的路径，一般是网址；参数data表示以post方式提交到url的数据
> 参数proxies用于设置代理。urlopen返回 一个类文件对象，它提供了如下方法：read() , readline() ,
> readlines() , close()
> ：这些方法的使用方式与文件对象完全一样;info()：返回一个httplib.HTTPMessage
> 对象，表示远程服务器返回的头信息getcode()：返回Http状态码。如果是http请求，200表示请求成功完成;404表示网址未找到；geturl()：返回请求的url；
> 
> urllib.request模块中的urlretrieve方法直接将远程数据下载到本地。 urllib.
> request.urlretrieve(url[, filename[, reporthook[,
> data]]])参数说明：url：外部或者本地urlfilename：指定了保存到本地的路径（如果未指定该参数，urllib会生成一个临时文件来保存数据）；reporthook：是一个回调函数，当连接上服务器、以及相应的数据块传输完毕的时候会触发该回调。我们可以利用这个回调函数来显示当前的下载进度。data：指post到服务器的数据。该方法返回一个包含两个元素的元组(filename,
> headers)，filename表示保存到本地的路径，header表示服务器的响应头。



```
import urllib.request 
import re    
import os    
import traceback #打印异常信息
targetDir = r"D:\Pic"  #文件保存路径  
def destFile(path):    
    if not os.path.isdir(targetDir):    
        os.mkdir(targetDir) #如果路径不存在,则生成文件路径
    pos = path.rindex('/')#搜索最后一个/的位置    
    t = os.path.join(targetDir, path[pos+1:])#将图片存储路径和图片连接,生成绝对存储路径    
    return t    
if __name__ == "__main__":  #程序运行入口  
    weburl = "https://www.douban.com/"#爬豆瓣首页
    webheaders = {'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:23.0) Gecko/20100101 Firefox/23.0'}   
    req = urllib.request.Request(url=weburl, headers=webheaders)  #构造请求报头  
    webpage = urllib.request.urlopen(req)  #发送请求报头  
    contentBytes = webpage.read()    
    sLogFile=open("D:\Pic\Log.txt",'a');#open with appending    
    for link in set(re.findall(r'((http|https):[^\s]*?(jpg|png|gif))', str(contentBytes))):  #正则表达式查找所有的图片  
        print(link)
        try:   
            sLink=link[0];
            urllib.request.urlretrieve(sLink, destFile(sLink)) #下载图片
        except:
            traceback.print_exc(file=sLogFile);#打印错误信息                     
            print('失败') #异常抛出  
    sLogFile.close();
```

### urllib.parse模块有一个可以对URL进行编码的函数urlencode()

```
from urllib.parse import urlencode
data={'a':'data','name':'数据'}
print(urlencode(data))


a=data&name=%E6%95%B0%E6%8D%AE
```

```
from urllib.parse import unquote
print(unquote('a=data&name =%E6%95%B0%E6%8D%AE '))


a=data&name =数据 
```



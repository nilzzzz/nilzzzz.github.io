---
layout: post
title: 实战演练：爬取百度百科1000个页面的数据
tag: python
---

### 实例爬虫-分析目标

步骤
 1. 确定目标：确定抓取哪个网站的哪些网页的哪部分数据。本实例确定抓取百度百科python词条页面以及它相关的词条页面的标题和简介。
 2. 分析目标：确定抓取数据的策略。一是分析要抓取的目标页面的URL格式，用来限定要抓取的页面的范围；二是分析要抓取的数据的格式，在本实例中就是要分析每一个词条页面中标题和简介所在的标签的格式；三是分析页面的编码，在网页解析器中指定网页编码，才能正确解析。
 3. 编写代码：在解析器中会使用到分析目标步骤所得到的抓取策略的结果。
 4. 执行爬虫。<br/>
**目标**：百度百科Python词条相关词条网页-标题和简介<br/>
**入口页**:http://baike.baidu.com/item/Python<br/>
**URL格式**：<br/>
-词条页面URL：/item/Python<br/>
**数据格式**：<br/>
标题：<br/>

```
<dd class="lemmaWgt-lemmaTitle-title"><h1>***</h1></dd>
```

简介：

```
<div class="lemma-summary">***</div>
```

### 实例代码

1、spider_main.py

```
#coding:utf8
import url_manager,html_downloader,html_outputer,html_parser

class SpiderMain(object):
    def __init__(self):
        self.urls = url_manager.UrlManager()
        self.downloader = html_downloader.HtmlDownloader()
        self.parser = html_parser.HtmlParser()
        self.outputer = html_outputer.HtmlOutputer()

    def craw(self,root_url):
        count = 1
        self.urls.add_new_url(root_url)        
        while self.urls.has_new_url():
            try:
                new_url = self.urls.get_new_url()
                print "craw %d:%s"%(count,new_url)
                html_content = self.downloader.download(new_url)
                new_urls,new_data = self.parser.parse(new_url,html_content)
                self.urls.add_new_urls(new_urls)
                self.outputer.collect_data(new_data)
                if count == 100:
                    break
                count = count + 1
            except Exception as e:
                        print str(e)
                        print "craw failed"

        self.outputer.output_html()

if __name__=="__main__":
    root_url = "http://baike.baidu.com/item/Python"
    obj_spider = SpiderMain()
    obj_spider.craw(root_url)
```

2、html_downloader.py

```
import urllib2

class HtmlDownloader(object):
    def download(self,url):
        if url is None:
            return

        response = urllib2.urlopen(url)
        if response.getcode() != 200:
            return None        
        return response.read()
```

3、html_parser.py

```
#encoding:utf8
from bs4 import  BeautifulSoup
import re
import urlparse

class HtmlParser(object):
    def parse(self,page_url,html_content):
        if page_url is None or html_content is None:
            return

        soup = BeautifulSoup(html_content,"html.parser",from_encoding="utf-8")
        new_urls = self._get_new_urls(page_url,soup)

        new_data = self._get_new_data(page_url,soup)

        return new_urls,new_data

    def _get_new_urls(self,page_url,soup):
        new_urls = set()
        links = soup.find_all('a',href=re.compile(r"/item/.+"))
        for link in links:
            new_url = link['href']
           # new_full_url = urlparse.urljoin(page_url,new_url)
            new_full_url = r"http://baike.baidu.com%s"%new_url
            new_urls.add(new_full_url)
        return new_urls

    def _get_new_data(self,page_url,soup):
        #res_data = set() 
        res_data = {}           
        res_data["url"] =  page_url

        #标题  <dd class="lemmaWgt-lemmaTitle-title"> <h1>Python</h1>
        title_node = soup.find('dd',class_="lemmaWgt-lemmaTitle-title").find("h1")
        res_data["title"] = title_node.get_text()

        #简介        
        summary_node = soup.find('div',class_="lemma-summary")
        res_data["summary"] = summary_node.get_text()   

        return res_data
```

4、url_manager.py

```
class UrlManager(object):
    def __init__(self):
        self.new_urls = set()
        self.old_urls = set()

    def add_new_url(self,url):      
        if url is None:
            return None
        if url not in self.new_urls and url not in self.old_urls:
            self.new_urls.add(url)

    def add_new_urls(self,urls):
        if urls is None or len(urls)==0:
            return
        for url in urls:
            self.new_urls.add(url)

    def get_new_url(self):
        new_url = self.new_urls.pop()
        self.old_urls.add(new_url)
        return new_url

    def has_new_url(self):
        return len(self.new_urls)!=0
```

5、html_outputer.py

```
class HtmlOutputer(object):
    def __init__(self):
        self.datas = []

    def collect_data(self,data):
        if data is None:
            return
        self.datas.append(data)

    def output_html(self):
        fout = open("output.html","w")
        fout.write("<html>")
        fout.write("<body>")
        fout.write("<table>")
        for data in self.datas:
            fout.write("<tr>")
            fout.write(r"<td width='25%%'>%s</td>"%data["url"])
            fout.write("<td width='5%%'>%s</td>"%data["title"].encode("utf-8"))
            fout.write("<td width='70%%'>%s</td>"%data["summary"].encode("utf-8"))
            fout.write("</tr>")

        fout.write("</table>")
        fout.write("</body>")
        fout.write("</html>")
```

### 测试结果
1000个测试数据部分
![此处输入图片的描述][1]
在项目目录下右键或F5刷新，可以看到output.html,再在此HTML右键选择properti看到项目路径，在浏览器中打开
![此处输入图片的描述][2]
![此处输入图片的描述][3]
**页面编码：UTF-8**
### 问题
一个网页可以有不一样的url吗？<br/>
python百度百科入口页：http://baike.baidu.com/link?url=Wf76vvoFrt-HJxx4hGR41WI0k-XmCuqVt7yRvY_37j2QELttcE11FsSDfFfadcPeflMaBjdB_FC5_YX3uxHZUK
数字的入口页：http://baike.baidu.com/view/21087.htm<br/>
现在用的是这个网址http://baike.baidu.com/item/Python<br/>
也能打开词条<br/>
[更多问题请自行imooc](http://www.imooc.com/learn/563),改不出BUG很烦，但是改完后超爽有木有！！！！
### 巴拉巴拉
> 这个端午假期，很享受。这学期到这个假期之前一直忙于练车，虽然认识了不少朋友（我才不会告诉你他们有多可爱，我们教练有多逗），但是练车看似很好玩，但是这一阶段下来还是费了不少心力，学习时间被分割了不少，耽误了清明假期，五一假期还好有个端午（知足常乐）。
> 爬虫之路很长，学到东西很开心，但是想走的远任重而道远（老师的要求我看我有点儿费劲去达到），开学又不知道会忙些什么乱七八糟的，假期时间自由的感觉好爽，学习到东西真的会让人踏实不少（烦心的事儿很多，敬告自己分轻重）。

  [1]: http://omztq7zo1.bkt.clouddn.com/%E7%88%AC%E8%99%AB%E6%B5%8B%E8%AF%95%E7%BB%93%E6%9E%9C.png
  [2]: http://omztq7zo1.bkt.clouddn.com/%E7%88%AC%E8%99%AB%E6%B5%8B%E8%AF%95%E7%BB%93%E6%9E%9C2.png
  [3]: http://omztq7zo1.bkt.clouddn.com/%E7%88%AC%E8%99%AB%E6%B5%8B%E8%AF%95%E7%BB%93%E6%9E%9C1.png
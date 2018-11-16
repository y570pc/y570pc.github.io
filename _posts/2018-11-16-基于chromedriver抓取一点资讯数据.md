---
layout: post
title:  "基于selenium抓取一点资讯数据"
category: 'bigdata'
tags: 爬虫 selenium chromedriver
author: y570pc
image: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539353525085&di=fa2704dfc094554b4c5d295b83751d81&imgtype=0&src=http%3A%2F%2Fpic.58pic.com%2F58pic%2F15%2F17%2F79%2F24V58PICTuB_1024.jpg"
---

## 说明

#### lxml库

`/`：从当前节点选取直接子节点

`//`：从当前节点选取直接子节点

`@`：选取属性

`[@attrib='value']`：选取给定属性且具有给定值的所有元素

`text()`：获取节点中的文本

#### selenium使用

`driver.refresh()`：刷新页面

`driver.execute_script("window.scrollBy(0, 800)")`：下拉滚动条，相对当前的坐标移动800像素

## 实操

#### chromedriver

下载对应chrome版本的chromedriver，解压后放入`D:\chromedriver\`文件夹下。

#### 代码

```python
from selenium import webdriver
from selenium.webdriver.common import keys
from selenium.webdriver.common import by
import lxml.html
import time,os,csv

chromedriver = 'D:\chromedriver\chromedriver.exe'
os.environ["webdriver.chrome.driver"] = chromedriver
driver = webdriver.Chrome(chromedriver)
#打开一点资讯网页
driver.get('https://www.yidianzixun.com/channel/m1476422')
#因为这个网页不是响应式的，所以先最大化，才能加载出旁边的刷新按钮
driver.maximize_window()
#等待3秒
driver.implicitly_wait(3)
#根据class找到刷新按钮，点击刷新按钮，获取最新资讯
driver.find_element_by_css_selector(".item.refresh.icon.iconfont.icon-refresh.anim").click()
#执行多次页面下拉脚本
for i in range(0,6):
    driver.execute_script("window.scrollBy(0,200)","")
    time.sleep(3)

doc = lxml.html.fromstring(driver.page_source)

#文章标题
title = doc.xpath('//div[@class="doc-title"]/text()')

#作者
author = doc.xpath('//span[@class="source"]/text()')

#评论数
comment = doc.xpath('//span[@class="comment-count"]/text()')

#评论数
data = doc.xpath('//span[@class="source"]/text()')

#图片

#链接
link_temp = doc.xpath('//div[@class="channel-news channel-news-0"]/a/@href')
link=[]
for i in range(len(link_temp)):
    link.append("https://www.yidianzixun.com"+link_temp[i])

#结果汇总
result = []
for i in range(len(title)):
    dic = {'title':title[i],'author':author[i],'comment':comment[i],'link':link[i]}
    result.append(dic)

#表头
headers = ['title','author','comment','link']
with open('f:\\yidianzixun.csv','w')  as f:
    f = csv.DictWriter(f,headers, delimiter=',', lineterminator='\n')   #lineterminator='\n'解决输出空行的问题
    f.writeheader()
    f.writerows(result)
print('ok')
```

## 参考资料

[1]. [python3解析库lxml](http://www.cnblogs.com/zhangxinqi/p/9210211.html)
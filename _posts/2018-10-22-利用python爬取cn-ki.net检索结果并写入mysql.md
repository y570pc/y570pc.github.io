---
layout: post
title:  "利用python爬取cn-ki.net检索结果并写入mysql"
category: 'bigdata'
tags: 爬虫 mysql 关键字
introduction: 
image: 
---


## 准备

#### 所需环境

* mysql-5.6.41
* mysql-connector-python-8.0.12-py3.6

#### 更改字符编码格式

当出现`ERROR 1366 (HY000):Incorrect string value`时，需将字符编码格式统一设置为gbk格式。

```
MYSQL>set character_set_client = 'gbk' ;
MYSQL>set character_set_connection = 'gbk' ;
MYSQL>set character_set_results= 'gbk' ;
MYSQL>set character_set_server= 'gbk' ;
MYSQL>set character_set_database= 'gbk' ;
```

#### 创建表格

```
CREATE DATABASE test01;
USE test01;
CREATE TABLE `lunwen` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(100) DEFAULT NULL,
  `Abstract` varchar(1000) DEFAULT NULL,
   PRIMARY KEY (`id`)
) ;
```

## 代码

{% highlight python%}
import requests
import mysql.connector
from lxml import html


KeyWords = '人工智能'       # 搜索关键词
MaxPage = 1                 # 爬取的页面数目
URL = 'https://search.ehn3.com/'

Num_Paper = 0

data = {
    'keyword': KeyWords,
    'db': 'SCDB'
}


def get_html(url, para_data):
    content = requests.get(url, params=para_data)
    return content

# 链接mysql数据库
conn = mysql.connector.connect(user='root', password='LLte6628', database='test01')
cur = conn.cursor()

content = get_html(URL+'search', data)
page_url = content.url

page_ii = 0
while page_ii < MaxPage:
    content = get_html(page_url, '')
    tree = html.fromstring(content.text)

    e1 = tree.xpath('//div[@class="mdui-col-xs-12 mdui-col-md-9 mdui-typo"]')
    for ei in e1:
        e2_title = ei.xpath('h3/a/text()')
        title = ''.join(e2_title)

        e2_author = ei.xpath('div[1]/span[1]/text()')
        author = ''.join(e2_author)
        e2_JnName = ei.xpath('div[1]/span[3]/text()')
        JnName = ''.join(e2_JnName)
        e2_JnVol = ei.xpath('div[1]/span[4]/text()')
        JnVol = ''.join(e2_JnVol)
        e2_JnType = ei.xpath('div[1]/span[5]/text()')
        JnType = ''.join(e2_JnType)

        e2_href = ei.xpath('h3/a/@href')
        href = ''.join(e2_href)
        if URL not in href:
            href = URL + href

        Jn_content = get_html(href, '')
        Jn_tree = html.fromstring(Jn_content.text)

        # 这是之前的代码
        # e2_abstract = Jn_tree.xpath('//div[@class="mdui-col-md-11 mdui-col-xs-9 mdui-text-color-black-text"]/p/text()')
        # 修改后的代码
        e2_abstract = Jn_tree.xpath('//div[@class="mdui-col-xs-12 mdui-text-color-black-text mdui-typo"]/p/text()')

        abstract = (''.join(e2_abstract)).strip()

        print('********************' + href)

        print('title: >>>>>>>>>>> %s' % title)
        print('href: ----------- %s' % href)
        print('author: >>>>>>>>>>> %s' % author)
        print('JnName: ---- %s ---- %s ---- %s' % (JnName, JnVol, JnType))
        print('>>>>>>>>>>> Abstract <<<<<<<<<<<')
        print('%s' % abstract)

        # 保存数据记录
        cur.execute('insert into lunwen (title, Abstract) VALUES (%s, %s)',
                   [ title, abstract])

        Num_Paper += 1

    page_ii += 1

    nextpg = tree.xpath('//div[@class="mdui-col-xs-9 TitleLeftCell mdui-valign"]/a[last()]')
    if nextpg.__len__() == 0:
        break
    nextpg_text = nextpg[0].text
    if nextpg_text == '下一页':
        page_url = nextpg[0].attrib['href']
    else:
        break

    if URL not in page_url:
        page_url = URL + page_url

print('****************************************************************************')
print('>>>>>>>>>>>>>>>>  数据导出结束，共导出 %d 篇文献！<<<<<<<<<<<<<<<<<<<<<<<<<<' % Num_Paper)

# 关闭数据库
conn.commit()
cur.close()
{% endhighlight %}

## 参考资料

[1]. [ERROR 1366 (HY000):Incorrect string value解决方案（亲测）](https://blog.csdn.net/geilivablemental/article/details/45034229)

[2]. [CNKI网页爬虫](https://blog.csdn.net/huahua19891221/article/details/79190177)
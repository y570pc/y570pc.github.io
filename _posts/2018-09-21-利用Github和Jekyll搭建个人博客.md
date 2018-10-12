---
layout: post
title:  "利用Github和Jekyll搭建个人博客"
categories: 网站搭建
tags: jekyll markdown 
author: y570pc
---

* content
{:toc}

## 站在巨人的肩膀上
Fork [xudailong.github.io](https://github.com/xudailong/xudailong.github.io)（博客模板）到我的仓库（repository）。

![01](/img/2018-09-21-01.jpg)

到我的对应仓库，点击Settings，rename仓库名。

![02](/img/2018-09-21-02.jpg)

## 本地编辑
将仓库文件copy到本地。

{% highlight js%}
git clone https://github.com/y570pc/y570pc.github.io
{% endhighlight %}

其中本地_posts文件夹下的md文件即为创建的博文，用mardown语言编写而成。显然md文件的编写难度远小于html文件的编写难度。而Jekyll可将md文件转变成html文件。

将本地文件push到github仓库
{% highlight js%}
git commit -m "message”
git push -u origin master
{% endhighlight %}

## 搭建本地调试环境
1. 安装Ruby 
2. 安装RubyGems
3. 用RubyGems安装Jekyll
4. 开启服务器，在本地仓库打开cmd，输入jekyll serve
5. 浏览器访问http://localhost:4001/ 


## 样式微调
#### 代码高亮
* 安装pygments
{% highlight ruby%}
gem install pygments.rb
{% endhighlight %}

* 修改配置文件_config.yml
{% highlight js%}
//修改前
markdown: kramdown
kramdown:
  input: GFM
  syntax_highlighter: rouge
  
//修改后
markdown: kramdown
highlighter: pygments
{% endhighlight %}

* md文件实现代码高亮的语法
![03](/img/2018-09-21-03.jpg)

* 显示效果
{% highlight python%}
import speech_recognition as sr
r = sr.Recognizer()
harvard = sr.AudioFile('harvard.wav')
with harvard as source:
    audio =r.record(source)
text = r.recognize_ibm(audio, username='b1c2ce4f-1420-4f49-82c5-ed73cfb320ec', password='BMLcTYAuPCdA', language='zh-CN')
{% endhighlight %}

#### 博文中添加图片
* 在本地仓库的根目录下新建图片文件夹img。将所需的图片保存至该文件夹下。
![04](/img/2018-09-21-04.jpg)

* md文件引用图片的语法
{% highlight js%}
![03](/img/2018-09-21-03.jpg) //[]内可以任意取名，()内即为图片路径
{% endhighlight %}

#### 修改字体
* 在本地仓库的css文件夹下存放着main.scss文件。应该是修改全局样式的配置文件。对其进行修改。
{% highlight js%}
//修改前
base-font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  
//修改后
base-font-family: "Times New Roman",Georgia;
{% endhighlight %}

#### markdown中使用LaTeX
* 对_includes文件夹中的head.html进行如下修改。
{% highlight js%}
//修改前
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
    tex2jax: { inlineMath: [["$","$"],["\\(","\\)"]] },
    "HTML-CSS": {
      linebreaks: { automatic: true, width: "container" }
    }
});
</script>
<script type="text/javascript"
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

  
//修改后
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } }); </script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
             inlineMath: [ ['$','$'], ["\\(","\\)"] ],
             processEscapes: true
           }
         });
</script>
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
{% endhighlight %}

* 测试LaTeX功能
{% highlight js%}
//独段插入
$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$
  
//段内插入
公式$\exp(-\frac{x^2}{2})$
{% endhighlight %}

* 独段插入公式效果

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

* 段内插入公式效果：公式$\exp(-\frac{x^2}{2})$

#### 插入iframe
* 插入iframe格式
{% highlight html%}
<iframe src="https://music.163.com/outchain/player?type=3&id=794070604&auto=0&height=66" width="500px" height="80px" frameborder="0" scrolling="no"> </iframe>
{% endhighlight %}

* 插入效果展示
<iframe src="https://music.163.com/outchain/player?type=3&id=794070604&auto=0&height=66" width="500px" height="80px" frameborder="0" scrolling="no"> </iframe>

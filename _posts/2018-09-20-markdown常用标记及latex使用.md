---
layout: post
title: "markdown常用标记及latex使用"
image: 
category: 'website'
tags: markdown latex
---

## markdown

#### 标题

使用`# 一级标题`，显示一级标题

使用`## 二级标题`，显示二级标题

使用`&emsp;`，显示&emsp;缩进

#### 字体

使用`**粗体**`，显示**粗体**

使用`*斜体*`，显示*斜体*

使用`<font color="green">绿色</font>`，显示<font color="green">绿色</font>

使用`:smile:`，显示:smile:

#### 排版

换行使用`<br>`

全角空格使用`&emsp;`

#### 引用

使用

```
> 但愿在我看不到的天际 <br>
> 你张开了双翼 <br>
> 遇见你已注定 <br> 
> 她会有多幸运 
```

显示

> 但愿在我看不到的天际 <br>
> 你张开了双翼 <br>
> 遇见你已注定 <br> 
> 她会有多幸运 

#### 链接

使用`[百度](http://www.baidu.com)`，显示[百度](http://www.baidu.com)

使用`![图片](https://gitee.com/amius/learngit/raw/master/%E5%A4%B4%E5%83%8F.jpg)`,显示图片。

使用`[几种常见回归及scikit-learn实现]({{ y570pc.github.io }}{% link _posts/2018-10-24-几种常见回归及scikit-learn实现.md %})`，实现站内引用，效果[几种常见回归及scikit-learn实现]({{ y570pc.github.io }}{% link _posts/2018-10-24-几种常见回归及scikit-learn实现.md %})。

#### 列表

使用

```
* &hearts;列表
	* &hearts;字列表 \\需要输入\tab键
```

显示

* &hearts; 列表
	* &hearts; 字列表

使用

```
* [ ] 一次性水杯
* [x] 西瓜
* [ ] 豆浆
* [x] 可口可乐
* [ ] 小茗同学
```

显示

* [ ] 一次性水杯
* [x] 西瓜
* [ ] 豆浆
* [x] 可口可乐
* [ ] 小茗同学

#### 表格

* 方法一：
>使用

```
|星期|1|2|3|4|5|6|7|
|---|---|---|---|---|---|---|---|
|早餐|香蕉牛奶燕麦粥|皮蛋瘦肉粥|蜂蜜小蛋糕|灌汤包|南瓜饼|肉末蛋羹|豆浆油条|
|中餐|爆炒鸡肝|笋干炒肉|箩卜炒肉|剁椒鱼头|葱油蛏子|风味蹄筋|珍珠丸子|
|晚餐|牛肉砂锅|虾皮炒海带|牛肉炒西芹|芝麻豆腐|香菇炒肉|土豆丝饼|叉烧肉|
```

>显示

|星期|1|2|3|4|5|6|7|
|---|---|---|---|---|---|---|---|
|早餐|香蕉牛奶燕麦粥|皮蛋瘦肉粥|蜂蜜小蛋糕|灌汤包|南瓜饼|肉末蛋羹|豆浆油条|
|中餐|爆炒鸡肝|笋干炒肉|箩卜炒肉|剁椒鱼头|葱油蛏子|风味蹄筋|珍珠丸子|
|晚餐|牛肉砂锅|虾皮炒海带|牛肉炒西芹|芝麻豆腐|香菇炒肉|土豆丝饼|叉烧肉|

* 方法二：
>使用

```
<table align="center">
    <tr  bgcolor="#F6F8FA" >
        <th>底色</th>
		<th>底色</th>
    </tr>
    <tr>
        <td>
		    <ul>
			    <li>item1</li>
				<li>item2</li>
			</ul>
		</td>
		<td>灰色</td>
    </tr>
</table>
```

>显示

<table align="center">
    <tr  bgcolor="#F6F8FA" >
        <th>底色</th>
		<th>底色</th>
    </tr>
    <tr>
        <td>
		    <ul>
			    <li>item1</li>
				<li>item2</li>
			</ul>
		</td>
		<td>灰色</td>
    </tr>
</table>

#### 样式
<style>
  .purple {
    color:inherit;
  }
  .purple:hover {
    color:rgb(252,109,38);
  }
</style>
Hey! Hover the cursor over me and guess what?! :)
{: .purple}

#### 折叠
<div style="margin:0 auto;width:750px;">
<details>
  <summary>Question 1</summary>
  <p><strong>Pellentesque habitant morbi tristique</strong> senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. <em>Aenean ultricies mi vitae est.</em> Mauris placerat eleifend leo. Quisque sit amet est et sapien ullamcorper pharetra. Vestibulum erat wisi, condimentum sed, <code>commodo vitae</code>, ornare sit amet, wisi. Aenean fermentum, elit eget tincidunt condimentum, eros ipsum rutrum orci, sagittis tempus lacus enim ac dui. <a href="#">Donec non enim</a> in turpis pulvinar facilisis. Ut felis.</p>
  <details>
    <summary>Related documents</summary>
    <ul>
      <li><a href="#">Lorem ipsum dolor sit amet,  consectetuer adipiscing elit.</a></li>
      <li><a href="#">Aliquam tincidunt mauris eu  risus.</a></li>
      <li><a href="#">Lorem ipsum dolor sit amet,  consectetuer adipiscing elit.</a></li>
      <li><a href="#">Aliquam tincidunt mauris eu  risus.</a></li>
    </ul>
  </details>
</details>
</div>

#### 嵌入文档

<figure class="video_container">
<iframe src="https://caueducn-my.sharepoint.com/personal/y570pc_cau_edu_cn/_layouts/15/Doc.aspx?sourcedoc={484a9634-d30c-4dca-8421-3fb14eabaf87}&amp;action=embedview&amp;wdStartOn=1" width="476px" height="673px" frameborder="0">这是嵌入 <a target="_blank" href="https://office.com">Microsoft Office</a> 文档，由 <a target="_blank" href="https://office.com/webapps">Office Online</a> 支持。</iframe>
</figure>

#### 创建滑动条


<div style="margin:0 auto; background:#F8F8F8;border:1px solid black;width:800px;height:600px;overflow-y:scroll;overflow-x:scroll;text-align:left;">
<p>
#建立索引，为了减少占用的计算资源<br>
bowtie-build bowtie_index/mm10.fa bowtie_index/mm10<br>
#查看生成的结果，共6个文件<br>
ls -l bowtie_index/*.ebwt<br>
#结果<br>
-rw-r--r-- 1 y570pc y570pc 59026331 10月 25 19:57 bowtie_index/mm10.1.ebwt<br>
-rw-r--r-- 1 y570pc y570pc 23988656 10月 25 19:57 bowtie_index/mm10.2.ebwt<br>
-rw-r--r-- 1 y570pc y570pc      458 10月 25 19:55 bowtie_index/mm10.3.ebwt<br>
-rw-r--r-- 1 y570pc y570pc 47977298 10月 25 19:55 bowtie_index/mm10.4.ebwt<br>
-rw-r--r-- 1 y570pc y570pc 59026331 10月 25 19:59 bowtie_index/mm10.rev.1.ebwt<br>
-rw-r--r-- 1 y570pc y570pc 23988656 10月 25 19:59 bowtie_index/mm10.rev.2.ebwt<br>
#比对<br>
bowtie -m 1 -S bowtie_index/mm10 Oct4.fastq > Oct4.sam  #'-S'表示输出格式为sam，'-m<br> -1'表示reads多次map到参考序列时bowtie仅报道一次。<br>
#查看结果<br>
head -n 10 Oct4.sam<br>
#转sam文件为bam文件，bam为二进制文件，可以减少存储空间<br>
samtools view -bSo Oct4.bam Oct4.sam<br>
#排序<br>
samtools sort Oct4.bam -o Oct4.sorted.bam<br>
#对已排序的文件建立索引<br>
samtools index Oct4.sorted.bam  #h会生成索引文件Oct4.sorted.bam.bai<br>
</p>
</div>


## latex

#### 希腊字母

|希腊字母|LaTeX表示|希腊字母|LaTeX表示|
|---|---|---|---|
|$\alpha$|\alpha|$\mu$|\mu|
|$\beta$|\beta|$\xi$|\xi|
|$\gamma$|\gamma|$o$|o|
|$\delta$|\delta|$\pi$|\pi|
|$\epsilon$|\epsilon|$\rho$|\rho|
|$\zeta$|\zeta |$\sigma$|\sigma|
|$\eta$|\eta|$\tau$|\tau|
|$\theta$|\theta|$\upsilon$|\upsilon|
|$\iota$|\iota|$\phi$|\phi|
|$\kappa$|\kappa|$\chi$|\chi|
|$\lambda$|\lambda|$\psi$|\psi|
|$\mu$|\mu|$\omega$|\omega|

#### 符号

|符号|LaTeX表示|符号|LaTeX表示|
|---|---|---|---|
|$\vert$|\vert|||

## 参考资料

[1]. [Markdown Guide](https://about.gitlab.com/handbook/product/technical-writing/markdown-guide/#hello)









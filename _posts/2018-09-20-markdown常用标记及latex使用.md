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

#### 引用

使用

```
>但愿在我看不到的天际
>你张开了双翼
>遇见你已注定
>她会有多幸运
```

显示

>但愿在我看不到的天际
>你张开了双翼
>遇见你已注定
>她会有多幸运

#### 链接

使用`[百度](http://www.baidu.com)`，显示[百度](http://www.baidu.com)

使用`![图片](https://gitee.com/amius/learngit/raw/master/%E5%A4%B4%E5%83%8F.jpg)`,显示图片。

使用`[几种常见回归及scikit-learn实现]({{ y570pc.github.io }}{% link _posts/2018-10-24-几种常见回归及scikit-learn实现.md %})`，实现站内引用，效果[几种常见回归及scikit-learn实现]({{ y570pc.github.io }}{% link _posts/2018-10-24-几种常见回归及scikit-learn实现.md %})。
#### 列表

使用

```
* &hearts;列表
	&hearts;字列表 \\需要输入\tab键
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










---
layout: post
title:  "2019-01-10-基于galaxy的16s rRNA和宏基因组分析"
category: 'bioinformation'
tags: docker galaxy 16srrna 宏基因组
author: y570pc
image: 
---

## 环境搭建

```
docker run -d -p 8080:80 quay.io/galaxy/introduction-training
```

在浏览器中打开http://localhost:8080。以管理员身份登录（账号：admin@galaxy.org，密码admin）。

安装tools。依次点击Admin > Install new tools > Browse valid repositories > 搜索所需工具 > install > install to galaxy > 在add new tool panel section输入mothur（相当于新建工具文件夹） > install。



## 参考资料

[01]. [16S Microbial Analysis with Mothur](https://galaxyproject.github.io/training-material/topics/metagenomics/tutorials/mothur-miseq-sop/tutorial.html)

[02]. [Analyses of metagenomics data - The global picture](https://galaxyproject.github.io/training-material/topics/metagenomics/tutorials/general-tutorial/tutorial.html)

[03]. [Metagenomics standard operating procedure v2](https://github.com/LangilleLab/microbiome_helper/wiki/Metagenomics-standard-operating-procedure-v2)

[04]. [可视化生信分析利器-Galaxy（第一讲）](https://www.jianshu.com/p/a1f297eb4859)
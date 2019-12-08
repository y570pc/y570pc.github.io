---
layout: post
title:  "基于style2paints为线稿上色"
category: 'bigdata'
tags: style2paints styletransfer
author: y570pc
image: "/img/2018-10-26-03.jpg"
---

#### 环境配置

```
pip install tensorflow_gpu
pip install keras
pip install bottle
pip install gevent
pip install h5py
pip install paste
pip install opencv-python
pip install scikit-image
```

#### clone style2paints

```
git clone https://github.com/lllyasviel/style2paints.git
```

#### 下载训练好的模型

模型已上传至[微云](https://share.weiyun.com/5NWhcdZ)， 密码为tcnker，自行下载，文件列表如下，将其存放至`./style2paints/server`。

```
baby.net
head.net
neck.net
tail.net
reader.net
girder.net
```

#### 启动服务

```
cd style2paints/server
python server.py
```

#### 上色

浏览器输入`localhost:8000`，上传线稿，选择颜色上色，分为粗调和精调，计算完毕后，显示结果，继续调整至自己满意为止。我的结果如下：

![04](../img/2018-10-26-04.jpg)

## 参考文献

[1]. [style2paintsThe ChIPpeakAnno user’s guide](https://github.com/lllyasviel/style2paints)

[2]. [](https://zhuanlan.zhihu.com/p/36560034)

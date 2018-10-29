---
layout: post
title:  "基于tensorflow构建CNN实现手写数字识别"
category: 'bigdata'
tags: CNN tensorflow 深度学习
author: y570pc
image: "http://img.debugrun.com/pic/2017/8/11/41f1fea92c6835009f4f10126c8a051d.png"
---


## CNN结构

#### 卷积层（convolutional layers）

每一个卷积层中使用的过滤器参数是相同的，这就是卷积核的权值共享，这是CNN的一个非常重要的性质。共享每一个卷积层的卷积核，可以大大减少网络的参数，这不仅可以降低计算的复杂度，而且还能减少因为连接过多导致的严重过拟合，从而提高了模型的泛化能力。

在每一个卷积层中，会使用多个卷积核运算，这是因为每一个卷积核滤波得到的图像就是一类特征的映射，我们提供越多的卷积核，能提供多个方向上的图像特征，可以从图像中抽象出有效而丰富的高阶特征。 

#### 池化层（pooling layers）

在卷积层之间往往会加入一个池化层(pooling layer)。池化层可以非常有效地缩小图片的尺寸，从而减少最后全连接层的参数，加快计算速度、并赋予模型对轻度形变的容忍性，提高了模型的泛化能力。
池化层前向传播过程类似于卷积层的操作一样，也是通过移动一个类似滤波器的结构完成的，不同于卷积层的是，池化层的滤波器计算不是加权求和，而且求区域内的极大值或者平均值。

#### 全连接层(fully connected layers)

因为全连接层的作用主要就是实现分类（Classification）。因为空间结构特性被忽略了，所以全连接层不适合用于在方位上找Pattern的任务。

## 参数说明

filter：卷积核，又称滤波器。

步长(stride)参数:步长即每次filter移动的间隔距离。

batch：一个batch是使一批数据在模型中训练一次。

epoch：一个epoch是使训练集中的全部样本训练一次。

padding：填充输入矩阵边缘。

交叉熵损失：
$$
H(p,q)=-\sum p(X)logq(x)
$$

*交叉熵刻画的是两个概率分布之间的距离，它表示通过概率分布q（预测值）表示概率分布p（真实值）的困难程度。两者越接近，交叉熵越小。*

## 实战

#### [所需数据](http://yann.lecun.com/exdb/mnist/)

因为网络问题，需手动将所需的数据下载至代码文件所在的MNIST_data文件夹下。


#### 代码
{% highlight python%}
from tensorflow.examples.tutorials.mnist import input_data
import tensorflow as tf


def weight_variable(shape):
    '''
    使用卷积神经网络会有很多权重和偏置需要创建,我们可以定义初始化函数便于重复使用
    这里我们给权重制造一些随机噪声避免完全对称,使用截断的正态分布噪声,标准差为0.1
    :param shape: 需要创建的权重Shape
    :return: 权重Tensor
    '''
    initial = tf.truncated_normal(shape, stddev=0.1)
    return tf.Variable(initial)


def bias_variable(shape):
    '''
    偏置生成函数,因为激活函数使用的是ReLU,我们给偏置增加一些小的正值(0.1)避免死亡节点(dead neurons)
    :param shape:
    :return:
    '''
    initial = tf.constant(0.1, shape=shape)
    return tf.Variable(initial)


def conv2d(x, W):
    '''
    卷积层接下来要重复使用,tf.nn.conv2d是Tensorflow中的二维卷积函数,
    :param x: 输入 例如[5, 5, 1, 32]代表 卷积核尺寸为5x5,1个通道,32个不同卷积核
    :param W: 卷积的参数
        strides:代表卷积模板移动的步长,都是1代表不遗漏的划过图片的每一个点.
        padding:代表边界处理方式,SAME代表输入输出同尺寸
    :return:
    '''
    return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding="SAME")


def max_pool_2x2(x):
    '''
    tf.nn.max_pool是TensorFLow中最大池化函数.我们使用2x2最大池化
    因为希望整体上缩小图片尺寸,因而池化层的strides设为横竖两个方向为2步长
    :param x:
    :return:
    '''
    return tf.nn.max_pool(x, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding="SAME")


def train(mnist):
    # 使用占位符
    x = tf.placeholder(tf.float32, [None, 784])     # x为特征
    y_ = tf.placeholder(tf.float32, [None, 10])     # y_为label

    # 卷积中将1x784转换为28x28x1  [-1,,,]代表样本数量不变 [,,,1]代表通道数
    x_image = tf.reshape(x, [-1, 28, 28, 1])

    # 第一个卷积层  [5, 5, 1, 32]代表 卷积核尺寸为5x5,1个通道,32个不同卷积核
    # 创建滤波器权值-->加偏置-->卷积-->池化
    W_conv1 = weight_variable([5, 5, 1, 32])
    b_conv1 = bias_variable([32])
    h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1)+b_conv1) #28x28x1 与32个5x5x1滤波器 --> 28x28x32
    h_pool1 = max_pool_2x2(h_conv1)  # 28x28x32 -->14x14x32

    # 第二层卷积层 卷积核依旧是5x5 通道为32   有64个不同的卷积核
    W_conv2 = weight_variable([5, 5, 32, 64])
    b_conv2 = bias_variable([64])
    h_conv2 = tf.nn.relu(conv2d(h_pool1, W_conv2) + b_conv2) #14x14x32 与64个5x5x32滤波器 --> 14x14x64
    h_pool2 = max_pool_2x2(h_conv2)  #14x14x64 --> 7x7x64

    # h_pool2的大小为7x7x64 转为1-D 然后做FC层
    W_fc1 = weight_variable([7*7*64, 1024])
    b_fc1 = bias_variable([1024])
    h_pool2_flat = tf.reshape(h_pool2, [-1, 7*7*64])    #7x7x64 --> 1x3136
    h_fc1 = tf.nn.relu(tf.matmul(h_pool2_flat, W_fc1) + b_fc1)  #FC层传播 3136 --> 1024，Relu激活函数（The Rectified Linear Unit）表达式为：f(x)=max(0,x)。

    # 使用Dropout层减轻过拟合,通过一个placeholder传入keep_prob比率控制
    # 在训练中,我们随机丢弃一部分节点的数据来减轻过拟合,预测时则保留全部数据追求最佳性能
    keep_prob = tf.placeholder(tf.float32)
    h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)

    # 将Dropout层的输出连接到一个Softmax层,得到最后的概率输出
    W_fc2 = weight_variable([1024, 10])  #MNIST只有10种输出可能
    b_fc2 = bias_variable([10])
    y_conv = tf.nn.softmax(tf.matmul(h_fc1_drop, W_fc2) + b_fc2)

    # 定义损失函数,依旧使用交叉熵  同时定义优化器  learning rate = 1e-4
    cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_*tf.log(y_conv),reduction_indices=[1]))  #参考[3]
    train_step = tf.train.AdamOptimizer(1e-4).minimize(cross_entropy)  #参考[4]
#参考[3]
    # 定义评测准确率
    correct_prediction = tf.equal(tf.argmax(y_conv, 1), tf.argmax(y_, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))

    #开始训练
    with tf.Session() as sess:
        init_op = tf.global_variables_initializer() #初始化所有变量
        sess.run(init_op)

        STEPS = 20000
        for i in range(STEPS):
            batch = mnist.train.next_batch(50)
            if i % 100 == 0:
                train_accuracy = sess.run(accuracy, feed_dict={x: batch[0], y_: batch[1], keep_prob: 1.0})  #feed_dict的作用是给使用placeholder创建出来的tensor赋值
                print('step %d,training accuracy %g' % (i, train_accuracy))
            sess.run(train_step, feed_dict={x: batch[0], y_: batch[1], keep_prob: 0.5})

        acc = sess.run(accuracy, feed_dict={x: mnist.test.images, y_: mnist.test.labels, keep_prob: 1.0})
        print("test accuracy %g" % acc)



if __name__=="__main__":
    mnist = input_data.read_data_sets("MNIST_data/", one_hot=True) # 载入数据集
    train(mnist)
{% endhighlight %}

## 参考资料
[1]. [TensorFlow实战：Chapter-3（CNN-1-卷积神经网络简介）](https://blog.csdn.net/u011974639/article/details/75363565)

[2]. [tensorflow 基础学习三：损失函数讲解](https://www.cnblogs.com/hypnus-ly/p/8047214.html)

[3]. [Tensorflow 的reduce_sum()函数到底是什么意思，谁能解释下？](https://www.zhihu.com/question/51325408)

[4]. [如何选择优化器 optimizer](https://blog.csdn.net/aliceyangxi1987/article/details/73210204)

[5]. [深度学习(DL)与卷积神经网络(CNN)学习笔记随笔-03-基于Python的LeNet之LR](https://blog.csdn.net/niuwei22007/article/details/47705081)
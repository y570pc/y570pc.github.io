---
layout: post
title:  "基于Apache+php+MySQL搭建简易的本地OA系统"
category: 'website'
tags: Apache MySQL  OA系统
author: y570pc
introduction: 
image: 
---

## 环境配置

##### apache配置
 
下载[Apache 2.4.35 Win64](https://www.apachelounge.com/download/)，此为免安装版，解压到amp目录下，修改配置文件`httpd.conf`（注意与php相关的配置根据php版本修改）。

运行cmd，进入Apache24/bin目录，输入httpd.exe，启动http服务器。在浏览器中输入localhost:8080/index.html，显示`It works!`，表明Apache配置成功。

#### php配置

下载[php VC15 x64 Thread Safe](https://windows.php.net/downloads/releases/php-7.2.11-nts-Win32-VC15-x64.zip)，此为免安装版，解压到amp目录下，修改配置文件名`php.ini-development`为`php.ini`，按要求配置，需要取消部分用`;`注释的配置。

在Apache24/htdocs文件夹中新建文件phpinfo.php，写入如下代码：

```php
<?php
echo phpinfo();
```

在浏览器中输入localhost:8080/phpinfo.php，如果显示php版本信息，则php配置成功。

#### mysql配置

下载[MySQL Community Server 5.6.41](https://dev.mysql.com/downloads/mysql/5.6.html)，解压到amp目录下，按要求安装。

启动mysql：`net start mysql`

修改root密码：

```sql
use mysql;
UPDATE user SET password=PASSWORD("123456") WHERE user='root';
FLUSH PRIVILEGES;
```

停止mysql：`net stop mysql`

测试mysql，在Apache24/htdocs文件夹中新建文件test.php，写入如下代码：

```php
<?php
define("DB_SERVER", "localhost");
define("DB_USER", "root");
define("DB_PASSWORD", "LLte6628");
define("DB_DATABASE", "snp01");

$con = mysqli_connect(DB_SERVER , DB_USER, DB_PASSWORD, DB_DATABASE);
if(!$con)
    die("连接错误：".mysqli_connect_error());
else
    echo "恭喜，mysql连接成功！\n";
?>
```

在浏览器中输入localhost:8080/test.php，如果显示`恭喜，mysql连接成功！`，则mysql配置成功。
## 主要参考资料
[1]. [Windows下Apache+PHP+MySQL搭建历程](https://www.jianshu.com/p/9f41bcdff322)

[2]. [windows下配置apache+php环境](https://www.cnblogs.com/52fhy/p/6059685.html)

[3]. [MYSQL安装出现问题（The service already exists）](https://blog.csdn.net/qq_39701269/article/details/77935490)
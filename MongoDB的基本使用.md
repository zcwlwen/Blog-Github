---
title: MongoDB的基本使用
date: 2016-08-25 16:47:43
tags:
---

#MongoDB的基本使用

安装过程这里不再详细解释参考：

[菜鸟教程](http://www.runoob.com/mongodb/mongodb-linux-install.html)

其实这里面已经很详细的讲解了MongoDB从安装到使用的过程。我这里只是记录一下，我在做一个项目的时候用到的简单的知识。

在linux平台下首先在终端输入```mongo```进入到mongo的shell命令下，之后就可以对数据库进行一系列的操作了。

![17:00:39.jpg](http://ww1.sinaimg.cn/large/65e4f1e6gw1f763l82shmj20bj01kaa0.jpg)
输入```help```命令后，可以查看一些操作。
![17:04:31.jpg](http://ww1.sinaimg.cn/large/65e4f1e6gw1f763oztu3vj20bj05tq3f.jpg)

**常用命令：**
- ```show dbs```  显示数据库列表 - ```show collections``` 显示当前数据库中的集合（类似关系数据库中的表） 
- ```show users``` 显示用户- ```use <db name>```  切换当前数据库- ```db.help()```  显示数据库操作命令，里面有很多的命令 - ```db.<CollectionsName>.help()```  显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合，并非真正意义上的命令 - ```db. <CollectionsName>.find()``` 对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据） 

---
更多的东西暂时还没用到所有也没有系统的去学习MongoDB，我这里要用到php去操作MongoDB，后面应该会再记录一下php操作MongoDB的基本使用。

**提供一些参考资料**

- [MongoDB官方文档](https://docs.mongodb.com/?_ga=1.225214617.2084623026.1471936871)
- [菜鸟教程](http://www.runoob.com/mongodb/mongodb-tutorial.html)
- [博客地址](http://www.cnblogs.com/TankMa/archive/2011/06/08/2074947.html)

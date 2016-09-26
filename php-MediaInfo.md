---
title: php-MediaInfo
date: 2016-08-28 17:31:43
tags:
---

#php-MediaInfo
----
##需求描述
获取一个视频文件的详细信息
##实现方法
####安装MediaInfo
- linux下安装方法终端下执行
>$ sudo apt-get install mediainfo

- Mac下安装方法终端下执行
>$ brew install mediainfo

- Mac下有相应的MediaInfo的GUI版本

####安装相应的开源框架
这次用的是GitHub上开源框架[php-mediainfo](https://github.com/mhor/php-mediainfo)。

1. 集成这个开源框架到你的项目中
  - 使用Composer将php-mediainfo安装到你的项目中。
  
  - 局部安装Composer安装命令如下（先要进到你的项目目录下在执行安装命令）
>curl -sS https://getcomposer.org/installer | php

  - 使用Composer安装php-mediainfo到你的项目（也要在你的项目目录下去执行）
 >$ php composer.phar require mhor/php-mediainfo
 
    等待安装结束后再项目中就可以使用php-mediainfo。
  
----
2. 使用php-mediainfo

```
require 'vendor/autoload.php';
use Mhor\MediaInfo\MediaInfo;
$mediaInfo = new MediaInfo( );
//获得视频信息
$mediaInfoContainer = $mediaInfo->getInfo('test.mpg');
//转换成json格式
$jsonData = json_encode($mediaInfoContainer);
//对json格式解码
$jsonDecodeData = json_decode($jsonData);

```
----
***$mediaInfoContainer***保存了"yanshi.mp4"这个文件详细的信息。
***$json***只是为了方便查看内容。将信息转换成json格式后放到json解析网站查看结构解析自己需要的信息。
更多的使用方法查看php-mediainfo的github仓库的readme文件。

**参考资料**

- [Composer中文网](http://docs.phpcomposer.com/00-intro.html#Locally)
- [php-mediainfo--GitHub地址](https://github.com/mhor/php-mediainfo)

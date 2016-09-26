---
title: Ubuntu下安装ffmpeg和依赖包以及ffmpeg的使用
date: 2016-09-12 17:43:53
tags:
---
#Ubuntu下安装ffmpeg和依赖包以及ffmpeg的使用

多次尝试后通过[廖雪峰这篇教程](http://www.liaoxuefeng.com/article/001456198314370db046cbe5e5a45388bf3ade4bc2c5cb0000)安装成功了。
[官方文档](https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu)也很详细，但是我安装失败了，原因应该是配置中路径的问题。这里不推荐使用。
使用廖雪峰老师的这篇教程安装成功环境如下：
>Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.5 LTS
Release:	14.04
Codename:	trusty

编译的时候的一些命令不要完全复制，要根据你下载的版本和压缩包类型使用不同的命令。

FFmpeg是最流行的开源视频转码工具包，在Ubuntu上可以直接通过apt-get安装，但是默认的编码器不提供x264这些non-free的编码器，所以需要自己编译。

**安装过程**

使用`apt-get update`和 `apt-get upgrade`把系统升级到最新版，然后安装下面的安装包：

> apt-get install autoconf automake build-essential libass-dev libfreetype6-dev  libtheora-dev libtool libvorbis-dev pkg-config texinfo zlib1g-dev unzip cmake yasm libx264-dev libmp3lame-dev libopus-dev

FFmpeg依赖的几个软件包有个最低版本要求：

> yasm >= 1.2.0
> libx264-dev >= 0.118
> libmp3lame-dev >= 3.98.3
> libopus-dev >= 1.1

在这几个包在Ubuntu 14.04上都符合FFmpeg的要求，所以可以直接用`apt-get`安装。
编译的bin目录放在**/opt/**目录下
先在**/opt/**目录下创建三个文件夹，分别为**ffmpeg_sources、ffmpeg_build、bin**。
然后下载源码包放到**/opt/ffmpeg_sources/**目录下。
源码包下载地址：（请自己选择最新的版本下载）
x265：[https://bitbucket.org/multicoreware/x265/downloads](https://bitbucket.org/multicoreware/x265/downloads)

fdk-aac：[https://github.com/mstorsjo/fdk-aac/releases](https://github.com/mstorsjo/fdk-aac/releases)

ffmpeg：[http://ffmpeg.org/download.html](http://ffmpeg.org/download.html)

**编译x265**
依次执行下面的命令这里你根据你下载的文件去执行，这里下载的是1.9版本）

>cd /opt/ffmpeg_sources
tar zxvf x265_1.9.tar.gz
cd x265_1.9/build/linux
PATH="/opt/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="/opt/ffmpeg_build" -DENABLE_SHARED:bool=off ../../source
make
make install
make distclean

----


**编译fdk-aac**

依次执行下面的命令 （这里你根据你下载的文件去执行，这里下载的是0.1.4版本）

>cd /opt/ffmpeg_sources
tar zxvf fdk-aac-v0.1.4.tar.gz
cd fdk-aac-0.1.4
autoreconf -fiv
./configure --prefix="/opt/ffmpeg_build" --disable-shared
make
make install
make distclean

---

**编译ffmpeg**

>cd /opt/ffmpeg_sources
unzip FFmpeg-release-3.0.zip
cd FFmpeg-release-3.0
PATH="/opt/bin:$PATH" PKG_CONFIG_PATH="/opt/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="/opt/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I/opt/ffmpeg_build/include" \
  --extra-ldflags="-L/opt/ffmpeg_build/lib" \
  --bindir="/opt/bin" \
  --enable-gpl \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree
PATH="/opt/bin:$PATH" make
make install
make distclean
hash -r


编译时间很长，成功的话会在```/opt/bin```目录下出现ffmpeg、ffprobe、ffserver程序。
最后，创建几个软连接，便于任意用户在任意目录下直接调用ffmpeg：
>ln -s /opt/bin/ffmpeg /usr/bin/ffmpeg
ln -s /opt/bin/ffprobe /usr/bin/ffprobe
ln -s /opt/bin/ffserver /usr/bin/ffserver


ffmpeg是转码程序，ffprobe可以用来分析视频文件，ffserver可以实现流媒体服务器。

**使用ffmpeg**

直接在终端执行
> ffmpeg -i yanshi.flv -c:v libx264 -b:v 1024k -c:a libfdk_aac -b:a 128k -ar 16000 final.mp4

这里的**yanshi.flv**是要转码的文件，**final.mp4**是转码后的文件。

**ffmpeg的常用参数详解**
**主要参数**
- -i ——设置输入文档名
- -f ——设置输出格式
- -y——若输出文件已存在时则覆盖文件
- -fs——超过指定的文件大小时则结束转换
- -ss——从指定时间开始转换
- -t从-ss时间开始转换（如-ss 00:00:01.00 -t 00:00:10.00即从00:00:01.00开始到00:00:11.00
- -title——设置标题
- -timestamp——设置时间戳
- -vsync——增减Frame使影音同步

**视频参数**
- -b:v——设置视频流量，默认为200Kbit/秒
- -r——设置帧率值，默认为25
- -s——设置画面的宽与高
- -aspect——设置画面的比例
- -vn——不处理视频，于仅针对声音做处理时使用
- -vcodec( -c:v )——设置视频视频编解码器，未设置时则使用与输入文件相同之编解码器

**音频参数**
- -b:a——设置每Channel（最近的SVN版为所有Channel的总合）的流量
- -ar——设置采样率
- -ac——设置声音的Channel数
- -acodec ( -c:a ) ——设置声音编解码器，未设置时与视频相同，使用与输入文件相同之编解码器
- -an——不处理声音，于仅针对视频做处理时使用
- -vol——设置音量大小，256为标准音量。（要设置成两倍音量时则输入512，依此类推。）

**注意事项**
- 以-b:v及-b:a首选项流量时，根据使用的ffmpeg版本，须注意单位会有kbits/sec与bits/sec的不同。（可用ffmpeg -h显示说明来确认单位。）
例如，单位为bits/sec的情况时，欲指定流量64kbps时需输入 -b:a 64k；单位为kbits/sec的情况时则需输入 -b:a 64。
- 以-acodec及-vcodec所指定的编解码器名称，会根据使用的ffmpeg版本而有所不同。例如使用AAC编解码器时，会有输入aac与libfaac的情况。此外，编解码器有分为仅供解码时使用与仅供编码时使用，因此一定要利用ffmpeg -formats确认输入的编解码器是否能运作。

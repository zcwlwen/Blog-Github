---
title: php-mediainfo信息解析
date: 2016-08-29 16:52:50
tags:
---

#php-mediainfo获得的信息解析

##数据解析

直接看代码就好了
一层一层解析

```
<?php
require 'vendor/autoload.php';
use Mhor\MediaInfo\MediaInfo;
$mediaInfo = new MediaInfo( );

//获得视频信息
$mediaInfoContainer = $mediaInfo->getInfo('test.mpg');
//转换成json格式
$jsonData = json_encode($mediaInfoContainer);
//对json格式解码
$jsonDecodeData = json_decode($jsonData);

/**********************************************************************************************************/
//最外层的解析  主要有用的是$general、$audios、$videos其他的信息这里用不到
//版本
$version = $jsonDecodeData->version;
//整体信息
$general = $jsonDecodeData->general;
//音频信息
$audiosZ  = $jsonDecodeData->audios;
$audios = $audiosZ[0];
//视频信息
$videosZ  = $jsonDecodeData->videos;
$videos = $videosZ[0];
//字幕信息
$subtitles = $jsonDecodeData->subtitles;
//图片
$images = $jsonDecodeData->images;
//菜单
$menus = $jsonDecodeData->menus;
//其他
$others = $jsonDecodeData->others;

/*********************************************************/
//Version不需要再解析  $version这里就是当前的版本信息
/*********************************************************/
//$general的解析
//
//视频格式列表
$video_format_list = $general->video_format_list;
//视频解编码器
$codecs_video = $general->codecs_video;
//音频格式列表
$audio_format_list = $general->audio_format_list;
//音频解编码器
$audio_codecs = $general->audio_codecs;
//完整名字
$complete_name = $general->complete_name;
//文件名
$file_name = $general->file_name;
//文件格式
$file_extension = $general->file_extension;
//格式
$format = $general->format->shortName;
//网络媒体类型
$internet_media_type = $general->internet_media_type;
//解编码器
$codec = $general->codec->shortName;
//文件大小 单位bit
$file_size = $general->file_size->bit;
//持续时间 单位毫秒
$duration = $general->duration->milliseconds;
//总比特率模式
$overall_bit_rate_mode = $general->overall_bit_rate_mode->shortName;
//总比特率
$overall_bit_rate = $general->overall_bit_rate->shortName;
//帧率
$frame_rate = $general->frame_rate->absoluteValue;

var_dump($codec);
/*********************************************************/
//$audios的解析
//音频格式
$Audio_format = $audios->format->shortName;
//音频网络媒体类型
$Audio_internet_media_type = $audios->internet_media_type;
//音频解编码器
$Audio_codec = $audios->codec->shortName;
//音频比特率模式
$Audio_bit_rate_mode = $audios->bit_rate_mode->shortName;
//音频比特率 单位bps
$Audio_bit_rate = $audios->bit_rate->absoluteValue;
//声道
$Audio_channel_s = $audios->channel_s->absoluteValue;
//音频采样率
$Audio_sampling_rate = $audios->sampling_rate->absoluteValue;
//音频采样数
$Audio_frame_count = $audios->frame_count;
//音频压缩模式
$Audio_compression_mode = $audios->compression_mode->shortName;

/***********************************************************/
//$videos的解析
//视频格式
$Video_format = $videos->format->shortName;
//视频格式版本
$Video_format_version = $videos->format_version;
//视频格式简介
$Video_format_profile = $videos->format_profile;
//视频格式设置  BVOP GOP  这些东西都不是很了解
$Video_format_settings = $videos->format_settings;
//视频格式设置矩阵
$Video_format_settings_matrix = $videos->format_settings_matrix;
//视频网络媒体类型
$Video_internet_media_type = $videos->internet_media_type;
//视频解编码器
$Video_codec = $videos->codec->shortName;
//视频解编码profile
$Video_codec_profile = $videos->codec_profile;
//视频比特率
$Video_bit_rate = $videos->bit_rate->absoluteValue;
//视频最大比特率
$Video_maximum_bit_rate = $videos->maximum_bit_rate->absoluteValue;
//视频宽度 单位像素
$Video_width = $videos->width->absoluteValue;
//视频高度 单位像素
$Video_height = $videos->height->absoluteValue;
//像素宽高比
$Video_pixel_aspect_ratio = $videos->pixel_aspect_ratio;
//显示宽高比  16:9  4:3--
$Video_display_aspect_ratio = $videos->display_aspect_ratio->absoluteValue;
//视频帧率
$Video_frame_rate = $videos->frame_rate;
//视频比色法
$Video_colorimetry = $videos->colorimetry;
//视频色彩空间
$Video_color_space = $videos->color_space;
//视频色度抽样
$Video_chroma_subsampling = $videos->chroma_subsampling;
//视频位深度
$Video_bit_depth = $videos->bit_depth;
//视频扫描类型
$Video_scan_type = $videos->scan_type->shortName;
//视频压缩模式
$Video_compression_mode = $videos->compression_mode->shortName;
//视频流大小 单位 bit
$Video_stream_size = $videos->stream_size->bit;
```

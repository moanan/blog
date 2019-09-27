---
title: 实用OSX命令
date: 2019-08-11 15:43:35
tags:
---

一些小工具很不起眼，但在需要时却非常有用。比如在吃核桃时，没有`核桃夹`会非常痛苦。
{% asset_img nut_cracking.jpeg 没有小工具的猩猩 %}

## 图片批量压缩-sip
> sips --resampleHeight 1200 \*.jpg --out `targetFolder`

To bulk resize multiple images, you can set the input image as a file name pattern, such as all jpg files: \*.jpg. Before running the resizing code, create a target folder like `targetFolder` to hold all the resized images. 


## 视频转码-ffmpeg
> ffmpeg -y -r 30 -i input.cine -r 30 -c:v libx264 output.mp4

-i [url] Input file
-y Overwrite output files without asking
-r [frameRate] Force the frame rate (input/output) to `frameRate`
-c[:stream_specifier] select encoder

其实还可以转音频，如将目录中的无损音乐文件`*.flac`转为Itunes支持的`*.m4a`格式：
> for f in \*.flac; do ffmpeg -i "$f" -acodec alac "${f%flac}m4a"; done

参考资料：[Index of the Apple macOS command line (OS X bash)](https://ss64.com/osx/)
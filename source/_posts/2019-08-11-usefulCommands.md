---
title: 实用OSX命令
date: 2019-08-11 15:43:35
tags:
---

一些小工具很不起眼，但在需要时却非常有用。比如在吃核桃时，没有`核桃夹`会非常痛苦。

<!--more-->

{% asset_img nut_cracking.jpeg 没有小工具的猩猩 %}

## 图片批量压缩-sip
```bash
sips --resampleHeight 1200 \*.jpg --out `targetFolder`
```

To bulk resize multiple images, you can set the input image as a file name pattern, such as all jpg files: \*.jpg. Before running the resizing code, create a target folder like `targetFolder` to hold all the resized images. 


## 视频转码-ffmpeg
ffmpeg支持绝大多数音视频编码的相互转换。通常我会转成mp4格式，大多数情况下输出视频体积远小于原始视频，并且几乎不影响视频质量。
```bash
ffmpeg -y -r 30 -i in.cine -r 30 -c:v libx264 out.mp4
```

-i [url] Input file
-y Overwrite output files without asking
-r [frameRate] Force the frame rate (input/output) to `frameRate`
-c[:stream_specifier] select encoder

其实还可以转音频，如将目录中的无损音乐文件`*.flac`转为Itunes支持的`*.m4a`格式：
```bash
for f in \*.flac; do ffmpeg -i "$f" -acodec alac "${f%flac}m4a"; done
```

## 文件夹同步-rsync
```bash
rsync source destination
```

-a archive, sync recursively
-z compress
-v verbose
-P progress
-n dry run

同步之前可以使用`-n`模拟真实输出，检验命令是否正确。注意：目标路径的末尾有`/`，表示同步该路径下的所有文件，而不包括该文件夹本身，如：

```bash
rsync -azvP --delete /media/pi/Elements/ /media/pi/My\ Passport/backup/Main_mirror
rsync -anzvP --delete /Users/anmo/Documents/ /Volumes/Backup/Documents (dry run)
```

参考资料：
[Index of the Apple macOS command line (OS X bash)](https://ss64.com/osx/)
[How To Use Rsync to Sync Local and Remote Directories on a VPS](
https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps )
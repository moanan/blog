---
title: 实用命令行工具
date: 2019-08-11 15:43:35
tags: 技术
---

整理一些平时常用的小工具。

<!--more-->

## 文件批量更名
GoPro相机自动生成的.LRV文件改为.mp4后缀就能直接播放，不需要将原视频转码压缩。
```bash
for f in ./*.LRV; do mv "$f"  "${f%.LRV}.mp4"; done
```

## 图片压缩-sip (OSX)
Mac自带的图片压缩工具[sips](https://www.birme.net/blog/bulk-resize-images-with-sips-on-mac/)，支持批处理，可以方便地将整个文件夹内的图片压缩成想要的分辨率。
```bash
sips --resampleHeight 1200 *.jpg --out `targetFolder`
```
To bulk resize multiple images, you can set the input image as a file name pattern, such as all jpg files: \*.jpg. Before running the resizing code, create a target folder like `targetFolder` to hold all the resized images. 

## PDF压缩
[ghostscript](https://www.ghostscript.com/)适用于大多数pdf的压缩:
```bash
ghostscript -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/printer -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
\\ or
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/printer -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```
Other options for PDFSETTINGS:
/screen selects low-resolution output similar to the Acrobat Distiller "Screen Optimized" setting.
/ebook selects medium-resolution output similar to the Acrobat Distiller "eBook" setting.
/printer selects output similar to the Acrobat Distiller "Print Optimized" setting.
/prepress selects output similar to Acrobat Distiller "Prepress Optimized" setting.
/default selects output intended to be useful across a wide variety of uses, possibly at the expense of a larger output file.

能将扫描文件压缩到10倍的高级指令[pdfsandwich](http://www.tobias-elze.de/pdfsandwich/)，再也不用担心辣鸡扫描仪导出的大文件作为普通附件上传，甚至还支持多国语言OCR：
```bash
pdfsandwich alice.pdf
```

## PDF合并多个文件
```bash
gs -dNOPAUSE -sDEVICE=pdfwrite -sOUTPUTFILE=combine.pdf -dBATCH *.pdf
```

## 文件夹同步-rsync
[rsync](
https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps )可以同步文件夹、备份硬盘，甚至通过ssh传输数据。如果是单纯需要备份，桌面应用[FreeFileSync](https://freefilesync.org/)也是很好的选择。
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

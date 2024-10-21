---
title: 骑行视频叠加数据
date: 2024-10-21 21:49:15
tags: 
mathjax: true
---

最近有小伙伴问我如何将骑行的码表数据叠加到运动相机的视频上，目前并没有任何一个免费的软件可以比较无痛的实现这个功能，即将码表数据可视化并渲染到已有的视频上，且保持时钟同步。

<!--more-->

## 安装
- [ffmpeg](https://www.ffmpeg.org/)
- [gopro-dashboard-overlay](https://github.com/time4tea/gopro-dashboard-overlay)
- need to install the font `Roboto-Medium.ttf`
- need to install `libraqm`, works with conda, follow [this](https://stackoverflow.com/questions/74608140/pillow-not-recognizing-libraqm-installation-on-mac-os)
```bash
brew install libraqm 
brew install freetype harfbuzz fribidi
pip uninstall Pillow  
pip install Pillow==9.4.0 --global-option="build_ext" --global-option="--enable-raqm"
```

我的工作流整理如下：

## 视频采集
- 设置1080p格式录像（也可以采用其他分辨率）
- 确保码表建立可靠的卫星信号后，同时开启码表记录和运动相机录制

## 视频压缩归档
- 视频体积较大，通过一下命令将视频压缩归档
```bash
# mac with Apple silicon (hardware acceleration)
for f in ./*.MP4; do ffmpeg -y -i "$f" -c:v h264_videotoolbox -b:v 12000k -pass 1 -vsync cfr -f null /dev/null && ffmpeg -i "$f" -c:v h264_videotoolbox -b:v 12000k -pass 2 -c:a aac -b:a 128k "${f%.MP4}_12mbs.mp4"; done
# linux
for f in ./*.MP4; do ffmpeg -y -i "$f" -c:v libx264 -b:v 10000k -pass 1 -vsync cfr -f null /dev/null && ffmpeg -i "$f" -c:v libx264 -b:v 10000k -pass 2 -c:a aac -b:a 128k "${f%.MP4}_10mbs.mp4"; done
```

## 数据渲染设置
- 根据码表自持的传感器以及自己的偏好，编辑[配置文件](https://github.com/time4tea/gopro-dashboard-overlay/blob/main/gopro_overlay/layouts/default-1920x1080.xml)，参考[我的配置](my_layout_1080p.xml)

## 码表数据对齐
- 将第一个原始视频裁剪为前1分钟左右的影像`alignment.mp4`；
- 将码表数据导出为`.fit`格式；
- 







## google map路径渲染

## 参考资料
github
google map


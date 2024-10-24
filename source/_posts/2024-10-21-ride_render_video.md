---
title: 骑行视频叠加数据
date: 2024-10-21 21:49:15
tags: 
mathjax: true
---

对数据敏感的盆友可以将码表数据同步渲染到运动相机的视频中，支持多种信息显示：日期、时间、GPS坐标、速度、海拔、坡度、踏频、心率、温度、地图等。支持多种码表类型、多种相机类型。

<!--more-->

{% asset_img Screenshot.png 500 %}

## 安装
- [ffmpeg](https://www.ffmpeg.org/)
- [gopro-dashboard-overlay](https://github.com/time4tea/gopro-dashboard-overlay)
- 安装字体[Roboto-Medium.ttf](Roboto-Medium.ttf)
- 如运行时有python报错，需安装`libraqm`, works with conda, follow [this](https://stackoverflow.com/questions/74608140/pillow-not-recognizing-libraqm-installation-on-mac-os)
```bash
brew install libraqm 
brew install freetype harfbuzz fribidi
pip uninstall Pillow  
pip install Pillow==9.4.0 --global-option="build_ext" --global-option="--enable-raqm"
```

## 视频采集
- 设置1080p格式录像（也可以采用其他分辨率）
- 确保码表建立可靠的卫星信号后，同时开启码表记录和运动相机录制

## 视频压缩归档
- 视频体积较大，通过以下命令将视频压缩归档：
```bash
# mac with Apple silicon (hardware acceleration)
for f in ./*.MP4; do ffmpeg -y -i "$f" -c:v h264_videotoolbox -b:v 12000k -pass 1 -vsync cfr -f null /dev/null && ffmpeg -i "$f" -c:v h264_videotoolbox -b:v 12000k -pass 2 -c:a aac -b:a 128k "${f%.MP4}_12mbs.mp4"; done
# linux
for f in ./*.MP4; do ffmpeg -y -i "$f" -c:v libx264 -b:v 10000k -pass 1 -vsync cfr -f null /dev/null && ffmpeg -i "$f" -c:v libx264 -b:v 10000k -pass 2 -c:a aac -b:a 128k "${f%.MP4}_10mbs.mp4"; done
```

## 数据渲染设置
- 根据码表自持的传感器以及自己的偏好，编辑[配置文件](https://github.com/time4tea/gopro-dashboard-overlay/blob/main/gopro_overlay/layouts/default-1920x1080.xml)，参考[我的配置](my_layout_1080p.xml)

## 码表数据对齐
- 将第一个原始视频裁剪为前1分钟左右的影像`test.mp4`
- 将码表数据导出为`.fit`格式
- 在[GOTOES网站](https://gotoes.org/strava/Combine_GPX_TCX_FIT_Files.php)上将`.fit`格式文件转换为`.gpx`格式`converted_from_fit.gpx`
- 试生成数据叠加视频：
```bash
/Path_To_Installtion/gopro-dashboard.py --units-speed kph --use-gpx-only --gpx converted_from_fit.gpx --layout-xml /Path_To_Layout/my_layout_1080p.xml test.mp4 test_overlay.mp4
```
- 播放`test_overlay.mp4`视频，通常会发现码表数据是滞后的，因此需要裁掉视频最初的几十秒，得到`test2.mp4`
- 重复执行前面两个步骤，直至找到合适的裁剪时间点`t`，这个过程中还可以持续调整[配置文件](https://github.com/time4tea/gopro-dashboard-overlay/blob/main/gopro_overlay/layouts/default-1920x1080.xml)
- 将第一个原始视频按时间`t`裁剪掉最初的部分，剩余的部分和后续的视频拼接成一个视频：
```
ffmpeg -f concat -safe 0 -i <(for f in ./*.MP4; do echo "file '$PWD/$f'"; done) -c copy combine.mp4
```

## 渲染
```bash
/Path_To_Installtion/gopro-dashboard.py --units-speed kph --use-gpx-only --gpx converted_from_fit.gpx --layout-xml /Path_To_Layout/my_layout_1080p.xml combine.mp4 overlay.mp4
```
- `overlay.mp4`即为输出的显示码表数据的视频

## google map路径渲染
{% youtube ZUS-b0iVxnC6cVoQ %}


## 参考资料
- [gopro-dashboard-overlay](https://github.com/time4tea/gopro-dashboard-overlay/tree/main)
- [how to create and animate track, route in Google Earth
](https://youtu.be/y_lYcwg_cVc?si=aGguZwphoGT8js2_)


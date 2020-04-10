---
title: 树莓派airplay热点
date: 2020-01-09 08:13:30
tags:
---

家里闲置一套非常古老的索尼音响，可以放磁带、CD、广播的那种，留之无用弃之可惜。幸好它还支持音频输入，所以索性连上树莓派打造一个无线音响。

<!--more-->

## 安装
https://github.com/mikebrady/shairport-sync

## 调试
start:
```bash
sudo systemctl enable shairport-sync.service
sudo service shairport-sync start
```

restart:
```bash
sudo service shairport-sync restart
```

adjust volume:(默认音域范围很窄，需要手动调至0 dB附近)
```bash
sudo alsamixer
```

config file:
```bash
vim /etc/shairport-sync.conf
```
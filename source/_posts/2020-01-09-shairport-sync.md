---
title: 树莓派airplay热点
date: 2020-01-09 08:13:30
tags:
---

闲置一套非常古老的索尼音响，可以放磁带、CD、广播的那种。幸好它还支持音频输入，所以索性连上树莓派，用[shairport-sync](https://github.com/mikebrady/shairport-sync)打造一个airplay无线音响。

<!--more-->

## 安装
https://github.com/mikebrady/shairport-sync/blob/master/INSTALL.md

### 关闭WiFi省电模式
```bash
iwconfig wlan0 power off
```
然后重启。

### （移除旧版本）
```bash
$ which shairport-sync
/usr/local/bin/shairport-sync
```
移除旧版本：
```bash
rm /usr/local/bin/shairport-sync
```
重复上面两条指令，直到没有更多`shairport-sync`。

### (移除旧启动程序)
```bash
rm /etc/systemd/system/shairport-sync.service
rm /lib/systemd/system/shairport-sync.service
rm /etc/init.d/shairport-sync
```
然后重启。

### 准备依赖
```bash
apt-get install build-essential git xmltoman autoconf automake libtool libpopt-dev libconfig-dev libasound2-dev avahi-daemon libavahi-client-dev libssl-dev libsoxr-dev
```

### 下载安装
```bash
git clone https://github.com/mikebrady/shairport-sync.git
cd shairport-sync/
autoreconf -fi
./configure --sysconfdir=/etc --with-alsa --with-soxr --with-avahi --with-ssl=openssl --with-systemd
make
sudo make install
```

### 配置Shairport
编辑配置文件`/etc/shairport-sync.conf`，有3处地方要改：

- airplay时显示的设备名称：
```bash
general =
{
  name = "name";
  // password = "secret";
  // ... other general settings
};
```
- 使用树莓派内置声卡时需要调整音量范围：
```bash
general =
{
  volume_range_db = 60; 
};
```

- 选择音源输出设备（声卡）：
```bash
alsa =
{
  output_device = "hw:0"; // the name of the alsa output device. Use "alsamixer" or "aplay" or "shairport-sync -h" to find out the names of devices, mixers, etc.
  mixer_control_name = "PCM"; // the name of the mixer to use to adjust output volume. If not specified, volume in adjusted in software.
  // ... other alsa settings
```

### 设置开机自启
```bash
systemctl enable shairport-sync
systemctl start shairport-sync
```

## troubleshoot
https://github.com/mikebrady/shairport-sync/issues/952
https://github.com/mikebrady/shairport-sync/issues/349
https://github.com/mikebrady/shairport-sync/issues/741

判断程序是否正常运行：
```bash
shairport-sync -V
systemctl status shairport-sync
```

### 没有声音
很可能是音源输出不对。通过命令`aplay -l`或`shairport-sync -h`找到目标声卡名字，并更新至配置文件`/etc/shairport-sync.conf`中`output_device = "hw:0"`。如树莓派4的内置声卡为`"hw:Headphones"`。

### 声音太小
默认音域范围很窄，如果没有正确设置配置文件，需要手动调至0 dB附近。
```bash
alsamixer
```
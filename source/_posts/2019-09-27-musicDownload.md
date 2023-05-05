---
title: 如何在国外优雅地使用网易云音乐
date: 2019-09-27 11:53:32
tags: 技术
---

身边一些在国外的朋友，由于版权受限不能使用网易云音乐等国内音乐平台，屈服于Spotify，Apple Music等国外资本家的音乐平台。下面分享一些我不屈国外靡靡之音的心得。

<!--more-->

## 傻瓜模式：网易UU加速器
在国外不能使用网易云音乐的本质原因是设备定外在国外，网易版权不适用。其实网易早就提你想好了，**最简单且免费**的途径是搭配使用[网易UU加速器](https://uu.163.com)，可以达到以子之矛攻子之盾的效果。网易UU加速器可以选择加速网易云音乐，此时网易云音乐的流量走国内，通过VPN传回，你的设备就伪装成了在国内。

## 硬核模式：翻墙回国
同理，**最可靠**的方法是直接将设备翻墙回国。方法与各种科学上网方式相同，在此不再赘述。在国内没有节点的同学可以考虑临时租用[FastSS](https://www.fastss.blue/index.php)的回国线路。

## 进阶模式：下载本地
音乐（以及其它资源）在（别人家的）云上总是有些虚无缥缈，随时可能因为版权、政策等原因突然下架。只有下载到本地才是实实在在的真金白银。下面介绍几种将云端音乐保存到本地的方法，真正**一劳永逸**地享受音乐。
注：以下方法均以实现翻墙回国或设备在国内为前提！

### 暴力下载
在网易云音乐网页版打开任意音乐，如周杰伦的*说好不哭*，网址是：
http://music.163.com/#/song?id=1391274164
所以，音乐id是*1391274164*，将这串数字替换到下面链接的*数字*：
http://music.163.com/song/media/outer/url?id=数字.mp3
得到该音乐的真实外链地址：
http://music.163.com/song/media/outer/url?id=1391274164.mp3
然后在浏览器打开这个地址，另存为就能下载.mp3格式的音乐。

### 第三方平台
如果嫌麻烦可以使用第三方网站
http://www.gequdaquan.net/gqss/
http://tool.liumingye.cn/music/?type=xiami&name=%E9%99%88%E5%A5%95%E8%BF%85
http://www.fulichaoshi.com/
http://music.wandhi.com/
http://moresound.tk/music/teach.html#c
搜索歌曲，直接点击下载。

### 命令行工具
显然，上述两种方法相当原始。想要批量下载自己喜欢的专辑、歌单，可以借助各位大佬post在GitHub上的命令行工具。我在MacOS测试了[music-dl](https://github.com/0xHJK/music-dl)， [music-get](https://github.com/winterssy/music-get)和[netease-cloud-music-dl](https://github.com/codezjx/netease-cloud-music-dl)几个工具，详细的使用说明在项目主页，我简要总结如下：

工具 					| 支持平台 	| 备注 
------------------------|-----------|-----
music-dl 				| 网易云, QQ, 酷狗, 咪咕, 百度, 虾米 | 适用于多平台搜歌、下载 
music-get 				| 网易云, QQ	| 需要登录 
netease-cloud-music-dl	| 网易云 	| 自动写入专辑封面，记录元数据 

其中我最满意的还是`netease-cloud-music-dl`，完全满足我的下载需求：
- 支持下载专辑封面并嵌入MP3文件
- 支持写入歌手名、音乐标题、专辑名等信息至ID3 Tags
- 支持跳过已下载的音频文件
- 支持常见设置选项，如：保存路径、音乐命名格式、文件智能分类等
- 默认下载比特率为320k的高品质音乐（若木有320k则会自动下载最高比特率）
- 支持下载单首/多首歌曲
- 支持下载歌手热门单曲（可配置最大下载数）
- 支持下载专辑所有歌曲
- 支持下载公开歌单所有歌曲

注：mac终端默认不走代理，因此即使ss/VPN设置全局代理，仍然无法使用命令行工具正常下载。我使用的是ss，在命令行输入执行以下两条指令：
```bash
export http_proxy=http://127.0.0.1:1087
export https_proxy=http://127.0.0.1:1087
```

当前终端就实现代理了。macOS 版的 SS 默认监控本地的HTTP端口是`1087`，而Windows版本的则是`1080`，如果改过默认端口，就使用你指定的端口。（终端重启后需要重新输入这两条指令）


参考资料：
1. [那些你可能不知道的网易云音乐奇技淫巧](https://zhuanlan.zhihu.com/p/67722976)
2. [网易云音乐mp3外链、真实地址下载方法](https://my.oschina.net/yzbty32/blog/3003833)
3. [mac 终端实现翻墙](https://kerminate.me/2018/10/22/mac-%E7%BB%88%E7%AB%AF%E5%AE%9E%E7%8E%B0%E7%BF%BB%E5%A2%99/)



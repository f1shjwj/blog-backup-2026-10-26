---
title: OPPO R11 改造记录
date: 2023/08/22 12:34:00
categories: 
- 记录
tags: 
- 刷机
thumbnail: /img/blog/OPPOR11.png
---

旧手机OPPO R11，目标是改造成服务器使用，用来挂QQbot啊MAA啊什么的。

不定时更新。

[头图来源](https://www.gsmarena.com/oppo_r11-8644.php)

<!-- more -->

## 参考链接

[第一次解锁刷机教程 酷安](https://www.coolapk.com/feed/31391857)

[橙盘](https://d.cyteam.cn/)，资源库，不过以前改过一次域名，不知道以后还会不会改，并且有密码，得加群获取。

[极客湾的刷机入门教程 b站](https://www.bilibili.com/video/BV1BY4y1H7Mc)

[晨钟酱的玩机资源合集](http://wanji.jamcz.com/)

[Magisk Github](https://github.com/topjohnwu/Magisk/releases)

[充电控制 ACCA Github](https://github.com/MatteCarra/AccA)

[也是充电控制 Github](https://github.com/sriharshaarangi/BatteryChargeLimit)

## 记录

之前第一次刷系统刷了MIUI12，初步体验了刷系统，然后装了Magisk和一些软件但是没装模块。

由于我的目标是改造成服务器，而MIUI比较臃肿，并且比较容易杀后台，所以现在我把它刷成了应该大概也许不会杀后台的类原生的系统，crDroid，比较歪打正着吧，很纯净，没有预装谷歌套装。

关于刷系统的步骤，我是在橙盘里下载的包，然后发现里面已经带了安装脚本了，所以就直接装上去了。然后TWRP也帮我装好了，太贴心了。其实极客湾的视频里头也讲的差不多了。如果以后出了啥问题导致砖了什么的，尝试了其他办法还是没用的话，可以直接通过包里的脚本重刷回去。

折腾手机的话最好还是备份一下字库，避免被格机。需要用到一个叫“多系统工具箱”的APP，酷安可下载，选择备份完整字库，把文件备份到电脑即可。有脚本，可以进fastboot直接用脚本刷回去。

然后是装Magisk。之前在MIUI12的时候我试过装最新版的Magisk，但是不起作用，后来从别的贴子知道MIUI12只能安装v23.0的版本，现在换了系统那就安装最新的v26.1吧。我一开始是通过TWRP卡刷进去的，但是后来不知道为啥卡第一屏了（可能是我姿势不对），所以就使用了极客湾视频中的第二种办法，就是把系统包里的boot镜像交给Magisk APP进行修补，再通过fastboot刷进去。判断是否root的办法是看APP里的版本号，其他的其实比较无关紧要。装好后在Rec备份system分区和boot镜像。

开始安装模块。对于我这种刷机次数不多的小白来说还是装个神仙自动救砖比较好。我用的是酷安用户“奋斗的小青年”发布的版本。之前想过装一下存储重定向的模块，后来想到这部手机也不是拿来日用的，就懒得安装了。题外话（记录给自己看的），如果是日用主力机的话，除了网上的各种优化模块推荐外也可以去看看DIYgod大佬的博客，看看有没有能用得上的。

由于我会给这部手机长期通电，所以装一个充电控制。我用的是[ACCA](https://github.com/MatteCarra/AccA)，装上去给个root权限就好了，很简单。

无论是想用来挂脚本挂QQbot还是MAA，必不可少的软件是termux，必不可少的步骤是异地组网。考虑到安全问题我不打算使用公网IP，于是我还是选择蒲公英。
//异地组网，在考虑WireGuard/Tailscale/zerotier

//软件远程控制，（摄像头，麦克风，扬声器？）

//安装termux，配置ssh连接，配置adb与被adb（？）

//开始qqbot或者maa吧

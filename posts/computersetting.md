---
title: jwj 的电脑调教记录
date: 2024/03/14 16:02:00
categories:
- 记录
tags:
- Linux
- 系统设置
thumbnail:
---

寒假重装了系统，想着重新配置一套我能够满意的系统环境，于是折腾了一大堆七七八八的东西，以后如果折腾了什么东西的话可能会诈尸更新。（ ~~诈尸还挺频繁~~ ）而且我感觉里面很多内容都可以拆分出来重新水一篇博客了。

哦对了，不要给自己创造需求，不装很多大型游戏的话其实 512G 完全够了，~~别问我怎么知道的。~~

<!-- more -->

电脑：机械革命 Code01 V2.0 R7-6800H 16G

## 零、系统功能

想搞 Hyper-V 和沙盒最好直接把系统换成 [Win11 专业版](https://zhuanlan.zhihu.com/p/637930518)。按照评论区的方案来做，亲测成功。

可以考虑把界面切换为全英。

添加功能中添加 WSL、Hyper-V、沙盒等虚拟化功能。如果不是非要虚拟机的话其实可以不用专业版的，毕竟家庭版也可以开 WSL，能开 WSL 相当于已经开了 Hyper-V 只是不给你直接用而已，然后你就可以在 WSL 的基础上开 Docker。我感觉我自己应该没啥必要开，可能也就偶尔开开沙盒，不过开都开了就这样吧。

- 非专业版开启 Hyper-V `bat`脚本（管理员运行）：

```batch
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hv.txt
for /f %%i in ('findstr /i . hv.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hv.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V -All /LimitAccess /ALL
pause
```

- 主策略 `bat`脚本（管理员运行）：

```batch
@echo off
 pushd "%~dp0"
 dir /b %SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~3*.mum >List.txt
 dir /b %SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum >>List.txt
 for /f %%i in ('findstr /i . List.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
 pause
```

## 一、系统设置

把设置内容都过一遍，比较重要的是开发人员模式、文件资源管理器中的显示文件扩展名等设置。

下载并打开 Dism++，系统优化的选项全部过一遍。下面是其他手动优化：

- [Win+R 启用以管理员身份运行](https://blog.csdn.net/weixin_44510468/article/details/109798768)
- [Windows 11 的 C 盘 User（用户）文件夹下的用户文件夹名称的修改](https://blog.csdn.net/2302_79084172/article/details/131657427)
- [Win11 右键默认显示更多](https://zhuanlan.zhihu.com/p/626355083)

以后只要是你用着不爽的地方，上网搜就是了，改到爽为止，就这么简单。

## 二、终端设置

将终端添加到快捷启动栏，以后 `Win+数字键` 快速打开，设置过一遍。在终端已经开启的情况下可以使用 Win+`键打开 Quake 窗口。

### Win 终端

Windows 的终端建议安装 Scoop 包管理，然后安装 sudo。这里比较适合安装绿色版本的软件和小型非常用软件。我开始使用这个工具时感觉相见恨晚，虽然我的电脑软件基本都已经安装完毕，但是因为强迫症，我决定将大部分软件都使用 Scoop 重新安装一遍。建议先安装代理软件解决网络问题后再使用 Scoop 安装其他软件，之后一般的软件都可以考虑能否从此处安装。个人感觉缺点是 search 速度较慢。

使用命令 `scoop config gh_token <your token>` 配置 Github 的 API 令牌。

- [Scoop——也许是 Windows 平台最好用的软件（包）管理器](https://zhuanlan.zhihu.com/p/463284082)
- [Scoop 安装、使用与卸载](https://zhuanlan.zhihu.com/p/609534833)
- [搭建 Windows 统一开发环境（Scoop）](https://zhuanlan.zhihu.com/p/128955118)

### WSL 终端

我非常喜欢 WSL，我希望将 WSL 作为我的主力开发环境。~~不是因为 WSL 的话我真的会转战 Linux 的。~~

安装 WSL，建议安装官方支持的 WSL，我最开始安装的是应用商店里的 Arch，后来安装 Docker 出问题了，所以投敌 Debian 了。
- [在 Windows 上用 WSL 优雅开发](https://dowww.spencerwoo.com/)。

换源，设置中文（可选），安装软件。可以 [把 WSL 移到 D 盘](https://blog.csdn.net/weixin_42705114/article/details/131106845)。
- 问题及解决：[检测到 localhost 代理配置，但未镜像到 WSL](https://www.cnblogs.com/hg479/p/17869109.html)。

[WSL 中的高级设置配置](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)，设置 Win 系统中的 `.wslconfig` 与 WSL 中的 `wsl.conf`。

安装常用软件：`sudo apt install git,vim,gcc,g++,tree,curl,wget,python-is-python3,python3-pip`

终端主题：使用 `zsh`。安装 `oh-my-zsh`，配置主题为 `powerlevel10k`。这玩意看个人喜好了。

安装 `JDK`：官网下载，[下载账号分享](https://bugmenot.com/view/oracle.com)；[Debian10 安装 JDK8](https://blog.csdn.net/weixin_42218986/article/details/114698566) （在 IDEA 上运行构建需要 `JDK11` 或更高版本，建议放弃 `JDK8`，确有需要可以转战 Docker 或者在 Win 上安装）

命令行 GPT：[Shell GPT](https://github.com/TheR1D/shell_gpt)（可以临时查询些 Linux 指令，问其他的东西也很方便，~~而且真的很帅！~~）
- （个人使用：使用 `pipx`安装，API 使用 [DeepSeek](https://www.deepseek.com/zh)，配置参数：`~/.config/shell_gpt/.sgptrc`）
- 若希望免费使用，你也可以使用这个：[免费 API Key](https://github.com/chatanywhere/GPT_API_free)

ssh: `apt install openssh-server` [SSH 远程连接](https://zhuanlan.zhihu.com/p/355748937)。不知道为什么我用 Win 端 Navicat 连接数据库容易卡死，所以最近用 ssh 连接试试，用到目前为止连接都很稳定。

以下是我个人碰到的 WSL 相关问题，仅供参考，可能有其他解决办法。

首先是文件权限问题，由于 Win 系统与 Linux 系统的区别，建议关注这一点，通过 sudo 生成的文件的所有者是 root，如有需要应通过 `chown` 手动修改为当前用户。

VSCode 本身是微软家的，本身就采用 Remote 远程连接形式连接，对 WSL 支持也非常好，所以至少目前为止我没有在上面遇到过任何莫名其妙的问题，在其他 IDE 碰到什么问题解决不了可以回 VSCode 看看。

对于不是 Remote 远程连接形式进行使用的软件可能就会有问题。我首先是在 MySQL 中发现的问题，使用 Win 桌面端 Navicat 直接通过端口连接 MySQL 会导致 MySQL 服务卡死，连在 WSL 中查看状态和停止服务都不行，只能重启 WSL，重启了 WSL 又得重启 VSCode，就很麻烦，网上搜一圈加上问 GPT 都没有很好的解决办法，最后选择通过 SSH 通道连接，连接后很稳定，Navicat 也支持。

然后是 IntelliJ 家的 IDE，如果使用的是专业版建议使用 Remote 远程连接 WSL 模式而不是直接打开 WSL 文件。首先是 Java，调用 WSL 内的 JDK8 会导致编译不可用，但是 Win 本地的 JDK8 就没有问题，所以我猜测使用远程连接应该也没问题。其次，我明明在 WSL 上装了 Maven，检查过环境变量，结果就是检测不出我有 Maven，就算我选择 IDEA 自带捆绑的 Maven，也提示“Debian 没有已配置的 Maven”。同样是网上搜一圈加上问 GPT 都没有很好的解决办法。后来通过终端指令构建起来后通过 VSCode 就运行起来了，但是 IDEA 同样报错。所以要么使用 Remote 远程连接，要么在 WSL 中直接安装 IDEA。

## 三、软件安装

### 常用软件

安装 Office，这里我搞了半天 365 没成功，还是搞数字版（2019）了。[Office Tool Plus 入门教程](https://www.coolhub.top/archives/42)，可使用 Scoop 安装。

安装图吧工具箱，里面的两个工具强推，一个是 SpaceSniffer，一个是 Geek Uninstaller，建议使用 Scoop 额外安装一遍，非常常用。

安装 Bandizip6，取消自动更新。该版本为最后一个无广告的版本。不建议使用 Scoop 安装，会安装成便携版本。

安装 Everything，我曾经选择用系统的索引搜索文件，但后来感觉还是不如 Everything。我将 Everything 放在了快捷启动栏的二号位。

安装 Notepad3，替代系统自带 Notepad。可以把系统的 Notepad 删了，删了后 Notepad 右键新建文本文档可能没了，[解决办法在此](https://blog.csdn.net/Winkexin/article/details/131588977)。常用软件，不建议使用 Scoop 安装。

安装 Okular，PDF 阅读器。启动浏览器太重了，所以搞了个专用的软件，目前体验不错。目前无法通过 Scoop 安装，看了下发现是因为官网的下载链接失效了，目前我已经提了 Issue。

安装 PotPlayer，理由同 Everything，用系统自带的玩意还是不得劲。

安装 PowerToys，这是微软官方的增强工具可以玩玩。我非常喜欢 PowerToys Run，快捷启动真的很方便。
- [集成 PowerToys 和 Everything](https://zhuanlan.zhihu.com/p/577572778)（感觉集成了反而不太好用了，还是用原来的 Everything 比较好，所以我现在关掉了）

安装 [Shell](https://github.com/moudey/Shell)，扩展你的 Windows 右键功能。可通过 Scoop 安装。

安装 Link Shell Extension，方便地创建符号链接等链接，具体请见下文。

安装 SwitchHosts，设置开机运行。[Github Hosts](https://github.com/ineo6/hosts) （这玩意也不能保证你不开代理的情况下稳定访问，只能说聊胜于无吧）

安装上网时需要用到的神秘妙妙工具！建议在安装 Scoop 前安装。如果你使用 bing 搜索引擎，建议设置“绕过域/网络”，添加"*.bing.com"，避免 bing 打开过慢的情况。

- [Steam 如何绕过 clash 的全局代理](https://cornradio.github.io/hugo/posts/%E8%AE%A9steam%E7%BB%95%E8%BF%87clash%E7%B3%BB%E7%BB%9F%E4%BB%A3%E7%90%86/)

安装 IDM，下载工具。

### 浏览器与拓展

在电脑上我一般使用双浏览器，主力为 Edge（考虑更换为开发者版），副手为 Firefox 开发者版。

说到浏览器就不得不提浏览器拓展了，这里我会将拓展也视为应用程序。

RSSHub Rader：RSS 订阅工具。一直以来我都想摸索出一套使用 RSS 订阅信息的最佳实践，但是没摸索出来，有空再研究研究，我是受够了现在的大数据推送信息流了。

Sider：侧边栏 GPT，可以结合上文提到的 API 使用。

uBlock Origin：开源的广告拦截插件，最近才用，感觉还行。

沉浸式翻译：翻译插件，可以在网页上同时显示中英文，还能翻译 PDF，非常推荐。

简悦：沉浸阅读插件，如果你受够了 CSDN 的各种花里胡哨，双击 A 键试试吧？还可以将网页以 Markdown 格式保存到本地，从而构建起信息收集的一环。这方面我也正在摸索中。

TechStack：显示 Github 仓库项目技术栈。非 Edge 插件，从谷歌插件商店中获取。

Stylus：CSS 样式插件，你可以理解成 CSS 版本的油猴，可以通过 CSS 语法修改网页元素。与油猴一样拥有 CSS 样式相关的网站。非 Edge 插件，从谷歌插件商店中获取。我目前安装了两个，一个是 B 站首页搜索引擎样式，一个是极简知乎。

油猴：不用我介绍了吧？写一下我现在用的几个：Github 增强-高速下载、Qzone Cleaner（QQ 空间纯净版）、知乎增强、自动无缝翻页、CSDNGreener、OCS（这个懂得都懂了）。

### 国产软件

安装火绒，各个功能简单过一遍。

火绒与 360 的选择：360 纯净版确实不错，不过还是建议使用火绒。你需要的不是杀毒能力强悍的杀毒软件，而是一个良好的电脑使用习惯。这里就体现了沙盒的重要性，可以跑一些不确定有没有病毒的软件。个人的看法是 360 应该作为核武使用，在病毒攻破电脑前绝不率先使用。

安装 QQ，存储设置看一下。

安装欧陆词典。我感觉这玩意不好使，想换成其他的翻译软件了。

安装坚果云。可以折腾折腾 Obsidian 多端同步什么的，待摸索中。

安装小米妙想，B 站可以搜到第三方电脑安装教程。

阿里网盘/百度网盘/迅雷：还没想好怎么装，装了个百度网盘的纯净旧版本。

### 开发软件

需要什么装什么就行了。

安装 Docker，也可以把相应的 WSL 移到 D 盘。[WSL 2 上的 Docker 远程容器入门](https://learn.microsoft.com/zh-cn/Windows/WSL/tutorials/WSL-containers)；[Docker 快速入门](https://docker.easydoc.net/)。

安装 VSCode，简单登录配置一下。改天有时间了我也会放出我使用的插件列表。Visual Studio 我还没安装，需要的时候再安装吧。

安装 IDEA 和 PyCharm，Pycharm 感觉对于 WSL 支持得不太好，在 Win 本地再补一个 Python 吧。

安装 Navicat。

### 可选软件

安装 Zlibrary。

安装 Telegram。现在成为了我为数不多的信息来源。

文件去重：Duplicate Cleaner。

文件整理：Dropit。暂时没装。

本地笔记：Obsidian。还没怎么研究透。普通 Markdown 文件适合用普通轻量的编辑器打开，Obsidian 用于做成体系的笔记。

调控性能：Universal x86 Tuning Utility。有人推荐我这电脑用这玩意，英语很差，暂时没搞明白。

平板/手机模拟触控板：VirtualTablet Server。个人感觉不算好用，仅记录。

B 站视频下载工具：DownKyi。可使用 Scoop 安装。

远程连接：ToDesk。这个是最近同学给我安利的，用着确实不错。

### 游戏软件

游戏平台：Steam，Epic，其他的平台还有 GOG/itch/Ubisoft，我懒得装了。可以下个 Steam++。

Epic 领游戏的话，可以配置下看能不能 Docker 自动领，我暂时没折腾。目前的办法是使用 RSSHub 订阅了 Epic 免费游戏，碰到感兴趣的游戏就可以自己去领了。

加速器目前就装了个小黑盒，作为 Steam 专项加速。

游戏 mod：除了创意工坊之外，杀戮尖塔可以打一下 anaertailin 的人物 mod，蔚蓝可以打一下 Miao 服国内联机的群服 mod，星露谷也有很多 mod 可以打，改天整理一下。

曾经是明日方舟玩家，安卓模拟器方面安装过雷电模拟器，所以在这说一下安卓模拟器与 Hyper-V 的兼容问题。因为 Hyper-V 的存在，导致安卓模拟器和虚拟机软件都不能很好地运行，如果需要运行虚拟机我建议直接使用专业版 Hyper-V，至于安卓模拟器的话，据说蓝叠支持了 Hyper-V，我个人没有具体考证；逍遥明确写明了支持 Hyper-V。本人之前在游玩明日方舟时选择的是雷电，虽然与 Hyper-V 不太兼容，不过流畅度尚可，相比逍遥有预置键位。玩其他手游并且在预置逍遥有键位的 或 对预置键位需求不大的 建议选择逍遥。明日方舟玩家可将配置调整为 2 核 2G 效果更佳。

## 四、文件管理

这里是我个人目前的文件管理，仅供参考，以后可能会有变动吧。其实我还有一堆杂乱的文件没有整理，有空有心思再说罢。

### 关于硬盘分区

首先是分区，可能表述得不会很严谨。

如果你使用的是机械硬盘，文件的读写速度部分取决于文件放置的位置，一般来说放在硬盘前部的文件比放在硬盘后部的文件速度要快一点，所以个人建议分为三个区，即系统、常用软件与文件、存档文件。当然多分几个区也是没问题的。

如果你使用的是固态硬盘的话就没这么多讲究了，但与机械硬盘相反，少分点区比较好，也更加方便。因为考虑到重置或重装系统可能带来的问题，我选择分为两个区，系统单独一区。下面以我这种做法为例，其实如果选择不分区的话会不用那么麻烦。

### jwj 的 D 盘根目录

C 盘只负责系统以及不由分说硬是要安装到 C 盘的软件，所以一般软件和文件我会全部放置在 D 盘。我目前的 D 盘根目录下的文件夹有下面几个：

`App_Files` / `Game_Files`：保存无安装程序的绿色软件/游戏软件，例如图吧工具箱等。比较常用的我会制作快捷方式，放入开始菜单文件夹，并通过 PowerToys Run 快捷启动。

`Program Files` / `Program Files (x86)`：软件安装位置。对于能够让我们选择安装在哪里的软件，我会安装在这里，通常情况下只需要更改安装盘符为 D 盘即可。我将 Scoop 的安装位置也放在了这里，但我认为还是在根目录新建文件夹为好，方便管理。

`Users`：用户文件，即日常保存文件的地方。系统的分类文件夹搬迁至此，同时新建一些文件夹比如说工作目录，并固定到快速访问中。

`Virtual`：虚拟化，我把 WSL 和 Docker 所需的 WSL 移至这里。

`WindowsApps` / `Wpsystem`：通过应用商店安装的程序。这个安装位置也是可以更改的，具体不再展开。

### 具体文件存放位置

具体文件放在哪呢？我选择放在系统提供的用户文件夹之下。按照以往分了几个区的习惯我会把文件放在专门放文件分区的根目录，但是现在提供放置文件的分区只有一个，放在根目录多少有些不雅观，正好用户文件夹下有一些现成的分类（文档图片音乐视频之类的），索性放在这里好了。这些默认分类可以右键属性，可以调整文件位置，我把所有默认分类全部移到了 D 盘，只更改了一个盘符。除了默认分类以外我也建立了一些文件夹作为自己的分类，如工作目录、未分类文件夹。

这里值得一提的是文档这个文件夹，很多应用程序的数据会默认放置在这里，所以我在文档这个默认分类之下又创建了一个名为“文档”的文件夹，这里才是保存个人文档的位置。然后就可以换一个图标并固定到快捷访问栏里了。

### 关于符号链接

还记得上文提到的 Link Shell Extension 吧，就是用来方便地创建符号链接等特殊链接的。

如果你不明白这些是什么意思，我这里挑了几篇文章：
- [符号链接、硬链接及其在 Windows 上的应用举例](https://sspai.com/post/66834)
- [Windows 的快捷方式，符号链接，软链接和硬链接](https://sunocean.life/blog/blog/2020/09/10/win-mklink)
- [LinkShellExtension 以及 Undrgrnd 风格美化](https://www.bilibili.com/read/cv28936476/)。

那么我用这玩意干啥了呢？ ~~其实也没干啥。~~ 由于软件默认的行为，还是需要用到 C 盘用户文件夹的，毕竟是用户的主目录，所以我给几个在 D 盘的文件夹创建了符号链接，并放在了 C 盘用户文件夹下，具体而言目前有这几个：`App_Files`、`Game_Files`、`安装包`、`工作目录`。

这玩意相比快捷方式而言有什么好处么？举个例子，比如说你打开了位于 C 盘的符号链接 `工作目录`，此时你会发现这时地址栏显示的还是 C 盘，但事实上文件是存储在 D 盘的。这么做的话，`工作目录`这个文件夹可以直接出现在目录树中对应的位置，而不是说因为它是快捷方式所以压根不显示，这样的特性会在很多软件中都会有所体现。

不过得注意对于有些软件还是会报错的，比如我将 WSL 目录中的 `Codespace`文件夹创建符号链接到 `工作目录`这个文件夹内，然后通过 IDEA 直接通过符号链接打开 Codespace 内部的文件，然后就会报错，还是得从 WSL 自身的文件路径打开。但是抛开软件报错这点不谈，这么做确实为我的文件管理带来不少便利。

当然，如果你嫌麻烦，快捷方式也是不错的选择，我也是这次重装系统后拿来尝尝鲜的。

### 关于文件整理

这里差不多就是待填坑了，我常用的文件倒是放的好好的，但以前保存的很多资料我都没有整理，扔在了“未整理”文件夹。

我记得我曾经用过一款软件叫做 Dropit，可以自动分类文件的，安利一下，等我整理文件的时候我肯定会下下来用的。

## 五、桌面美化

虽说最好的美化是默认，但下面这些美化选项除了 Rainmeter 之外也不会耗费多少时间，何乐而不为？

安装 Bing 壁纸，非常简单。我现在不使用 Bing 壁纸了，现在的壁纸是这个：[Yosemite Color Block](https://simpledesktops.com/browse/desktops/2015/jul/11/yosemite-color-block/)。

安装 TranslucentTB（在微软商店下载），用来美化任务栏。设置成白色，透明度 100 就可以了。

可选：安装 Rainmeter，仅安装一个时间组件。[时间日期插件](https://www.zhihu.com/question/389422259/answer/2439637893)

## 六、开机启动

现在的启动文件夹在哪呢？`Win+R`或者使用 PowerToys Run 运行 `shell:startup`就可以打开启动目录了，然后把快捷方式扔进去就行了。

我还有两个需求，第一是本地开机后台运行 Github Hosts Server，第二是运行 WSL 中的 MySQL，分别对应的是 Win 系统下的命令行应用和 WSL 中的应用服务。我试过在虚拟机运行 Github Hosts Server，但当时并未成功，我觉得可能是因为那时候并未解决如上文所说的 `检测到 localhost 代理配置，但未镜像到 WSL`，所以我还是在本地运行了。

然后我查了些资料，下面是链接：

- [win10 子系统 (WSL) 开机启动以及启动脚本](https://www.icksky.com/notes/win10-sub-system-auto-start-scripts/)
- [Windows 通过 cmd 后台运行进程方法](https://zhuanlan.zhihu.com/p/658030901)
- [隐藏 CMD 窗口，使程序在后台执行](https://blog.csdn.net/weixin_44259233/article/details/115969473)

后来我是这么做的：

`startup.cmd`：

```batch
@echo off
title startup

if "%1"=="h" goto begin
start mshta vbscript:createobject("wscript.shell").run("""%~nx0"" h",0)(window.close)&&exit

:begin

wt -w _quake powershell -nologo -window minimized
powershell Start-Process -WindowStyle hidden -FilePath "D:\App_Files\hosts-server-pkg-win-x64\hosts-server.exe" -ArgumentList "--port=8888"
powershell Start-Process -WindowStyle hidden -FilePath "D:\App_Files\bat\startup\WSL_init.cmd"
```

`WSL_init.cmd`：

```batch
bash -c "sudo /home/cmdjwj/.init.sh"
```

（WSL 中）`.init.sh`：

```bash
#!/bin/sh
sudo service mysql start
sudo service ssh start
```

~~呃，看起来是有点脱裤子放屁的味道。~~ 感觉可以问问 GPT 看它怎么说。

这样的效果就是，开机后会有一个黑影闪过，稍等片刻基本就能使用。第一个脚本中的 powershell 指令设置了后台运行的参数，让命令行工具与脚本后台运行。

然后我发现了 **第二种启动 WSL 内服务的方法：** 还记得上文我提到过的这篇官方文章么？[WSL 中的高级设置配置](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)，只需要在 WSL 内的 `/etc/wsl.conf` 中的 `[boot]`下面加上一条 `command = service mysql start && service ssh start` 就行了！然后我就感觉有点奇怪，为啥网上都没人提这一条啊？

## 七、虚拟机设置

这里我选择直接使用 Hyper-V。

IP 地址问题是需要注意一下的，这是我使用的解决办法，经测试可用 ssh 连接：[关于 Hyper-V 网络设置经验总结](https://blog.csdn.net/jasonhwang/article/details/115287437) 如果使用这种办法，那么之后的虚拟机都是可以使用这个自建虚拟网卡的，虚拟机内配置即可。
也可以试试配置 NAT+端口映射的办法，不过我觉得如果是虚拟机的话说不定用这种方式更合适。

---

## 总结

不得不说，折腾新的系统以及写这篇文章，虽然乐于折腾的我乐在其中，但是确实耗费了我大量的时间，是有些得不偿失的，这也是为什么很多人说美化的尽头是默认，不无道理。

另外，身处大模型时代，如果你想写一段什么脚本（比如说开机启动的脚本），你完全可以交给 GPT 来做，从而节省时间。

说到底，调教电脑，目的还是为了自己使用电脑用得舒心安心。我是非常不喜欢 Win 的命令行，也多亏是 WSL 将我继续挽留在 Win 系统中，目前用着很舒服。

希望这篇文章对于正在调教电脑的你有所帮助！

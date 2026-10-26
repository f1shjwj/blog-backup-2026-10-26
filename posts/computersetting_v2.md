---
title: jwj 的电脑调教记录 v2.0
date: 2025/02/13 13:19:00
categories:
- 记录
tags:
- 系统设置
thumbnail:
---
一年过去了，我又开始对系统环境不满意了，很想重装，尤其是想换到 Linux 环境去）还是将记录更新一下。主要基于上篇博客进行修改。

<!-- more -->

电脑：机械革命 Code01 V2.0 R7-6800H 16G

## 零、系统

将系统版本改为 [Win11 专业版](https://zhuanlan.zhihu.com/p/637930518)，按照评论区的方案来做。这是 Hyper-V 和沙盒的前置条件。

添加功能中添加 WSL、Hyper-V、沙盒等虚拟化功能。

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

通过 Scoop 下载并打开 Dism++，系统优化的选项全部过一遍。下面是其他手动优化：

- [Win+R 启用以管理员身份运行](https://blog.csdn.net/weixin_44510468/article/details/109798768)
- [Windows 11 的 C 盘 User（用户）文件夹下的用户文件夹名称的修改](https://blog.csdn.net/2302_79084172/article/details/131657427)
- [Win11 右键默认显示更多](https://zhuanlan.zhihu.com/p/626355083)

以后只要是你用着不爽的地方，上网搜就是了，改到爽为止，就这么简单。

## 二、终端设置

我厌倦了 Windows Terminal，所以开始使用 WezTerm，它是跨平台的，所以未来我转 Linux 时可以参考配置。

安装完毕后将 WezTerm 固定于任务栏，之后可用 `Win+数字键` 快速打开。

安装字体：建议安装一份你喜欢的 Nerd Fonts 与一份思源黑体。

在用户根目录下新建 `.wezterm.lua`文件并编辑配置：（个人配置，仍未完善，仅供参考）

```lua
local wezterm = require 'wezterm'
local config = wezterm.config_builder()

local my_font = wezterm.font('Hack Nerd Font')

config.color_scheme = 'Dracula+'
config.term = 'wezterm'
config.colors = {
    scrollbar_thumb = 'rgba(51, 51, 51, 0.95)',
}
config.font = my_font
config.font_size = 14

config.enable_scroll_bar = true
config.min_scroll_bar_height = 32

config.front_end = 'WebGpu'
config.window_background_opacity = 0.95
config.window_decorations = "INTEGRATED_BUTTONS|RESIZE"
config.window_frame = {
    font = my_font,
    font_size = 12,
}

config.scrollback_lines = 10000
config.default_cursor_style = 'SteadyBar'

config.launch_menu = {
    {
      label = 'WSL - Codespace',
      args = {'wsl', '--cd', '~/codespace'},
    },
    {
      label = 'PowerShell - Home',
      args = {'powershell', '-NoLogo'},
      cwd = wezterm.home_dir,
    },
}

config.default_prog = { 'wsl', '--cd', '~/codespace' }

config.wsl_domains = {
    {
      name = 'WSL:Debian',
      distribution = 'Debian',
      default_cwd = '~/codespace',
    },
}

return config
```

### Powershell & cmd

包管理：Scoop

使用命令 `scoop config gh_token <your token>` 配置 Github 的 API 令牌。

- [Scoop——也许是 Windows 平台最好用的软件（包）管理器](https://zhuanlan.zhihu.com/p/463284082)
- [Scoop 安装、使用与卸载](https://zhuanlan.zhihu.com/p/609534833)
- [搭建 Windows 统一开发环境（Scoop）](https://zhuanlan.zhihu.com/p/128955118)

推荐安装：[gow](https://github.com/bmatzelle/gow)。

### WSL

我觉得这一部分可以单独开一篇博客来讲，因为这边基本上就是 Linux 环境配置，所以这里我会压缩一些 Linux 配置本身的篇幅。

建议安装官方支持的 WSL，不要安装第三方 WSL 环境，个人推荐 Debian。

- [在 Windows 上用 WSL 优雅开发](https://dowww.spencerwoo.com/)
- [将 WSL 移到 D 盘](https://blog.csdn.net/weixin_42705114/article/details/131106845)
- [检测到 localhost 代理配置，但未镜像到 WSL](https://www.cnblogs.com/hg479/p/17869109.html)
- [WSL 中的高级设置配置](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)，设置 Win 系统中的 `.wslconfig` 与 WSL 中的 `wsl.conf`

以下是我个人碰到的 WSL 相关问题，仅供参考，可能有其他解决办法。

使用 Win 端 Navicat 直接连接数据库会莫名其妙卡死，暂无直接解决方案，可用 ssh 间接连接使用。

目前对 WSL 支持最好的应该是 VS Code Remote，之前使用 IntelliJ 系列会出现问题，不知道现在如何，同样建议使用 Remote。

最后，如果真的很喜欢 Linux 终端环境，我觉得还是直接去用 Linux 系统比较好。至少我自己是这么想的。WSL 并不完美，也会出现问题。

## 三、软件安装

### 常用软件

Office：使用 Office Tool Plus 安装。[Office Tool Plus 入门教程](https://www.coolhub.top/archives/42)

Bandizip6：压缩软件。该版本为最后一个无广告的版本，安装后需要取消自动更新。不建议使用 Scoop 安装，会安装成便携版本。

Notepad3：替代系统自带 Notepad。可以把系统的 Notepad 删了，删了后 Notepad 右键新建文本文档可能没了，[解决办法在此](https://blog.csdn.net/Winkexin/article/details/131588977)。常用软件，不建议使用 Scoop 安装。

图吧工具箱：工具合集。里面的两个工具强推，一个是 SpaceSniffer，一个是 Geek Uninstaller，建议使用 Scoop 额外安装一遍，非常常用。

Everything：搜索工具。我将 Everything 放在了快捷启动栏的二号位。

Okular：PDF 阅读器。我现在有点想换掉了。

PotPlayer：视频播放器。

PowerToys：微软官方的系统增强工具。我现在已经离不开快捷启动器 PowerToys Run 了。

[Shell](https://github.com/moudey/Shell)：扩展你的 Windows 右键功能。

Link Shell Extension：方便地创建符号链接等链接。

SwitchHosts：Hosts 管理工具。设置开机运行。[Github Hosts](https://github.com/ineo6/hosts) （我现在觉得这玩意聊胜于无，不如直接开代理）

代理工具，各凭本事。建议在安装 Scoop 前安装。如果你使用 bing 搜索引擎，建议设置“绕过域/网络”，添加"*.bing.com"，避免 bing 打开过慢的情况。[Steam 如何绕过 clash 的全局代理](https://cornradio.github.io/hugo/posts/%E8%AE%A9steam%E7%BB%95%E8%BF%87clash%E7%B3%BB%E7%BB%9F%E4%BB%A3%E7%90%86/)

IDM：下载工具。现在我不太喜欢这个。

### 浏览器与扩展

使用浏览器：Vivaldi。备用：暂时为 Edge。以下为浏览器拓展。

ChatGPTBox：调用 GPT API 的开源插件。

KeePassXC-Browser：密码管理工具 KeePassXC 配套插件。

RSSHub Rader：RSS 订阅工具。

TechStack：显示 Github 仓库项目技术栈。

uBlock Origin：开源的广告拦截插件。

沉浸式翻译：翻译插件。建议选择使用大模型翻译。

简悦：可用于开启阅读模式、导出、剪藏等。

Stylus：可以理解成 CSS 版本的油猴。目前安装了两个，B 站首页搜索引擎样式、极简知乎。

油猴：无需多言。目前使用的：Github 增强-高速下载、Qzone Cleaner（QQ 空间纯净版）、知乎增强、CSDNGreener

### 开发软件

需要什么装什么就行了。

Docker：可以把相应的 WSL 移到 D 盘。[WSL 2 上的 Docker 远程容器入门](https://learn.microsoft.com/zh-cn/Windows/WSL/tutorials/WSL-containers)；[Docker 快速入门](https://docker.easydoc.net/)。

#### VSCode

以下为 VSCode 插件。顺带一提，对于开发人员来说笔记软件用 Obsidian 不如直接用 VSCode+Foam。

AI 系列：

- Github Copilot
- Continue：开源 Copilot

Docker 系列：

- Docker
- Dev Containers

翻译系列：

- 翻译（英汉词典）
- Comment Translate

Python 系列、C/C++ 系列

Code Runner：代码运行（感觉用处也不大，我用命令行用惯了）

Markdown/笔记系列：

- Markdown All in One
- Markdown Notes
- Office Viewer(Markdown Editor)
- Pangu-Markdown
- Markdown+Math
- Tex Preview
- Foam

Git 系列：

- Git Graph
- Git History
- GitLens
- Add to GIT Ignore

Web：

- Preview on Web Server
- REST Client

数据库：

- SQLTools

其他：

- Makefile Tools
- Rainbow CSV
- WakaTime

### 可选软件

Duplicate Cleaner：文件去重。

Dropit：文件整理。

DownKyi：B 站视频下载工具

### 游戏软件

Steam++：Steam 辅助工具，附带一些网站加速。已经很久没打开过了。

小黑盒加速器：我个人用来作为 Steam 专项加速。

游戏 mod：除了创意工坊之外，杀戮尖塔可以打一下 anaertailin 的人物 mod，蔚蓝可以打一下 Miao 服国内联机的群服 mod，星露谷也有很多 mod 可以打，改天整理一下。

雷电模拟器：明日方舟启动器。说一下安卓模拟器与 Hyper-V 的兼容问题。因为 Hyper-V 的存在，导致安卓模拟器和虚拟机软件都不能很好地运行，如果需要运行虚拟机我建议直接使用专业版 Hyper-V，至于安卓模拟器的话，据说蓝叠支持了 Hyper-V，我个人没有具体考证；逍遥明确写明了支持 Hyper-V。本人之前在游玩明日方舟时选择的是雷电，虽然与 Hyper-V 不太兼容，不过流畅度尚可，相比逍遥有预置键位。玩其他手游并且在预置逍遥有键位的 或 对预置键位需求不大的 建议选择逍遥。明日方舟玩家可将配置调整为 2 核 2G 效果更佳。

## 四、文件管理

### 关于硬盘分区

机械硬盘：文件的读写速度部分取决于文件放置的位置，一般来说放在硬盘前部的文件比放在硬盘后部的文件速度要快一点，所以个人建议分为三个区以上，即系统、常用软件与文件、存档文件。

固态硬盘：分为两个区，C 盘作为系统盘尽量只存放系统文件，D 盘存放软件与个人文件，通过软链接与用户根目录建立连接。对于一些系统文件不必执着于应搬尽搬的原则，很多时候属于吃力不讨好，不如在需要备份时老老实实地多看两眼 C 盘。

### jwj 的 D 盘根目录

C 盘只负责系统以及不由分说硬是要安装到 C 盘的软件，所以一般软件和文件我会全部放置在 D 盘。我目前的 D 盘根目录结构如下：

`App_Files` / `Game_Files`：保存无安装程序的绿色软件/游戏软件，例如图吧工具箱等。比较常用的我会制作快捷方式，放入固定文件夹中，将文件夹通过软链接链接到开始菜单，并通过 PowerToys Run 快捷启动。

`Program Files` / `Program Files (x86)`：软件安装位置。对于能够让我们选择安装在哪里的软件，我会安装在这里，通常情况下只需要更改安装盘符为 D 盘即可。Scoop 更建议安装在根目录，方便管理。

`Users`：用户文件，即日常保存文件的地方。系统的分类文件夹搬迁至此，同时新建一些文件夹比如说工作目录，并固定到快速访问中。

`Virtual`：虚拟化，我把 WSL 和 Docker 所需的 WSL 移至这里。

`WindowsApps` / `Wpsystem`：通过应用商店安装的程序。这个安装位置也是可以更改的，具体不再展开。

### 具体文件存放位置

用户根目录下，与 Linux 类似。默认分类可以右键属性调整文件夹位置，将所有默认分类全部移到 D 盘，只更改一个盘符即可。除了默认分类以外我也建立了一些文件夹作为自己的分类，如工作目录、未分类文件夹，并通过符号链接链接到根目录。建议额外新建一个文档文件夹并链接到根目录。

### 关于符号链接

- [符号链接、硬链接及其在 Windows 上的应用举例](https://sspai.com/post/66834)
- [Windows 的快捷方式，符号链接，软链接和硬链接](https://sunocean.life/blog/blog/2020/09/10/win-mklink)
- [LinkShellExtension 以及 Undrgrnd 风格美化](https://www.bilibili.com/read/cv28936476/)。

## 五、桌面美化

非常简洁简单的美化，不会消耗多少时间。

壁纸：[Simple Desktops](https://simpledesktops.com/)

TranslucentTB：美化任务栏，在微软商店下载。个人设置：白色，透明度 100 。

Rainmeter：仅安装一个时间组件。[时间日期插件](https://www.zhihu.com/question/389422259/answer/2439637893)

## 六、开机启动

启动文件夹打开方式：`Win+R`或者使用 PowerToys Run 运行 `shell:startup`。

- [win10 子系统 (WSL) 开机启动以及启动脚本](https://www.icksky.com/notes/win10-sub-system-auto-start-scripts/)
- [Windows 通过 cmd 后台运行进程方法](https://zhuanlan.zhihu.com/p/658030901)
- [隐藏 CMD 窗口，使程序在后台执行](https://blog.csdn.net/weixin_44259233/article/details/115969473)

自己编写的脚本就在此省略了。烦死了，想去 Linux。

## 七、虚拟机设置

这里我选择直接使用 Hyper-V。

IP 地址问题是需要注意一下的，这是我使用的解决办法，经测试可用 ssh 连接：[关于 Hyper-V 网络设置经验总结](https://blog.csdn.net/jasonhwang/article/details/115287437) 如果使用这种办法，那么之后的虚拟机都是可以使用这个自建虚拟网卡的，虚拟机内配置即可。
也可以试试配置 NAT+端口映射的办法，不过我觉得如果是虚拟机的话说不定用这种方式更合适。

---

## 总结

当初我为了配置耗费了大量的时间，但是现在的我仍然很绝望，越来越想转 Linux 了。

身处大模型时代，如果你想写一段什么脚本（比如说开机启动的脚本），你完全可以交给 GPT 来做，从而节省时间。

说到底，调教电脑，目的还是为了自己使用电脑用得舒心安心。希望这篇文章对于正在调教电脑的你有所帮助！

---
title: 在手机上使用 termux 写 C++代码
date: 2020/02/24 21:43:00
categories: 
- 技术
tags: 
- Linux
thumbnail: https://image.3001.net/images/20180501/15251875958364.png
---

本文将会介绍如何使用 termux 写 C++代码并编译运行。

[头图来源](https://www.sqlsec.com/2018/05/termux.html)

<!-- more -->

-------

## 下载安装 termux

下载安装前手机建议腾出 500MB 以上的空间。
如果可以科学上网建议在谷歌应用商店下载。
如果不能科学上网建议在酷安下载。

- **谷歌的 termux 的下载链接：[点此下载](https://play.google.com/store/apps/details?id=com.termux)**
- **酷安的 termux 的下载链接：[点此下载](https://www.coolapk.com/apk/com.termux)**

## 更换国内镜像源

安装好后建议先换上国内镜像源，避免未科学上网时下载失败或网速过慢。

- **更换清华大学镜像源使用如下命令**

```
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux stable main@' $PREFIX/etc/apt/sources.list
```

相关帮助链接：[帮助链接](https://mirror.tuna.tsinghua.edu.cn/help/termux/)

- **更换中国科学技术大学镜像源使用如下命令**

```
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.ustc.edu.cn/termux stable main@' $PREFIX/etc/apt/sources.list
```

相关帮助链接：[帮助链接](http://mirrors.ustc.edu.cn/help/termux.html)

换源后应及时更新镜像源，使用如下命令：

```
apt clean
pkg up
```

如国内镜像源出现问题、出现错误或不能下载到想要的软件，可以换回官方镜像源：

```
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://termux.net/ stable main@' $PREFIX/etc/apt/sources.list
```

当无法连接官方镜像源时应科学上网再次尝试。

## linux 基础命令

termux 是一个免 root 的 linux 模拟器，所以使用时应掌握一些 linux 的命令。当然没掌握也没关系，下面会有一些 linux 的基础命令供参考。

| 命令 | 含义 |
|:----------:|:----------:|
| cd <目录地址> | 打开目录地址 |
| cp <文件名> <目标目录地址> | 拷贝到目标目录地址 |
| mkdir <文件夹名> | 新建文件夹 |
| rm <文件名> | 删除文件 |
| rm -rf <文件夹名> | 删除文件夹 |
| ls | 查询该文件夹下文件及文件夹 |

更多命令请自行搜索相关资料。

## 安装软件及工具

这里给出安装我们需要的软件的命令，可根据需求自行更改。

如使用官方镜像源，请科学上网。

```
pkg install vim clang python gdb cgdb curl
```

- vim 为编辑器，可自行更改为其它编辑器，但非常建议使用 vim。学习 vim 的操作不会很难，常用的命令只有那么几个，习惯了就好。
- curl 可以优化 termux 终端界面，酌情下载安装。
- clang 为 C 和 C++编译器。
- python 可以让我们制作一些对拍小程序，也有很多其它的玩法，酌情下载安装。
- gdb 和 cgdb 为调试工具，可根据需要酌情下载安装。
- 也可自行安装其它软件及工具。

## vim 的使用

下面是 vim 的一些简单命令。

| 命令 | 含义 |
|:----------:|:----------:|
| vim <文件名> | 编辑文件，若文件不存在则自动创建新文件 |
|    i    |  切换到编辑模式        |
| ESC |  退出编辑模式        |
|   :x   |  保存退出        |
|   :q   |  未改动退出       |
|  :q!   |  不保存退出    |

更多命令请自行搜索相关资料。

## C++的编译与运行

### 编译

切换到 cpp 文件目录（cd <目录>）后命令如下：

```
g++ <cpp 代码文件> -o <生成程序名>
```

如：

g++ hello.cpp -o hello

即可在该目录下生成名为 hello 的可执行文件。

### 运行

```
./<可执行文件名>
```

如：

./hello

即可执行 hello 程序。

如出现错误：permission denied

则说明没有权限访问该程序，此时输入：

```
chmod +x <程序名>
```

如：

chmod +x hello

即可执行。

## 可选步骤

以下为可选步骤，但非常建议做一做。

### 创建工作目录

建议在根目录中新建一个文件夹作为工作目录，如果你比较懒，之后直接在根目录中编辑文件也是可以的。

名字建议用纯英文字母，方便进入。

建议不使用会与命令起冲突的名字，比如 gcc，cpp。

### 配置 vim

其实可以不配置，但是非常建议配置。

配置之后会给打代码增添不少便利。

执行如下命令：

```
vim ~/.vimrc
```

将以下内容复制粘贴进去保存即可。

可根据需要自行修改。

```
" 设置当文件被改动时自动载入"
set autoread
" quickfix 模式"
autocmd FileType c,cpp map <buffer> <leader><space> :w<cr>:make<cr>
"代码补全 "
set completeopt=preview,menu 
"允许插件  "
filetype plugin on
"共享剪贴板  "
set clipboard=unnamed 
"从不备份  "
set nobackup
"自动保存"
set autowrite
set ruler  " 打开状态栏标尺"
set cursorline  "突出显示当前行"
set magic  " 设置魔术"
set guioptions-=T  "隐藏工具栏"
set guioptions-=m  "隐藏菜单栏"
set foldcolumn=0
set foldmethod=indent 
set foldlevel=3 
set foldenable  " 开始折叠"
" 不要使用 vi 的键盘模式，而是 vim 自己的"
set nocompatible
" 语法高亮"
set syntax=on
" 去掉输入错误的提示声音"
set noeb
" 在处理未保存或只读文件的时候，弹出确认"
set confirm
"自动缩进 "
set autoindent
set cindent
" Tab 键的宽度"
set tabstop=4
" 统一缩进为 4"
set softtabstop=4
set shiftwidth=4
" 不要用空格代替制表符"
set noexpandtab
" 在行和段开始处使用制表符"
set smarttab
" 显示行号"
set number
" 历史记录数"
set history=1000
"禁止生成临时文件"
set nobackup
set noswapfile
"搜索忽略大小写"
set ignorecase
"搜索逐字符高亮"
set hlsearch
set incsearch
"行内替换"
set gdefault
"编码设置"
set enc=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
"语言设置"
set langmenu=zh_CN.UTF-8
set helplang=cn
" 我的状态行显示的内容（包括文件类型和解码）"
set statusline=[%F]%y%r%m%*%=[Line:%l/%L,Column:%c][%p%%]
" 总是显示状态行"
set laststatus=2
" 命令行（在状态行下）的高度，默认为 1，这里是 2"
set cmdheight=2
" 侦测文件类型"
filetype on
" 载入文件类型插件"
filetype plugin on
" 为特定文件类型载入相关缩进文件"
filetype indent on
" 保存全局变量"
set viminfo+=!
" 带有如下符号的单词不要被换行分割"
set iskeyword+=_,$,@,%,#,-
" 字符间插入的像素行数目"
set linespace=0
" 增强模式中的命令行自动完成操作"
set wildmenu
" 使回格键（backspace）正常处理 indent, eol, start 等"
set backspace=2
" 允许 backspace 和光标键跨越行边界"
set whichwrap+=<,>,h,l
" 可以在 buffer 的任何地方使用鼠标（类似 office 中在工作区双击鼠标定位）"
set mouse=a
set selection=exclusive
set selectmode=mouse,key
" 通过使用：commands 命令，告诉我们文件的哪一行被改变过"
set report=0
" 高亮显示匹配的括号"
set showmatch
" 匹配括号高亮的时间（单位是十分之一秒）"
set matchtime=1
" 光标移动到 buffer 的顶部和底部时保持 3 行距离"
set scrolloff=3
" 为 C 程序提供自动缩进"
set smartindent
" 高亮显示普通 txt 文件（需要 txt.vim 脚本）"
 au BufRead,BufNewFile *  setfiletype txt
"自动补全"
:inoremap ( ()<ESC>i
:inoremap ) <c-r>=ClosePair(')')<CR>
:inoremap { {<CR>}<ESC>O
:inoremap } <c-r>=ClosePair('}')<CR>
:inoremap [ []<ESC>i
:inoremap ] <c-r>=ClosePair(']')<CR>
:inoremap " ""<ESC>i
:inoremap ' ''<ESC>i
function! ClosePair(char)
    if getline('.')[col('.') - 1] == a:char
        return "/<Right>"
    else
        return a:char
    endif
endfunction
filetype plugin indent on 
"打开文件类型检测，加了这句才可以用智能补全"
set completeopt=longest,menu
"C++的编译和运行  "
"编译运行"
map <F10> :call CompileRunGpp()<CR>  
func! CompileRunGpp()  
exec "w"  
exec "!g++ % -o %<"  
exec "! ./%<"  
endfunc
"编译"
map <F8> :call CompileGpp()<cr>  
func! CompileGpp()  
exec "w"  
exec "!g++ % -o %<"  
endfunc
"运行"
map <F9> :call RunGpp()<cr>    
func! RunGpp()    
exec "w"    
exec "! ./%<"    
endfunc
```

保存后再次进入一个 cpp 文件时，编辑将会更加方便，并可以在 vim 编辑器中直接编译和运行。

#### 操作方法

| 按键 | 作用 |
|:----------:|:----------:|
| F8（音量+键+ 8） |     编译          |
| F9（音量+键 + 9）|     运行          |
| F10（音量+键 + 0）|   编译并运行 |

当然，也可以模仿上面的代码创造自己的快捷键，比如 gdb 调试。

### 优化 termux 终端界面

使用 zsh 来替代 bash 作为默认 shell。

在已安装 curl 的情况下使用以下命令：

```
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
```

Android 6.0 以上可能会弹框确认是否授权访问文件，点击允许，之后会在 termux 根目录生成 storage 目录，可以方便地对外部文件进行操作。

安装成功后先后会有两个选项，分别选择背景色和字体，如下：

Enter a number, leave blank to not to change:
 
Enter a number, leave blank to not to change: 

我选的是 14 和 6，仅供参考。

如果希望再次更改配色，可执行如下命令：

```
~/termux-ohmyzsh/install.sh
```

重启软件或开启新 session 生效。

### 修改启动问候语

可以编辑问候语文件直接修改问候语。

输入如下命令即可修改：

```
vim $PREFIX/etc/motd
```

保存重启即可。

## termux 的其他玩法

- termux 可以在手机上安装正规 linux，比如 ubuntu、kali，并能安装桌面（也就是说可以图形化操作）， 但 NOI linux 除外（大雾） ；
- termux 可以愉快地玩耍 python，制作网络爬虫、微信机器人以及其它；
- 在 termux 上安装 kali-linux 及其它安全工具，靠自己来暴力破解 WIFI， 并从此走上 CTF 的不归路（大雾） ；
- 在 termux 上运用 php 和 WordPress 或 hexo 搭建网站和博客；
- 用 termux ssh 远程连接电脑，从此躺在床上就可以操作电脑；
- 在 termux 上使用 Aria2 打造自己的下载工具，配置好的话还可以高速下载百度云文件；
- 安装 Termux-api，直接操作调动系统底层，用代码发短信、打电话。

-------

termux 作为一个 linux 模拟器，可实现的功能远远不止打打 C++和 python 的代码，希望有更多的人能够认识 termux 的强大并挖掘出更多玩法，也希望能够有更多关于 termux 的优秀文章出现。

## 文章部分内容引用地址

[Termux 高级终端安装使用配置教程 - 国光](https://www.sqlsec.com/2018/05/termux.html)

[vim 美化基本配置](https://www.cnblogs.com/jokerBi/articles/10609264.html)

头图来源：国光博客头图

## 推荐阅读

知乎专栏：[安卓手机配合 linux 新玩法挖掘](https://zhuanlan.zhihu.com/c_1208079877376901120)

...

...

...

...

...

彩蛋：

```
pkg install nyancat
nyancat
```

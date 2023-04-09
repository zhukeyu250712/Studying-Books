# Linux基础

[toc]

## 一.常用命令

``` linux
(1) ctrl c: 取消命令，并且换行
(2) ctrl u: 清空本行命令
(3) tab键：可以补全命令和文件名，如果补全不了快速按两下tab键，可以显示备选选项
(4) ls: 列出当前目录下所有文件，蓝色的是文件夹，白色的是普通文件，绿色的是可执行文件
(5) pwd: 显示当前路径
(6) cd XXX: 进入XXX目录下, cd .. 返回上层目录
(7) cp XXX YYY: 将XXX文件复制成YYY，XXX和YYY可以是一个路径，比如../dir_c/a.txt，表示上层目录下的dir_c文件夹下的文件a.txt
(8) mkdir XXX: 创建目录XXX
(9) rm XXX: 删除普通文件;  rm XXX -r: 删除文件夹
(10) mv XXX YYY: 将XXX文件移动到YYY，和cp命令一样，XXX和YYY可以是一个路径；重命名也是用这个命令
(11) touch XXX: 创建一个文件
(12) cat XXX: 展示文件XXX中的内容
(13) 复制文本
    windows/Linux下：Ctrl + insert+(有的是fn)，Mac下：command + c
(14) 粘贴文本
    windows/Linux下：Shift + insert+(有的是fn)，Mac下：command + v
```

## 二.tmux和vim

### 2.1 tmux

``` 
功能：
    (1) 分屏。
    (2) 允许断开Terminal连接后，继续运行进程。
结构：
    一个tmux可以包含多个session，一个session可以包含多个window，一个window可以包含多个pane。
    实例：
        tmux:
            session 0:
                window 0:
                    pane 0
                    pane 1
                    pane 2
                    ...
                window 1
                window 2
                ...
            session 1
            session 2
            ...
操作：
    (1) tmux：新建一个session，其中包含一个window，window中包含一个pane，pane里打开了一个shell对话框。
    (2) 按下Ctrl + a(acwing)/b(平常的虚拟机)后手指松开，然后按%：将当前pane左右平分成两个pane。
    (3) 按下Ctrl + a/b后手指松开，然后按"（注意是双引号"）：将当前pane上下平分成两个pane。
    (4) Ctrl + d：关闭当前pane；如果当前window的所有pane均已关闭，则自动关闭window；如果当前session的所有window均已关闭，则自动关闭session。
    (5) 鼠标点击可以选pane。
    (6) 按下ctrl + a/b后手指松开，然后按方向键：选择相邻的pane。
    (7) 鼠标拖动pane之间的分割线，可以调整分割线的位置。
    (8) 按住ctrl + a/b的同时按方向键，可以调整pane之间分割线的位置。
    (9) 按下ctrl + a/b后手指松开，然后按z：将当前pane全屏/取消全屏。
    (10) 按下ctrl + a/b后手指松开，然后按d：挂起当前session。
    (11) tmux attach/a：打开之前挂起的session。
    (12) 按下ctrl + a/b后手指松开，然后按s(默认开打的是session级)：选择其它session。
        方向键 —— 上：选择上一项 session/window/pane
        方向键 —— 下：选择下一项 session/window/pane
        方向键 —— 右：展开当前项 session/window
        方向键 —— 左：闭合当前项 session/window
    (13) 按下Ctrl + a/b后手指松开，然后按c(默认打开的是window级别)：在当前session中创建一个新的window。
    (14) 按下Ctrl + a/b后手指松开，然后按w：选择其他window，操作方法与(12)完全相同。
    (15) 按下Ctrl + a/b后手指松开，然后按PageUp/Pagedown：翻阅当前pane内的内容。
    (16) 鼠标滚轮：翻阅当前pane内的内容。
    (17) 在tmux中选中文本时，需要按住shift键。（仅支持Windows和Linux，不支持Mac，不过该操作并不是必须的，因此影响不大）
    (18) tmux中复制/粘贴文本的通用方式：
        (1) 按下Ctrl + a/b后松开手指，然后按[
        (2) 用鼠标选中文本，被选中的文本会被自动复制到tmux的剪贴板
        (3) 按下Ctrl + a/b后松开手指，然后按]，会将剪贴板中的内容粘贴到光标处
```

### 2.2 vim

```
功能：
    (1) 命令行模式下的文本编辑器。
    (2) 根据文件扩展名自动判别编程语言。支持代码缩进、代码高亮等功能。
    (3) 使用方式：vim filename
        如果已有该文件，则打开它。
        如果没有该文件，则打开个一个新的文件，并命名为filename
        
模式：
    (1) 一般命令模式
        默认模式。命令输入方式：类似于打游戏放技能，按不同字符，即可进行不同操作。可以复制、粘贴、删除文本等。
    (2) 编辑模式
        在一般命令模式里按下i，会进入编辑模式。
        按下ESC会退出编辑模式，返回到一般命令模式。
    (3) 命令行模式
        在一般命令模式里按下:/?三个字母中的任意一个，会进入命令行模式。命令行在最下面。
        可以查找、替换、保存、退出、配置编辑器等。
        
操作：
    (1) i：进入编辑模式
    (2) ESC：进入一般命令模式
    (3) h 或 左箭头键：光标向左移动一个字符
    (4) j 或 向下箭头：光标向下移动一个字符
    (5) k 或 向上箭头：光标向上移动一个字符
    (6) l 或 向右箭头：光标向右移动一个字符
    (7) n<Space>：n表示数字，按下数字后再按空格，光标会向右移动这一行的n个字符
    (8) 0 或 功能键[Home]：光标移动到本行开头
    (9) $ 或 功能键[End]：光标移动到本行末尾
    (10) G：光标移动到最后一行
    (11) :n 或 nG：n为数字，光标移动到第n行
    (12) gg：光标移动到第一行，相当于1G
    (13) n<Enter>：n为数字，光标向下移动n行
    (14) /word：向光标之下寻找第一个值为word的字符串。
    (15) ?word：向光标之上寻找第一个值为word的字符串。
    (16) n：重复前一个查找操作
    (17) N：反向重复前一个查找操作
    (18) :n1,n2s/word1/word2/g：n1与n2为数字，在第n1行与n2行之间寻找word1这个字符串，并将该字符串替换为word2
    (19) :1,$s/word1/word2/g：将全文的word1替换为word2
    (20) :1,$s/word1/word2/gc：将全文的word1替换为word2，且在替换前要求用户确认。
    (21) v：选中文本
    (22) d：删除选中的文本
    (23) dd: 删除当前行
    (24) y：复制选中的文本
    (25) yy: 复制当前行
    (26) p: 将复制的数据在光标的下一行/下一个位置粘贴
    (27) u：撤销
    (28) Ctrl + r：取消撤销
    (29) 大于号 >：将选中的文本整体向右缩进一次
    (30) 小于号 <：将选中的文本整体向左缩进一次
    (31) :w 保存
    (32) :w! 强制保存
    (33) :q 退出
    (34) :q! 强制退出
    (35) :wq 保存并退出
    (36) :set paste 设置成粘贴模式，取消代码自动缩进
    (37) :set nopaste 取消粘贴模式，开启代码自动缩进
    (38) :set nu 显示行号
    (39) :set nonu 隐藏行号
    (40) gg=G：将全文代码格式化
    (41) :noh 关闭查找关键词高亮
    (42) Ctrl + q：当vim卡死时，可以取消当前正在执行的命令
    (43)ggdG全删
    挂起：Ctrl + z<====>fg
    
异常处理：
    每次用vim编辑文件时，会自动创建一个.filename.swp的临时文件。
    如果打开某个文件时，该文件的swp文件已存在，则会报错。此时解决办法有两种：
        (1) 找到正在打开该文件的程序，并退出
        (2) 直接删掉该swp文件即可
```

## 三.shell语法

### 1.概论

#### 概论

shell是我们通过命令行与操作系统沟通的语言。

shell脚本可以直接在命令行中执行，也可以将一套逻辑组织成一个文件，方便复用。
AC Terminal中的命令行可以看成是一个“shell脚本在逐行执行”。

Linux中常见的shell脚本有很多种，常见的有：

- Bourne Shell(/usr/bin/sh或/bin/sh)
- Bourne Again Shell(/bin/bash)
- C Shell(/usr/bin/csh)
- K Shell(/usr/bin/ksh)
- zsh
- …

Linux系统中一般默认使用bash，所以接下来讲解bash中的语法。
文件开头需要写#! /bin/bash，指明bash为脚本解释器。

#### 脚本示例
新建一个test.sh文件，内容如下：

```shell
#! /bin/bash
echo "Hello World!"
```

#### 运行方式
作为可执行文件

```shell
acs@9e0ebfcd82d7:~$ chmod +x test.sh  # 使脚本具有可执行权限
acs@9e0ebfcd82d7:~$ ./test.sh  # 当前路径下执行
Hello World!  # 脚本输出
acs@9e0ebfcd82d7:~$ /home/acs/test.sh  # 绝对路径下执行
Hello World!  # 脚本输出
acs@9e0ebfcd82d7:~$ ~/test.sh  # 家目录路径下执行
Hello World!  # 脚本输出
```

#### 用解释器执行

```shell
acs@9e0ebfcd82d7:~$ bash test.sh
Hello World!  # 脚本输出
```

### 2.注释

#### 单行注释
每行中#之后的内容均是注释。

```shell
# 这是一行注释

echo 'Hello World'  #  这也是注释
```

#### 多行注释
格式：

```shell
:<<EOF
第一行注释
第二行注释
第三行注释
EOF
```

其中EOF可以换成其它任意字符串。例如：

```shell
:<<abc
第一行注释
第二行注释
第三行注释
abc

:<<!
第一行注释
第二行注释
第三行注释
!
```

### 3.变量

#### 定义变量
定义变量，不需要加$符号，例如：

```shell
name1='yxc'  # 单引号定义字符串
name2="yxc"  # 双引号定义字符串
name3=yxc    # 也可以不加引号，同样表示字符串
```

#### 使用变量
使用变量，需要加上$符号，或者${}符号。花括号是可选的，主要为了帮助解释器识别变量边界。

```shell
name=yxc
echo $name  # 输出yxc
echo ${name}  # 输出yxc
echo ${name}acwing  # 输出yxcacwing
```

#### 只读变量
使用readonly或者declare可以将变量变为只读。

```shell
name=yxc
readonly name
declare -r name  # 两种写法均可

name=abc  # 会报错，因为此时name只读
```

#### 删除变量
unset可以删除变量。

```shell
name=yxc
unset name
echo $name  # 输出空行
```

#### 变量类型
​	1.自定义变量（局部变量）
​		子进程不能访问的变量
​	2.环境变量（全局变量）
​		子进程可以访问的变量

自定义变量改成环境变量：

```shell
acs@9e0ebfcd82d7:~$ name=yxc  # 定义变量
acs@9e0ebfcd82d7:~$ export name  # 第一种方法
acs@9e0ebfcd82d7:~$ declare -x name  # 第二种方法
```

环境变量改为自定义变量：

```shell
acs@9e0ebfcd82d7:~$ export name=yxc  # 定义环境变量
acs@9e0ebfcd82d7:~$ declare +x name  # 改为自定义变量
```

#### 字符串
字符串可以用单引号，也可以用双引号，也可以不用引号。

单引号与双引号的区别：

- 单引号中的内容会原样输出，不会执行、不会取变量；
- 双引号中的内容可以执行、可以取变量；

```shell
name=yxc  # 不用引号
echo 'hello, $name \"hh\"'  # 单引号字符串，输出 hello, $name \"hh\"
echo "hello, $name \"hh\""  # 双引号字符串，输出 hello, yxc "hh"
```

获取字符串长度

```shell
name="yxc"
echo ${#name}  # 输出3
```

提取子串

```shell
name="hello, yxc"
echo ${name:0:5}  # 提取从0开始的5个字符
```

### 4.默认变量

#### 文件参数变量

在执行shell脚本时，可以向脚本传递参数。\$1是第一个参数，​\$2是第二个参数，以此类推。特殊的，$0是文件名（包含路径）。例如：

创建文件<font color="red">test.sh</font>：

```shell
#! /bin/bash

echo "文件名："$0
echo "第一个参数："$1
echo "第二个参数："$2
echo "第三个参数："$3
echo "第四个参数："$4
```

然后执行该脚本：

```shell
acs@9e0ebfcd82d7:~$ chmod +x test.sh 
acs@9e0ebfcd82d7:~$ ./test.sh 1 2 3 4
文件名：./test.sh
第一个参数：1
第二个参数：2
第三个参数：3
第四个参数：4
```

#### 其它参数相关变量

| 参数       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| $#         | 代表文件传入的参数个数，如上例中值为4                        |
| $*         | 由所有参数构成的用空格隔开的字符串，如上例中值为"\$1 \$2 \$3 \$4" |
| $@         | 每个参数分别用双引号括起来的字符串，如上例中值为"\$1" "​\$2" "​\$3" "​\$4" |
| $$         | 脚本当前运行的进程ID                                         |
| $?         | 上一条命令的退出状态（注意不是stdout，而是exit code）。0表示正常退出，其他值表示错误 |
| $(command) | 返回command这条命令的stdout（可嵌套）                        |
| `command`  | 返回command这条命令的stdout（不可嵌套）                      |



## 四、SSH

### 1、ssh登录

#### (1)基本用法

远程登录服务器：

```shell
ssh user@hostname
```

    user: 用户名
    hostname: IP地址或域名

第一次登录时会提示：

```shell
The authenticity of host '123.57.47.211 (123.57.47.211)' can't be established.
ECDSA key fingerprint is SHA256:iy237yysfCe013/l+kpDGfEG9xxHxm0dnxnAbJTPpG8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入`yes`，然后回车即可。
这样会将该服务器的信息记录在`~/.ssh/known_hosts`文件中。

然后输入密码即可登录到远程服务器中。

默认登录端口号为`22`。如果想登录某一特定端口：

```
ssh user@hostname -p 22
```

#### (2) 配置文件

创建文件` ~/.ssh/config`。

然后在文件中输入：

```
Host myserver1
    HostName IP地址或域名
    User 用户名

Host myserver2
    HostName IP地址或域名
    User 用户名
```

之后再使用服务器时，可以直接使用别名`myserver1`、`myserver2`。

#### (3)密钥登录

创建密钥：

```
ssh-keygen
```

然后一直回车即可。

执行结束后，`~/.ssh/`目录下会多两个文件：

    id_rsa：私钥
    id_rsa.pub：公钥

之后想免密码登录哪个服务器，就将公钥传给哪个服务器即可。

例如，想免密登录`myserver`服务器。则将公钥中的内容，复制到`myserver`中的`~/.ssh/authorized_keys`文件里即可。

也可以使用如下命令一键添加公钥：

```
ssh-copy-id myserver
```

#### (4)执行命令

命令格式：

```
ssh user@hostname command
```

例如：

```
ssh user@hostname ls -a
```

或者

```
# 单引号中的$i可以求值

ssh myserver 'for ((i = 0; i < 10; i ++ )) do echo $i; done'
```

或者

```
# 双引号中的$i不可以求值

ssh myserver "for ((i = 0; i < 10; i ++ )) do echo $i; done"
```



### 2、scp传文件

#### (1)基本用法

命令格式：

```
scp source destination
```

将`source`路径下的文件复制到`destination`中

一次复制多个文件：

```
scp source1 source2 destination
```

复制文件夹：

```
scp -r ~/tmp myserver:/home/acs/
```

将本地家目录中的`tmp`文件夹复制到`myserver`服务器中的`/home/acs/`目录下。

```
scp -r ~/tmp myserver:homework/
```

将本地家目录中的`tmp`文件夹复制到`myserver`服务器中的`~/homework/`目录下。

```
scp -r myserver:homework .
```

将`myserver`服务器中的`~/homework/`文件夹复制到本地的当前路径下。

指定服务器的端口号：

```
scp -P 22 source1 source2 destination
```

注意： `scp`的`-r -P`等参数尽量加在`source`和`destination`之前。
使用`scp`配置其他服务器的`vim`和`tmux`

`scp ~/.vimrc ~/.tmux.conf myserver:`



### 3、配置文件

**.tmux.conf**

```
set-option -g status-keys vi
setw -g mode-keys vi

setw -g monitor-activity on

# setw -g c0-change-trigger 10
# setw -g c0-change-interval 100

# setw -g c0-change-interval 50
# setw -g c0-change-trigger  75


set-window-option -g automatic-rename on
set-option -g set-titles on
set -g history-limit 100000

#set-window-option -g utf8 on

# set command prefix
set-option -g prefix C-a
unbind-key C-b
bind-key C-a send-prefix

bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

bind < resize-pane -L 7
bind > resize-pane -R 7
bind - resize-pane -D 7
bind + resize-pane -U 7


bind-key -n M-l next-window
bind-key -n M-h previous-window



set -g status-interval 1
# status bar
set -g status-bg black
set -g status-fg blue


#set -g status-utf8 on
set -g status-justify centre
set -g status-bg default
set -g status-left " #[fg=green]#S@#H #[default]"
set -g status-left-length 20


# mouse support
# for tmux 2.1
# set -g mouse-utf8 on
set -g mouse on
#
# for previous version
#set -g mode-mouse on
#set -g mouse-resize-pane on
#set -g mouse-select-pane on
#set -g mouse-select-window on


#set -g status-right-length 25
set -g status-right "#[fg=green]%H:%M:%S #[fg=magenta]%a %m-%d #[default]"

# fix for tmux 1.9
bind '"' split-window -vc "#{pane_current_path}"
bind '%' split-window -hc "#{pane_current_path}"
bind 'c' new-window -c "#{pane_current_path}"

# run-shell "powerline-daemon -q"

# vim: ft=conf
```

**.vimrc**

```
" An example for a vimrc file.
"
" To use it, copy it to
"     for Unix and OS/2:  ~/.vimrc
"             for Amiga:  s:.vimrc
"  for MS-DOS and Win32:  $VIM\_vimrc
"           for OpenVMS:  sys$login:.vimrc

" When started as "evim", evim.vim will already have done these settings.
if v:progname =~? "evim"
  finish
endif

" Use Vim settings, rather then Vi settings (much better!).
" This must be first, because it changes other options as a side effect.
set nocompatible

" allow backspacing over everything in insert mode
set backspace=indent,eol,start

if has("vms")
  set nobackup          " do not keep a backup file, use versions instead
else
  set backup            " keep a backup file
endif
set history=50          " keep 50 lines of command line history
set ruler               " show the cursor position all the time
set showcmd             " display incomplete commands
set incsearch           " do incremental searching
"==========================================================================
"My Setting-sunshanlu
"==========================================================================
vmap <leader>y :w! /tmp/vitmp<CR>
nmap <leader>p :r! cat /tmp/vitmp<CR>

"语法高亮
syntax enable
syntax on
"显示行号
set nu

"修改默认注释颜色
"hi Comment ctermfg=DarkCyan
"允许退格键删除
"set backspace=2
"启用鼠标
set mouse=a
set selection=exclusive
set selectmode=mouse,key
"按C语言格式缩进
set cindent
set autoindent
set smartindent
set shiftwidth=4

" 允许在有未保存的修改时切换缓冲区
"set hidden

" 设置无备份文件
set writebackup
set nobackup

"显示括号匹配
set showmatch
"括号匹配显示时间为1(单位是十分之一秒)
set matchtime=5
"显示当前的行号列号：
set ruler
"在状态栏显示正在输入的命令
set showcmd

set foldmethod=syntax
"默认情况下不折叠
set foldlevel=100
" 开启状态栏信息
set laststatus=2
" 命令行的高度，默认为1，这里设为2
set cmdheight=2


" 显示Tab符，使用一高亮竖线代替
set list
"set listchars=tab:\|\ ,
set listchars=tab:>-,trail:-


"侦测文件类型
filetype on
"载入文件类型插件
filetype plugin on
"为特定文件类型载入相关缩进文件
filetype indent on
" 启用自动补全
filetype plugin indent on 


"设置编码自动识别, 中文引号显示
filetype on "打开文件类型检测
"set fileencodings=euc-cn,ucs-bom,utf-8,cp936,gb2312,gb18030,gbk,big5,euc-jp,euc-kr,latin1
set fileencodings=utf-8,gb2312,gbk,gb18030
"这个用能很给劲，不管encoding是什么编码，都能将文本显示汉字
"set termencoding=gb2312
set termencoding=utf-8
"新建文件使用的编码
set fileencoding=utf-8
"set fileencoding=gb2312
"用于显示的编码，仅仅是显示
set encoding=utf-8
"set encoding=utf-8
"set encoding=euc-cn
"set encoding=gbk
"set encoding=gb2312
"set ambiwidth=double
set fileformat=unix


"设置高亮搜索
set hlsearch
"在搜索时，输入的词句的逐字符高亮
set incsearch

" 着色模式
set t_Co=256
"colorscheme wombat256mod
"colorscheme gardener
"colorscheme elflord
colorscheme desert
"colorscheme evening
"colorscheme darkblue
"colorscheme torte
"colorscheme default

" 字体 && 字号
set guifont=Monaco:h10
"set guifont=Consolas:h10

" :LoadTemplate       根据文件后缀自动加载模板
"let g:template_path='/home/ruchee/.vim/template/'

" :AuthorInfoDetect   自动添加作者、时间等信息，本质是NERD_commenter && authorinfo的结合
""let g:vimrc_author='sunshanlu'
""let g:vimrc_email='sunshanlu@baidu.com'
""let g:vimrc_homepage='http://www.sunshanlu.com'
"
"
" Ctrl + E            一步加载语法模板和作者、时间信息
""map <c-e> <ESC>:AuthorInfoDetect<CR><ESC>Gi
""imap <c-e> <ESC>:AuthorInfoDetect<CR><ESC>Gi
""vmap <c-e> <ESC>:AuthorInfoDetect<CR><ESC>Gi



" ======= 引号 && 括号自动匹配 ======= "
"
":inoremap ( ()<ESC>i

":inoremap ) <c-r>=ClosePair(')')<CR>
"
":inoremap { {}<ESC>i
"
":inoremap } <c-r>=ClosePair('}')<CR>
"
":inoremap [ []<ESC>i
"
":inoremap ] <c-r>=ClosePair(']')<CR>
"
":inoremap < <><ESC>i
"
":inoremap > <c-r>=ClosePair('>')<CR>
"
"":inoremap " ""<ESC>i
"
":inoremap ' ''<ESC>i
"
":inoremap ` ``<ESC>i
"
":inoremap * **<ESC>i

" 每行超过80个的字符用下划线标示
""au BufRead,BufNewFile *.s,*.asm,*.h,*.c,*.cpp,*.java,*.cs,*.lisp,*.el,*.erl,*.tex,*.sh,*.lua,*.pl,*.php,*.tpl,*.py,*.rb,*.erb,*.vim,*.js,*.jade,*.coffee,*.css,*.xml,*.html,*.shtml,*.xhtml Underlined /.\%81v/
"
"
" For Win32 GUI: remove 't' flag from 'guioptions': no tearoff menu entries
" let &guioptions = substitute(&guioptions, "t", "", "g")

" Don't use Ex mode, use Q for formatting
map Q gq

" This is an alternative that also works in block mode, but the deleted
" text is lost and it only works for putting the current register.
"vnoremap p "_dp

" Switch syntax highlighting on, when the terminal has colors
" Also switch on highlighting the last used search pattern.
if &t_Co > 2 || has("gui_running")
  syntax on
  set hlsearch
endif

" Only do this part when compiled with support for autocommands.
if has("autocmd")

  " Enable file type detection.
  " Use the default filetype settings, so that mail gets 'tw' set to 72,
  " 'cindent' is on in C files, etc.
  " Also load indent files, to automatically do language-dependent indenting.
  filetype plugin indent on

  " Put these in an autocmd group, so that we can delete them easily.
  augroup vimrcEx
  au!

  " For all text files set 'textwidth' to 80 characters.
  autocmd FileType text setlocal textwidth=80

  " When editing a file, always jump to the last known cursor position.
  " Don't do it when the position is invalid or when inside an event handler
  " (happens when dropping a file on gvim).
  autocmd BufReadPost *
    \ if line("'\"") > 0 && line("'\"") <= line("$") |
    \   exe "normal g`\"" |
    \ endif

  augroup END

else

  set autoindent                " always set autoindenting on

endif " has("autocmd")

" 增加标行高亮
set cursorline
hi CursorLine  cterm=NONE   ctermbg=darkred ctermfg=white

" 设置tab是四个空格
set ts=4
set expandtab

" 主要给Tlist使用
let Tlist_Exit_OnlyWindow = 1
let Tlist_Auto_Open = 1
```

```
scp .tmux.conf .vimrc zky:
```



## 五、git

### 1、代码托管平台

[git.acwing.com](git.acwing.com)

### 2、git基本概念

（1）工作区：仓库的目录。工作区是独立于各个分支的。
（2）暂存区：数据暂时存放的区域，类似于工作区写入版本库前的缓存区。暂存区是独立于各个分支的。
（3）版本库：存放所有已经提交到本地仓库的代码版本
（4）版本结构：树结构，树中每个节点代表一个代码版本。

### 3、git常用命令

```git
1、git config --global user.name xxx：设置全局用户名，信息记录在~/.gitconfig文件中
2、git config --global user.email xxx@xxx.com：设置全局邮箱地址，信息记录在~/.gitconfig文件中

3、git init：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中

4、git add XX：将XX文件添加到暂存区
    git add .：将所有待加入暂存区的文件加入暂存区

5、git rm --cached XX：将文件从仓库索引目录中删掉
  git restore --staged XX:从暂存区删除
  git restore XX:将工作区回到暂存区，该回滚不仅仅是只能在add的时候回滚，删除了之后也能回滚掉
  
6、git commit -m "给自己看的备注信息"：将暂存区的内容提交到当前分支
	将暂存区持久化

7、git status：查看仓库状态

8、git diff XX：查看XX文件相对于暂存区修改了哪些内容

9、git log：查看当前分支的所有版本
   git log --pretty=oneline :将log一行一行显示
   
10、git reflog：查看HEAD指针的移动历史（包括被回滚的版本）

11、git reset --hard HEAD^ 或 git reset --hard HEAD~：将代码库回滚到上一个版本
    git reset --hard HEAD^^：往上回滚两次，以此类推
    git reset --hard HEAD~100：往上回滚100个版本
    git reset --hard 版本号：回滚到某一特定版本
    
    
12、git checkout — XX或git restore XX：将XX文件尚未加入暂存区的修改全部撤销

13、git remote add origin git@git.acwing.com:xxx/XXX.git：将本地仓库关联到远程仓库

14、git push -u (第一次需要-u以后不需要)：将当前分支推送到远程仓库
    git push origin branch_name：将本地的某个分支推送到远程仓库

15、git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下

16、git checkout -b branch_name：创建并切换到branch_name这个分支

17、git branch：查看所有分支和当前所处分支

18、git checkout branch_name：切换到branch_name这个分支

19、git merge branch_name：将分支branch_name合并到当前分支上

20、git branch -d branch_name：删除本地仓库的branch_name分支
21、git branch branch_name：创建新分支

22、git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支

23、git push -d origin branch_name：删除远程仓库的branch_name分支

24、git pull：将远程仓库的当前分支与本地仓库的当前分支合并
    git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并

25、git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应

26、git checkout -t origin/branch_name 将远程的branch_name分支拉取到本地

27、git stash：将工作区和暂存区中尚未提交的修改存入栈中

28、git stash apply：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素

29、git stash drop：删除栈顶存储的修改

30、git stash pop：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素

31、git stash list：查看栈中所有元素
```


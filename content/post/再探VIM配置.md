+++ 
title = "再探VIM配置" 
date = "Fri, 02 Sep 2016 16:13:00 GMT" 
tags = ["工具"] 
categories = ["OS"]
description = "开始制作属于自己的VIM。" 
+++ 


- 最初找到这个发行版[spf13-vim](https://github.com/spf13/spf13-vim)，在ubuntu上用的还比较方便，有很多插件；最近在mac上用，总是不兼容vim，用brew安装了最新的vim，还是跟系统不兼容，总是有问题，于是就找到了下面第二个配置。
- [vim－config](https://github.com/j1z0/vim-config)，这哥们基于vundle配置了一些有用的插件，国内有某君翻译了[他的博文](http://codingpy.com/article/vim-and-python-match-in-heaven/).美中不足的是，安装有问题，有几处安装不成功。使用的时候也是有问题。在翻译的文章最后，编者才告知有问题，但是并没有给出解决的办法。于是我又开始自己造轮子了。
- 拷贝默认的配置文件 ``` $ cp /usr/sh are/vim/vimrc ~/.vimrc ```
- 编辑文件 ```$ vim ~/.vimrc ```
- 安装Vundle: ```$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim ```
- 将插件管理器[vundle](https://github.com/VundleVim/Vundle.vim)的Configure Plugins:拿过来
- 再借用了上面[vim－config](https://github.com/j1z0/vim-config)中的高亮颜色插件，折叠等,并参考了该[博文](http://www.jianshu.com/p/a0b901307b76)


于是得到下面的配置文件

```
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
"Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
"Plugin 'L9'
" Git plugin not hosted on GitHub
"Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
"Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
"Plugin 'rstacruz/sparkup', {
'rtp'
        : 'vim/'
}
" Install L9 and avoid a Naming conflict if you've already installed a
"different version somewhere else.
"Plugin 'ascenator/L9', {'name': 'newL9'}

""code folding
Plugin 'tmhedberg/SimpylFold'

"Colors!!!
Plugin 'altercation/vim-colors-solarized'
Plugin 'jnurmine/Zenburn'


" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line

" ==================

" Configuration file for vim
set modelines=0         " CVE-2007-2438

" Normally we use vim-extensions. If you want true vi-compatibility
" remove change the following statements
"set nocompatible       " Use Vim defaults instead of 100% vi compatibility
set backspace=2         " more powerful backspacing

" Don't write backup file if vim is being called by "crontab -e"
au BufWrite /private/tmp/crontab.* set nowritebackup nobackup
" Don't write backup file if vim is being called by "chpass"
au BufWrite /private/etc/pw.* set nowritebackup nobackup

set number

"filetype on

set history=1000

" 启动的时候黑色背景，关闭，启动的时候彩色，后面有设置F5切换
"set background=dark

"语法高亮
" For full syntax highlighting:
let python_highlight_all=1
syntax on

set autoindent

set smartindent

set tabstop=4

set shiftwidth=4

set showmatch

"当vim进行编辑时，如果命令错误，会发出一个响声，该设置去掉响声
set vb t_vb=

"在编辑过程中，在右下角显示光标位置的状态行
set ruler

set incsearch

"默认情况下，寻找匹配是高亮度显示的，该设置关闭高亮显示

"set nohls

"custom keys
let mapleader=" "
map <leader>g  :YcmCompleter GoToDefinitionElseDeclaration<CR>
""
call togglebg#map("<F5>")

```
最后别忘了
在vim界面输入``` :PluginInstall```安装

![](http://images2015.cnblogs.com/blog/781469/201609/781469-20160903112937168-1853675546.png)

为方便有兴趣的同学使用，我将配置文件放到[github](https://github.com/tmhm/vim-config)上了,只要clone下来，进入该文件目录下，执行```setup_vim.sh```即可。

*Enjoy it～*




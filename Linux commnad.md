<h1 align=center>Linux / VIM C++ command</h1>
<p align=right>update 2019.6.14</p>
<p align=center>該文件包含了linux基礎指令，vim下基礎指令</p>

<h3>目錄</h2>


1. [移动到指定目录](#)
1. [查看文件下的资料](#)
1. [創建文件夾](#)
1. [remove 所有资料](#)
1. [解压缩](#)
1. [VIM C++](#)
	1. [編寫文件常用指令](#)
	1. [編譯執行](#)
	1. [vim下一鍵生成編譯](#)
1. [VIM 插件](#)
	1. ale 語法檢查
	1. Vundle 插件管理器
	1. youcompleteme 代碼補全

---

<h3 id=>移动到指定目录</h4>

1. cd ./path

1. 返回上一層 ：cd ../ 

<h3 id=>查看文件下的资料</h4>

* ls -a 列出文件夹下所有文件

<h3 id=>創建文件夾</h4>

```mkdir test1``` :創建一個空目錄

```mkdir -p test1/test2```:递归创建多个目录

<h3 id=>Remove 所有资料</h4>


此方法是强制删除所有目录下文件:rm -rf /path

ps.使用时请千万谨慎

<h3 id=>解压缩</h4>

* ```tar xvzf file.tgz -C /dir you want```

* ```tar xvzf file.tar.gz -C /dir you want```

* ```uzip file.zip -d /dir you want```

---

<h3 id=>VIM C++</h4>

<h4>編寫文件常用指令</h4>

* ```i``` ：insert 用來鍵入

* ```x``` : 游標的地方delete

* ```u``` : 返回上一步

* ```：q!``` : 不保存強制退出

* ```：wq！```：保存后退出


<h4>編譯/執行</h4>

1. 使用g++ 編譯cpp文件：
* ```g++ filename.cpp``` : 自動生成a.out文件
* ```g++ filename.cpp``` -c 生成文件名 : 自動生成指定的文件名

2. ./檔名 ：執行文件

<h4>vim下一鍵生成編譯</h4>

將以下代碼添加到 ~/.vimrc中， vimrc 打開方式 ``` vi ~/.vimrc```
```

map <F5> :call CompileRunGcc()<CR>
func! CompileRunGcc()
	exec "w"
	if &filetype == 'c'
		exec "!g++ % -o %<"
		exec "!time ./%<"
	elseif &filetype == 'cpp'
		exec "!g++ % -o %<"
		exec "!time ./%<"
	elseif &filetype == 'java' 
		exec "!javac %" 
		exec "!time java %<"
	elseif &filetype == 'sh'
		:!time bash %
	elseif &filetype == 'python'
		exec "!time python2.7 %"
    elseif &filetype == 'html'
        exec "!firefox % &"
    elseif &filetype == 'go'
        exec "!go build %<"
        exec "!time go run %"
    elseif &filetype == 'mkd'
        exec "!~/.vim/markdown.pl % > %.html &"
        exec "!firefox %.html &"
	endif
endfunc

```

添加后保存，Fn+F5可一鍵編譯運行

---

<h3> vim 插件 </h4>

## ale 語法檢查

git hub地址
> https://github.com/w0rp/ale

install

```
mkdir -p ~/.vim/pack/git-plugins/start
git clone --depth 1 https://github.com/w0rp/ale.git ~/.vim/pack/git-plugins/start/ale
```
---
### vundle 插件管理器

1. install

```
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
2. 配置.vimrc

```vi .vimrc```

进入后复制贴上

```
set nocompatible               "去除VIM一致性，必须"
filetype off                   "必须"

"设置包括vundle和初始化相关的运行时路径"
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

"启用vundle管理插件，必须"
Plugin 'VundleVim/Vundle.vim'

"在此增加其他插件，安装的插件需要放在vundle#begin和vundle#end之间"
"安装github上的插件格式为 Plugin '用户名/插件仓库名'"

call vundle#end()              
filetype plugin indent on      "加载vim自带和插件相应的语法和文件类型相关脚本，必须"
```

---


### youcompleteme 代碼補全

直接利用Vundle插件管理器安装

1. vi .vimrc 进入vimrc配置

2. 在call vundle#begin()以及call vundle#end()  之间 加入

```Plugin 'Valloric/YouCompleteMe'```


3. :wq 保存跳出

4. 进入vim 使用 ：进行尾行命令 输入 PluginInstall 完成安装


5. 执行  ```git submodule update --init --recursive```

会开始下载细部文件到插件的各个文件夹中

6. 执行编译(完成下面两步骤）

- ```cd ~/.vim/bundle/YouCompleteMe``` :到安装的目录下

- ```./install.py --clang-completer``` :执行isntall.py来进行编译安装

这个时候安装还没有完成， 打开cpp会出现
“No .ycm_extra_conf.py file detected, so no compile flags are available. Thus no semantic support for C/C++/ObjC/ObjC++”

我们要进行最后一步

7. 进入vimrc 下方添加

```
let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'
```

注意网上还有很多估计是旧版本的插件， 所以.ycm_extra_conf.py的档案位置不同, 新版本的路径请依照上面

8. 打开cpp档案， 补全功能正常运行

---

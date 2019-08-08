<h1 align=center>Linux / VIM C++ /Mini-conda command</h1>
<p align=right>update 2019.8.8</p>
<p align=center>該文件包含了linux基礎指令，vim下基礎指令</p>
<h3>目錄</h2>

1. [移动到指定目录 cd](#)
2. [查看文件下的资料 ls](#)
3. [创建文件 touch](#)
4. [創建文件夾 mkdir](#)
5. [搜寻文件or文件夹](#)
6. [查看当前目录下的文件数](#)
7. [remove 所有资料 rm](#)
8. [移動文件](#)
9. [解压缩](#)
10. [建立资料连接 In](#)
11. [查看CPU，内存占用率](#)
12. [杀死用户进程 kill](#)
13. [确认本机IP位置及端口l](#)
14. [实时查看NVIDIA GPU使用率](#)
15. [g++相关](#)
16. [VIM C++](#)
    1. [編寫文件常用指令](#)
    2. [g++編譯執行](#)
    3. [连接头文件及源文件的编译方式](#)
    4. [编辑中使用鼠标](#)
    5. [设定行数显示](#)
    6. [vim下一鍵生成編譯](#)
    7. [vim编辑时显示当前的文件名](#)
17. [VIM 插件](#)
    1. ale 語法檢查
    2. Vundle 插件管理器
    3. youcompleteme 代碼補全
18. [Mini-Conda使用](#)

------

<h3 id=>移动到指定目录</h4>

1. `cd ./path`

2. 返回上一層 ：`cd ../ `

3. 如果要指定的目录在上面多层 可以用 ```../../```, 放回基层就放多少个```../```

   Example: 

   ```
   ../../input/dataset
   ```

4. 回到主目录 ```cd ~```

<h3 id=>查看文件下的资料</h4>

- ls -a 列出文件夹下所有文件

------

<h3 id=>創建文件夾</h4>
```mkdir test1``` :創建一個空目錄

```mkdir -p test1/test2```:递归创建多个目录

------

<h3 id=> 创建文件 </h3>

先cd到你要的创建文件的路径，接着

```touch 文件名.扩展名```

------

<h3 id=>搜寻文件or文件夹
</h3>

```find / -iname "文件名"```：

```find / -type d -iname "文件夹名```

------

<h3 id=>查看当前目录下的文件数</h4>

- 统计当前目录下文件的个数（不包括目录）

```
$ ls -l | grep "^-" | wc -l
```

- 统计当前目录下文件的个数（包括子目录）

```
$ ls -lR| grep "^-" | wc -l
```

- 查看某目录下文件夹(目录)的个数（包括子目录）

```
$ ls -lR | grep "^d" | wc -l
```

**命令解析：**

- `ls -l`

长列表输出该目录下文件信息(注意这里的文件是指目录、链接、设备文件等)，每一行对应一个文件或目录，`ls -lR`是列出所有文件，包括子目录。

- `grep "^-"`
  过滤`ls`的输出信息，只保留一般文件，只保留目录是`grep "^d"`。
- `wc -l`
  统计输出信息的行数，统计结果就是输出信息的行数，一行信息对应一个文件，所以就是文件的个数。

------

<h3 id=>Remove 所有资料</h4>



此方法是强制删除所有目录下文件:rm -rf /path

ps.使用时请千万谨慎

<h3 id=>解压缩</h4>



- ```tar xvf file.tar -C /dir you want```
- ```tar xvzf file.tgz -C /dir you want```
- ```tar xvzf file.tar.gz -C /dir you want```
- ```unzip file.zip -d /dir you want```

------

<h3 id=>移動文件 </h3>

mv 文件名 方式 目的地



------

<h3 id=>建立资料连接 In </h3>

ln 是在 Linux 及其他 Unix Like 作業系統建立連結的指令，概念跟 Windows 的捷徑差不多

**ln 的語法**

ln [OPTION]… TARGET […] [LINKNAME […]]



**ln 常用參數**
-s, –symbolic: 建立 symbolic link。
-f, –force: 如果目標檔案已經存在，不會提出詢問，而會直接強制覆蓋檔案。
-i, –interactive: 如果目標檔案已經存在，會先提出詢問，不會直接強制覆蓋檔案。
-n, –no-clobber： 不會覆蓋任何檔案。



Example:

在目前工作目錄建立一個 symbolic link，連結到 /var/www/html/index.htm

```$ ln -s /var/www/html/index.htm```

------

<h3 id=>查看CPU、内存占用率 </h3>

键入```top``` : 

- PID：进程的ID
- USER：进程所有者
- PR：进程的优先级别，越小越优先被执行
- NInice：值
- VIRT：进程占用的虚拟内存
- RES：进程占用的物理内存
- SHR：进程使用的共享内存
- S：进程的状态。S表示休眠，R表示正在运行，Z表示僵死状态，N表示该进程优先值为负数
- %CPU：进程占用CPU的使用率
- %MEM：进程使用的物理内存和总内存的百分比
- TIME+：该进程启动后占用的总的CPU时间，即占用CPU使用时间的累加值。

------

<h3 id=>杀死进程kill PID </h3>

```ps aux``` : 查看所有进程

```kill pid``` : 找到对应的pid号， 例如2316是你要关闭的程序pid， 则```kill 2316```

删除指定程序所有进程：

```ps -ef | grep 程序名称| awk '{print $2}' | xargs kill -9```

例如要kill所有vim进程则

```ps -ef | grep vim| awk '{print $2}' | xargs kill -9```

------

<h3 id=>确认本机IP及端口 </h3>

```ifconfig -a``` : 确认IP位置

```netstat -anptl``` : 确认端口

------

<h3 id=>实时查看NVIDIA GPU使用率</h4>

Nvidia自带了一个nvidia-smi的命令行工具，会显示显存使用情况

```nvidia-smi```

如果想不间断持续监控可以使用watch 指令

基础用法：

```watch [options]  command```

最常用的参数是 -n， 后面指定是每多少秒来执行一次命令。

监视显存：我们设置为每 3s 显示一次显存的情况：

```watch -n 3 nvidia-smi```

------

<h3 id=>VIM C++</h4>

<h4>編寫文件常用指令</h4>

- ```i``` ：insert 用來鍵入
- `d` : delete 删除所选的区域
- ```x``` : 游標的地方delete
- ```u``` : 返回上一步
- ```：q!``` : 不保存強制退出
- ```:w ``` : 保存不退出
- ```：wq！```：保存后退出

<h4>g++編譯/執行</h4>

1. 使用g++ 編譯cpp文件：

- ```g++ filename.cpp``` : 自動生成a.out文件
- ```g++ filename.cpp``` -c 生成文件名 : 自動生成指定的文件名

2. ```./檔名``` ：執行文件





#### 连接头文件与源文件的编译方式

假设有三个文件包含

**function.cpp** ：源文件

**function.h** ： 头文件

**main.cpp** ： main 执行文件

```g++ -o out Function.cpp Function.h mian.cpp``` ： 将三个文件一起编译

接着会生成out(这个out是可以指定名称的)

然后执行 ```./out``` 就能运行文件

------

<h4>编辑中使用鼠标</h4>

: set mouse=a 可以开启功能 （鼠标mode下无法使用复制贴上)

: set mouse= 关闭功能

------

<h4>设定行数显示</h4>

: set numbers 可以开启功能

**或是直接在vimrc中添加 :set mouse=a**, 永久使用



------

<h4 align = center id = >g++ 相关</h4>

查看g++ 版本 ：`g++ -v`

版本安装， 例如4.8版  `sudo apt-get install gcc-4.8`

查看安装是否成功 `ls /usr/bin/gcc*`

设置优先级 `sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 100`

查看设置结果 `sudo update-alternatives --config gcc`



------



<h4 align = center id = >vim下一鍵生成編譯</h4>

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

------

<h4 align = center id=>vim编辑时显示当前的文件名</h4>

命令vi .vimrc叫出vimrc

添加
```set laststatus=2``` 在最底部

------

<h3 align = center id=> vim 插件 </h3>

### ale 語法檢查

git hub地址

> https://github.com/w0rp/ale

install

```
mkdir -p ~/.vim/pack/git-plugins/start
git clone --depth 1 https://github.com/w0rp/ale.git ~/.vim/pack/git-plugins/start/ale
```

------

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

------

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
- ```./install.py --clang-completer``` :执行install.py来进行编译安装

这个时候安装还没有完成， 打开cpp会出现
“No .ycm_extra_conf.py file detected, so no compile flags are available. Thus no semantic support for C/C++/ObjC/ObjC++”

我们要进行最后一步

7. 进入vimrc 下方添加

```
let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'

```

注意网上还有很多估计是旧版本的插件， 所以.ycm_extra_conf.py的档案位置不同, 新版本的路径请依照上面

8. 打开cpp档案， 补全功能正常运行

------



<h3 align = center id=> Mini-Conda 使用 </h3>

创建虚拟环境 virtualEnv : `conda create -n 环境名 python=3.6 -y`

启动环境 ：`source activate 环境名`

安装package ：`conda install 包名`



将jupyter notebook添加 conda的虚拟环境内核， 也就是jupyter可以使用conda的环境

1. 启动虚拟环境 `source activate 环境名`

2. 安装`conda install ipykernel`

3. 将环境添加至juypter notebook中

`python -m ipykernel install --user --name open-mmlab --display-name "python (open-mmlab)"`

4.启动juypter notebook > kernel > change kernel 就能看见新增的内核了

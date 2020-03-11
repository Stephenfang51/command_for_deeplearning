<h1 align=center>Linux, VIM C++, Anaconda3, git, Docker, Shell</h1>
<p align=right>update 2020.3.11</p>
<h2 align = 'center'>目錄</h2>

> ### Linux

1. [移动到指定目录 cd](#1)
2. [查看文件下的资料 ls](#2)
3. [創建文件夾 mkdir or rmdir](#3)
4. [创建文件夹 touch](#4)
5. [搜寻文件or文件夹](#5)
6. [查看当前目录下的文件数](#6)
7. [remove 所有资料 rm](#7)
8. [解压缩及打包](#8)
9. [移動文件及复制文件移动, 修改文件名mv](#9)
10. [建立资料连接, 软连接  In](#10)
11. [chmod 添加文件或目录权限](#11)
12. [查看CPU，内存占用率, 磁盘占用](#12)
    - swapfile 增加虚拟内存
13. [杀死用户进程 kill PID](#13)
14. [确认本机IP位置及端口](#14)
15. [其他](#15)
    1. Nm 目标文件格式分析

------

> ### Vim

16. [VIM 编辑器常用操作](#16)
    1. [編寫文件常用指令](#16-1)
    2. [vim功能添加](#16-5)
    3. [vim下一鍵生成編譯](#16-6)
    4. [vim编辑时显示当前的文件名](#16-7)
17. [VIM 插件](#17)
    1. ale 語法檢查
    2. Vundle 插件管理器
    3. youcompleteme 代碼補全
18. [g++相关](#18)

------

> ### Anaconda3

19. [anaconda3 / mini-conda 使用](#19)

------

> ### Git

20. [git 指令集](#20)

------

21. [cuda使用](#21)
22. [pycharm SSH访问远程服务器](#22)
23. [python pip 包安装及管理](#23)
24. Jupyter-notebook SSH远程连接 （待完成）
25. Docker指令 ( 镜像, 容器操作 )
26. Shell 脚本(Shell script) 语法与执行 
27. python 安装 基于Linux
28. onnx 模型相关
29. PytorchToCaffe



------

> ### Others

1. 查看版本
2. 推荐好用
3. Nvidia 查看GPU使用率
4. ffmpeg 使用

------

<h2 align=center>目录结束<h2>

<h3 id="1">1. 移动到指定目录</h3>

1. 到指定目录 ：`cd ./path` 

2. 返回上级目录 ：`cd .. `

3. 如果要指定的目录在上面多层 可以用 ```../../```, 放回基层就放多少个```../```

   Example: 

   ```
   ../../input/dataset
   ```

4. 回到主目录 ```cd ~```

5. `pwd`获取当前绝对路径

<h3 id="2">2. 查看文件下的资料</h4>

- `ls -a` 列出文件夹下所有文件

- `ls -lah` 列出文件下的文件 以及 效果信息例如文件大小

  

------

<h3 id="3">3. 創建文件夾及删除文件夹</h4>

`mkdir test1` :創建一個空目錄

`mkdir -p test1/test2`:递归创建多个目录

`rmdir` 也就是remove empty directories 只能删除空目录 较少用

------

<h3 id="4">4.  创建文件 </h3>

先cd到你要的创建文件的路径，接着

```touch 文件名.扩展名```

------

<h3 id="5">5. 搜寻文件or文件夹
</h3>

参考 https://blog.miniasp.com/post/2010/08/27/Linux-find-command-tips-and-notice

注意以下都是用 `/` 表示搜索所有路径， 也可以依照需求改变路径

依照文件名找

```find / -iname "文件名"```：

依照目录找

```find /  -iname "文件夹名" -type d```

依照档案名找

`find / -iname "档案" -type f`

`find / -iname '*.log' -type f ` : 找副档名为log, 可以依照需求自己改





------

<h3 id="6">6. 查看当前目录下的文件数</h4>

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

<h3 id="7">7. Remove 所有资料</h4>

此方法是强制删除所有目录下文件:rm -rf /path

ps.使用时请千万谨慎

<h3 id="8">8. 解压缩及打包</h4>



- ```tar xvf file.tar -C /dir you want```
- ```tar xvzf file.tgz -C /dir you want```
- ```tar xvzf file.tar.gz -C /dir you want```
- ```unzip file.zip -d /dir you want```

#### unrar 解壓

```
unrar e test.rar 解压文件到当前目录

unrar x test.rar /path/to/extract

unrar l test.rar 查看rar中的文件

unrar v test.rar 更详细

unrar t test.rar 测试是否可以成功解压
```



#### zip 打包

如果是直接压缩几个文件，那就可以直接使用命令 ,

```
zip newfilename.zip filename1 filename2
```

但是一般情况下都会压缩一个文件夹或者叫目录，使用命令

```
zip -r newfilename.zip file1 file2。
```

如果文件夹下面包含太多的文件，可以使用-q选项，不现实压缩的过程，即zip -q -r newfilename.zip file1 file2。
一般来讲，-q选项和-r选项已经够用了

EX.
将/root/test文件夹下的test1文件夹和test2.txt压缩到 aaa.zip
在/root/test目录下，执行命令

```
zip -q -r aaa.zip test1 test2.txt
```

OK！压缩完成！

------

<h3 id="9">9. 移動文件及复制文件移动， 修改文件名 </h3>

`mv 文件名 方式 目的地`



#### 复制文件下的内容移动

```CP [选项] 源文件或目录 目的文件或目录```

例如要备份一个source.list文件

`cp /etc/apt/sources.list /etc/apt/sources.list.backup`

- -b 同名,备分原来的文件
- -f 强制覆盖同名文件
- -r 按递归方式保留原目录结构复制文件

#### 复制文件夹下内容

*号表示所有文件的意思， 将dir1文件夹中所有的东西全部复制都dstpath/dir2文件夹下面

```cp sourcepath/dir1/*  dstpath/dir2```

#### 复制文件夹及里面的内容

将dir1文件夹 完整复制到path2底下

```cp -a path1/dir1 path2/```

<h4>修改文件名 mv </h4>

将filename1 改成 filename2

`mv filename1 filename2`

------

<h3 id="10">10. 建立资料连接， 软连接 In </h3>

Linux ln命令是一个非常重要命令，它的功能是为某一个文件在另外一个位置建立一个同步的链接。

当我们需要在不同的目录，用到相同的文件时，**我们不需要在每一个需要的目录下都放一个必须相同的文件**，我们只要在某个固定的目录，放上该文件，然后在 其它的目录下用ln命令链接（link）它就可以，**不必重复的占用磁盘空间。**

**ln 的語法**

```
ln [选项] [源文件] [目标文件或目录]
```



**ln 必要参数**：

- -b 删除，覆盖以前建立的链接
- -d 允许超级用户制作目录的硬链接
- -f 强制执行
- -i 交互模式，文件存在则提示用户是否覆盖
- -n 把符号链接视为一般目录
- -s 软链接(符号链接)
- -v 显示详细的处理过程



**软连接 / 硬连接**

链接又可分为两种 : 硬链接(hard link)与软链接(symbolic link)，硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置

Example:

之前刚好遇到一个问题用上了软连接， Cmake编译的过程中， lib里面找不到libcudart.so这个动态库， 于是建立一个软连接将cuda文件夹中的libcudart.so 连接到 /usr/lib/下

```shell
ln -s /usr/local/cuda/lib64/libcudart.so /usr/lib/libcudart.so
```



------

<h3 id="11">11. chmod 添加文件或目录权限 </h3>

> 参考http://linux.vbird.org/linux_basic/0210filepermission.php

Linux 的文件可以分别为owner、groups、others三种身份并且有各自的read/wrtie/execute 权限， 一共3x3=9种

利用`ls -al`可以查看目录下文件的详细权限， 可看到类似-rwxrwxrwx 的字样， 这九个权限为三个三个为一组

可用分数表示各权限

r : 2

w : 4

x : 1

所以权限数字为

owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0

如果要将一个文件的权限设定为所有对象都可以开启， 那么也就是-rwxrwxrwx, 也就是777

`chmod 777 ./test.sh`

如果希望该档案不让人修改， 可以`chmod 755 ./test.sh`



##### 用符号类型改变权限

`chmod u +x  filename`   +/-/=

```
u ： user 
g ：group 
o ：others
a ： all(所有身份)
```

```
r : read
w : write
x : execute
```



------

<h3 id="12">12. 查看CPU、内存占用率, 磁盘占用 </h3>

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



`df -h` ： 查看磁盘占用情况

#### swapfile 設置 增加虛擬內存

切割原磁盤空間， 例如需要4GB空間作為內存

1. `sudo fallocate -l 4G /swapfile` :  l用來指定分配大小， /swapfile作為swap使用的檔案存放路徑
2. `. sudo chmod 600 /swapfile` ：为设置好的档案添加root权限
3. `sudo mkswap /swapfile` :  告知系统将此档案作为swap使用
4. `sudo swapon /swapfile` ：启用swap

将swap设定为开机时自动挂载（ubuntu 18之下）

```
sudo vim /etc/fstab
添加以下到最下排
/swapfile   none swap    sw 0 0
```



`swapon -s` : 查看当前的swap状态

`swaoff /swapfile` ：禁用swapfile

`swaoff -a` ：禁用所有的swapfile

`swapon -a`:  启用所有swapfile

可以通过先禁用，再启动的方式，来擦除swap中已有的数据



------

<h3 id="13">13. 杀死进程kill PID </h3>

```ps aux``` : 查看所有进程

`top` : 查看所有进程

```kill pid``` : 找到对应的pid号， 例如2316是你要关闭的程序pid， 则```kill 2316```

`kill -9 pid` : 强制终止进程

删除指定程序所有进程：

```ps -ef | grep 程序名称| awk '{print $2}' | xargs kill -9```

例如要kill所有vim进程则

```ps -ef | grep vim| awk '{print $2}' | xargs kill -9```

------

<h3 id="14">14. 确认本机IP及端口 </h3>

```ifconfig -a``` : 确认IP位置，如果是连接wifi 请看wlan0底下inet的位置

```netstat -anptl``` : 确认端口

------

<h3 id="15">15. 其他</h4>

#### 1. nm 目标文件格式分析

查看动态库 (so) ：`nm -D /path/to/xxxx.so`

這樣會打印出全部內容， 可以用grep篩選 如

`nm -D /path/to/xxxx.so | grep xxxx`



------

<h3 id="16">16. VIM 编辑器常用操作</h4>

<h4 #="16-1">編寫文件常用指令</h4>

- `w` 向下行移動

- `b` 向上行移動

- ```i``` ：insert 用來鍵入

- `d` : delete 删除所选的区域

- ```x``` : 游標的地方delete

- ```u``` : 返回上一步

- `ctrl + r`: 撤銷上一步

- ```：q!``` : 不保存強制退出

- ```:w ``` : 写入保存不退出

- ```：wq！```：写入保存后退出

- `y`：鼠标框起复制

- `p`:贴上

- 多行注釋

  在要注釋掉的行首 加入`#if 0`

  在要注釋掉的行尾 加入`#endif`

------

<h4 #="16-5">VIM 功能添加</h4>

vim提供许多的功能可以依照需求在编辑文件中时添加， 也能直接在~/.vimrc文件中添加成永久功能

#### 透过vimrc添加为永久功能

1. `vim ~/.vimrc` 开启or新建文件 (mac位置通常放在/User/yourname/.vimrc, linux 在etc/vimrc/.vimrc)
2. 将需要的功能添加进文件并且保存退出

#### 功能列表

`: syntax on` 开启语法高亮

`: set mouse=a` 可以开启功能 （鼠标mode下无法使用复制贴上)

`: set mouse =` 关闭功能, 或是直接在vimrc中添加 : **set mouse=a**, 永久使用

`: set number` 可以开启功能

------

<h4 id ="16-4" >vim下一鍵生成編譯</h4>

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

<h4 #="16-5">vim编辑时显示当前的文件名</h4>

命令vi .vimrc叫出vimrc

添加
```set laststatus=2``` 在最底部

------

<h3 id="17">17. vim 插件 </h3>

#### ale 語法檢查

git hub地址

> https://github.com/w0rp/ale

install

```
mkdir -p ~/.vim/pack/git-plugins/start
git clone --depth 1 https://github.com/w0rp/ale.git ~/.vim/pack/git-plugins/start/ale
```

#### vundle 插件管理器

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

#### youcompleteme 代碼補全

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

<h3 id = "18">18.  g++ 相关</h3>
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

<h4 id = >g++ 其它</h4>

查看g++ 版本 ：`g++ -v`

版本安装， 例如4.8版  `sudo apt-get install gcc-4.8`

查看安装是否成功 `ls /usr/bin/gcc*`

设置优先级 `sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 100`

查看设置结果 `sudo update-alternatives --config gcc`

------

<h3 id="19">19. anaconda3/ Mini-Conda 使用 </h3>

#### 虚拟环境

查看当前所有环境 ： `conda info --env` or `conda env list`

创建虚拟环境 virtualEnv : `conda create -n 环境名 python=3.6 -y`

启动环境 ：`source activate 环境名`

关闭环境 ：`source deactivate`

删除环境 :`conda env remove -n 环境名`

安装package ：`conda install 包名` 用conda设置的源安装

安装package 方法二 : `pip install` 



#### 虚拟环境路径

`../anaconda3/envs/`

#### 虚拟环境包的路径

conda环境下， 查看安装的package `pip list`

`/usr/local/anaconda3(或者是miniconda3)/envs/环境名（自己创建的）/lib/python3.x/site-packages/`



#### jupyter notebook 添加conda环境到内核

将jupyter notebook添加 conda的虚拟环境内核， 也就是jupyter可以使用conda的环境

1. 启动虚拟环境 `source activate 环境名`
2. 安装`conda install ipykernel`
3. 将环境添加至juypter notebook中

`python -m ipykernel install --user --name open-mmlab --display-name "python (open-mmlab)"`

4.启动juypter notebook > kernel > change kernel 就能看见新增的内核了



#### conda 安装pytorch

```
# CUDA 9.0
conda install pytorch==1.0.1 torchvision==0.2.2 cudatoolkit=9.0 -c pytorch

# CUDA 10.0
conda install pytorch==1.0.1 torchvision==0.2.2 cudatoolkit=10.0 -c pytorch

# CPU Only
conda install pytorch-cpu==1.0.1 torchvision-cpu==0.2.2 cpuonly -c pytorch
```

参考Pytorch官方 [https://pytorch.org/get-started/previous-versions/](https://pytorch.org/get-started/previous-versions/)



#### conda 换源

設置源的文件在  `~/.condarc`

所以用vim 進入編輯

```shell
#1  vim 进入
vim ~/.condarc
#2. 貼入清華源

channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
ssl_verify: true


#3. 如清華源失效， 可以試試中科大源
channels:
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
ssl_verify: true


#4. 上海交大源
channels:
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/main/
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/free/
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud/conda-forge/
ssl_verify: true


#以上三个源， 泽一即可
```

经过实际测验， 以下速度飞快， 推荐使用

```
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/main/
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/free/
  - defaults
show_channel_urls: true
```







------

<h3 id="20">20. git 操作指令</h3>

1. 配置指令如以下， Your Name 键入github名称， email键入注册github的邮箱

```
git config --global user.name Your Name
git config --global user.email email@example.com
```

git config命令的–global参数，用了这个参数，表示你这台机器上所有的 Git 仓库都会使用这个配置



2. 生成金钥SSH Key, 并且连敲三次回车

```
ssh-keygen -t rsa -C email@example.com
```



3. 到github上， 打开Account settings > SSH and GPG keys>点击Add SSH keys ， 将id_rsa.pub答案内容复制并贴在空格内， title可以自取



#### 建立本地仓库

创建一个存放程序的资料夹， 里面可以放各仓库， 例如以下

```
cd 指定路径
mkdir myprogram
cd myprogram
mkdir example #这就是一个repo存在myprogram中
```

接着在example文件夹下执行git init 初始化本地仓库 就可成功创建repo仓库

```
git init
```

然后将要存放的文件放进example资料夹中, 例如放了一个test.txt文件进去

接着我们需要对文件进行追踪， 键入， 这样如果之后档案修改过就可以追踪到

```git ad
git add test.txt
```

如果上传文件较多， 可以直接追踪全部

```
git add --all
```

确认没有问题， 就可以commit到repo中， 例如”update the file“

```
git commit -m "updata the file"
```



#### 在github上创建仓库repo

建立好远程仓库之后， 就可以进行远程 - 本地连接

```git remote add origin 网址.git```



接着就可以进行同步 push到remote repo, 注意下面的xxxx表示 branch名称 

通常push需要输入username 和 password，就输入github的即可

```
git push origin master #依照branch可替换名称
```

检查repo状态, 可以查看当前的状态

```
git status
```



#### 克隆 clone

克隆可以将网上的开源库给download到自己的本地仓中例如向下面这样， 后面加上网址

```
git clone https://github.com/Stephenfang51/Grad_CAM_Pytorch-1.01
```



克隆项目中特定的版本

```
git clone -b <version_name> --single-branch
```



更新子模块submodule

```
git clone --recursive-submodule http://xxxxxxxxxxxxxx.git
```





#### 删除文件

cd到repo的文件夹后， 一般的删除本地以及远程文件的方式

```
git rm 文件名
```



如果只想删除远程而保留本地的方式

```
git rm -r --cached 文件名
```



记得最终如果要同步到remote repo都要git push一下

#### 分支（branch）

部分参考http://gogojimmy.net/2012/01/21/how-to-use-git-2-basic-usage-and-worflow/

- 查看当前分支 `git branch` , 如果包含远程 加上`-a`

- 开设分支 `Git branch <new branch-name>`

- 切换分支 `git checkout branch-name`

- 删除分支 `git branch -d branch-name`

- 合并分支 

  - 例如开发人员在A branch开发成功， 希望合并目前的master， 可以在A branch下输入

    `git rebase master` 该方法会基于master branch最新的commit内容在将自己A分支下新添加的内容加进去, **PS. commit 量较多建议使用rebase·**

  - `git merge` 不同于rebase, 是从master额外来条线

- 删除远程分支

  - 先查看远程分支 `git branch -r` 找到要刪除的

  - `git branch -r -d origin/branch-name` ， -r表示remote的意思, 該句主要将远程和本地的跟踪删除

  - `git push origin :branch-name` 正式删除远程分支

    

#### 取消操作

- 回到上一次commit时的状态 `git reset --hard HEAD `, 参数HEAD^表示目前版本的上个版本， HEAD~2则是在上一个



#### 開啟Git GUI

`gitk --all` : 可以開啟git的gui版本

#### 更新 fetch / pull

本机如果有推送东西到server， 除了本地的分支， 远程分支也会记录一份在本机上， 一样也是有HEAD / master分支， 但会在前面加行远程节点origin， 变成`origin/HEAD` 以及 `origin/master`

如果线上有更新的部分， 但是本机并没有， 则执行fetch时候， 会自动从远程抓一份下来， 并且更新origin的部分

```
git fetch
```

既然`origin/master`是从`master`分支出去且更新， 如果想要master更新`origin/master`， 就需要merge

```
git merge origin/master
```

执行之后就会将master 来回到跟`origin/master`同个位置



而 git pull = git fetch + git merge

```
git pull --rebase
```





#### 问题解决

1. error: failed to push some refs to

问题在于远程版本比本地的还要新， 需要先pull回来做更新才能重新push

解决方式：

先进行更新, 然後合併

1. `git fetch `
2. `git merge `

然后在push即可



2. Changes not staged for commit:

   因為要提交的提交的档案尚未track， 需要对该档案 git add 档案名， 然后在重新执行commit, push等

   

3. Git修改密码后命令行push代码报“fatal: Authentication failed for 

   ```shell
   git config --system --unset credential.helper
   git config --global credential.helper store
   ```

   然后在git push 就会要求输入使用者的账号和密码

------

<h3 id="21">21. cuda 使用 </h3>

查看cuda版本

`cat /usr/local/cuda/version.txt`

查看NVCC版本

`nvcc -V`

查看cudnn 版本

`cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`



**cudnn 下载**  : 依照对应的CUDA下载 

[https://developer.nvidia.com/rdp/cudnn-download](https://developer.nvidia.com/rdp/cudnn-download)

------

<h3  id="22"> 22. pycharm SSH访问远程服务器 </h3>

1. #### 将pycharm IDE SSH连线到远程服务器段

   - 服务器需要提供host ip地址，port端口号， host name(root)， 登入密码
     - 例如远程服务器的为 test@192.168.3.2， test为hostname, 192.168.3.2为host 地址
   - Pycharm setting中的project interpreter中可以新增服务器上的python解释器，找到SSH Interpreter地方， 输入以上服务器提供的资料进行连接



2. #### 实现资料互传

   - Pycharm>tools > deployment>configuation 的mapping可以设置映射
     - Local path是本地项目的路径
     - Deplotment path是服务器的路径

   

   - Pycharm>tools > deployment>Brose remote host 可以访问远程的资料夹

   

3. #### 开启服务器的终端

   Pycharm > tools > start SSH session 可以在pycharm下方开启远程终端

------

<h3 id="23">23. python pip 安装及管理 </h3>

#### 清华源加速

pip安装包之前， 命令行键入

`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

在进行包的安装即可飞速



#### ReadTimeoutError(“HTTPSConnectionPool）

遇到这样的问题通常就是国外源从国内访问太慢

建议可以直接下载包的whl 或者 tar.gz压缩包到本地进行安装

```
sudo pip3 install xxxxx.whl
```

或者是将压缩包解压缩后进行安装

```
tar -xvzf xxxxx.tar.gz
cd xxxxxx
sudo python3 setup.py install
```

即可完成安装



#### Ubuntu下 安装常用的包

##### matplotlib

```
sudo apt-get python3-matplotlib 
```





------

<h3 id="24">24. Jupyter notebook SSH 远程连接 </h3>

1. 首先确保本机与远程主机都已经安装且可以正常使用jupyter notebook
2. 待完成。。。

------

<h3 id="">25. Docker 指令 </h3>

#### 使用指南

需要了解三个概念

1. 镜像 images
2. 容器 container
3. 仓库 repository

##### 镜像images

镜像构建时， 是一层一层的构建， 前一层的镜像为当前层的基础， 任一层的改变只发生在当前层， 每一层只包含该层需要添加的东西， 任何额外的层都应该在构建结束前清除

官方的 [Docker hub](https://hub.docker.com/) 存储大量的images可以pull

##### 容器container

容器可以有自己root文件系统， 网络配置， 有自己独立的空间， 跟虚拟机类似， 每创建容器时是以镜像为基础添加一层



##### 镜像加速器

主要为服务商提供类似于Docker Hub

- Azure 中国镜像 https://dockerhub.azk8s.cn
- 网易云 https://hub-mirror.c.163.com
- 七牛云加速器 https://reg-mirror.qiniu.com

加速器地址

```json
{
	"registry-mirrors": [
	"https://dockerhub.azk8s.cn",
	"https://hub-mirror.c.163.com", 
	"https://reg-mirror.qiniu.com"
	]
}
```



1. mac 桌面版 添加加速器

   在任务栏点击 Docker for mac 应用图标 -> Perferences... -> Daemon -> Registry mirrors。在列表中填写加速器地址即可。修改完成之后，点击 Apply & Restart 按钮，Docker 就会重启并应用配置的镜像地址了。



2. Linux 添加加速器

```
cd /etc/docker
vim daemon.json

#添加下面内容, 文件必须符合json规范
{
	"registry-mirrors": [
	"https://dockerhub.azk8s.cn",
	"https://hub-mirror.c.163.com", 
	"https://reg-mirror.qiniu.com"
	]
}

#保存后离开， 重启docker
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```



**从Dockerfile 创造镜像image**

与Dockerfile同一层目录下 依照 `docker build -t name:tag `

Ex. 例如建造onnx-tensorrt的镜像, 就先clone下项目的repo， 然后确认repo中有dockerfile

```
#务必加上sudo
sudo docker build -t ubuntu/onnx2trt:v5.0
```

接着就会如下开始build

```
Sending build context to Docker daemon  3.977MB
Step 1/16 : FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
9.0-cudnn7-devel-ubuntu16.04: Pulling from nvidia/cuda
976a760c94fc: Pull complete 
c58992f3c37b: Pull complete 
0ca0e5e7f12e: Pull complete 
f2a274cc00ca: Pull complete 
708a53113e13: Pull complete 
371ddc2ca87b: Pull complete 
f81888eb6932: Pull complete 
19dbd9dd59d6: Pull complete 
e07d92c8415d: Extracting [======================>                            ]  276.3MB/615.8MB
aa4c26baf056: Download complete 
```



**Pull 镜像下来**

```
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

假设从Docker hub pull一个ubuntu的镜像 则`docker pull ubuntu:18.04`

仓库名其实是两段式名称<用户名/软件名>, 如果没给用户名就是默认官方 library/ubuntu

预设的registry是Docker Hub， 所以指令等同于

`docker pull registry.hub.docker.com/ubuntu:latest`





**Run 运行容器**

例如启动bvlc/caffe:cpu这个镜像, cpu这tag一定要指定

```
docker run -it bvlc/caffe:cpu
```



表示启动caffe cpu版本 并且使用ipython交互式

```
docker run -it bvlc/caffe:cpu ipython
```



docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

常用options

- -i ： 以交互模式运行容器，通常与 -t 同时使用；
- -t ： 为容器重新分配一个伪输入终端，通常与 -i 同时使用；





**查看本机装的镜像images**

`docker image ls` 查看目前本机有的images

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
bvlc/caffe          cpu                 0b577b836386        18 months ago       1.64GB
```

`docker image ls -a` 可以看到中间层镜像， 也就是无标签镜像， 不需要删除



#### container  容器常用指令 



**查看容器container**

`docker ps -a`或是`docker container ls -a` : 可以查看到终止状态的容器，前者为旧的指令

`docker container ls` ：可以查看运行中的容器



**跳出运行中的容器**

`ctrl + D` : 可以跳出正在执行中的容器， 容器状态为EXIT终止， 但是并没有被删除



**重新启动终止状态的容器** 

`docker container start <container ID>` ： 一样ID只需要输入前三位就可以



**进入容器** 

确保容器不是终止的状态

`docker attach <container ID>` ：进入容器， 离开后会终止

`docker exec [option] <container ID> bash` : 该方式进入容器， 离开后容器不会终止



**保存镜像到本地（save)与 载入(load)**

`docker save -o images-name.tar images-name:images tag` : 会将images 保存到本地

Images_name.tar 可以自定义

`docker load --input images-name.04.tar` : 将保存在本地的读取载入

**导入与导出容器**

`docker export /containerID/ > test.tar `  ： 将某个容器导出为test.tar

`cat test.tar | docker import - test/test:v1.0` ： 透过Import将一个容器导入到镜像库

`docker import https://www.xxxxx.tgz example/imagerepo` ： 透过url导入



**PS import 与 load区别在于容**

容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状
态），而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入
时可以重新指定标签等元数据信息。



**其他常用**

`docker ps -a` 查看正在运行中的container

`docker stop <container id>` : 停止指定的docker

`docker stop $(docker ps -aq)` ： 停止所有的容器

`docker rm` 删除不需要的容器， 容器必须是停止状态

`docker rm  $(docker ps -q -a)` 删除所有已经停止的容器

`docker container prune -f` 删除所有已经停止的容器

`docker image rm <image id>` 删除镜像， 确保删除之前容器已经移除， 通常image id 只需要取前三位就可以判别

`docker image rm $(docker image ls -q xxxxx)` 可以删除所有仓库名为xxxxx的

`docker system df`:查看docker 占用本地硬盘的状况

`docker attach container-id`: 可以重新进入运行在后台的容器

`dokcer tag 镜像ID 修改后的镜像名:修改后的tag` : 重新修改images

`docker rename 容器ID 修改后的容器名：修改后的容器tag` : 重新修改containers

`docker --version` : 查看docker 版本



**从容器中复制文件到宿主机**

docker cp <容器ID>:/文件路径/文件<这里有空格>/宿主机保存路径 



**保存修改后的镜像**

1. [root@xxxxxxxx /]# 中的xxxxxx是产生的容器ID
2. 将容器打包成镜像` docker commit xxxxxxxxxxx 镜像名称`， 则会自动存储好镜像
3. 如果要删除旧镜像， 会有依赖关系， 必须将新的镜像先save出来到本地， 然后将新与旧镜像删除之后， 在load进刚刚save到本地的镜像



参考https://my.oschina.net/u/2937605/blog/1797218





**基于深度学习的docker image**

Deepo:  

`docker pull ufoym/deepo:cpu`



**在docker上安装jupyter 与本地端浏览器上使用**

1. 先在镜像上安装好jupyter notebook: `pip install jupyter` 

   然后执行`jupyter notebook --generate-config` 生成配置文件(不确定是否完全需要)

2. 安装好之后记得重新保存一个镜像

3. 启动刚刚保存好的镜像 启动容器的方式必须向下面这样

   `docker run -it -p 8888:8888 镜像名称`

   加了一个 -p 表示将本机的port端口导入容器中的port端口， 左边为本机， 右边为容器的

4. 在容器中启动jupyter notebook

   ```py
   jupyter notebook --ip 0.0.0.0 --port 8888 --no-browser --allow-root
   ```

    终端会显示例如下面地址

   http://127.0.0.1:8888/?token=2dd74bc68a1a2b6aabab07b34c678f0b6ef0156f6afc13cf

   在本地浏览器可打开

ps.参考[https://medium.com/@jihung.mycena/docker-%E5%BB%BA%E7%AB%8B-jupyter-container-8084748e2f33



**问题参考**

如果遇到Jupyter notebook启动kernel时 发生

```
ImportError: cannot import name 'create_prompt_application'
#注意该报错会显示在终端， jupyter notebook的服务器无法成功连接
```

表示 ipython 和 prompt-toolkit 版本匹配有问题

解决如下

1.重新安装ipython

```
sudo pip3 uninstall ipython
sudo pip3 install ipython
```

2. 重新强制安装prompt-toolkit

```
sudo pip3 install 'prompt-toolkit<2.1.0,>=2.0.0' --force-reinstall
```

3. 执行完毕后记得存储镜像

------

<h3 id="">26. Shell 脚本语法与执行 </h3>

- 编写脚本时 第一行指定解释器 `#! /bin/bash`
- `chmod +x  xxxxx.sh ` 加上x 表示为sh文件添加可执行（excute） 权限, 否则无法执行

##### 变量Variable

- `variable=123 ` 变量赋值时， 等号左右没有空格
- `${variable}` 使用变量时可以加{}
- 变量 `$0` 表示sh脚本本身
- 变量`$#` 返回参数数量

##### 传参

- `$1, $2, $3....$n` 表示传入的参数， 并且依照顺序， 例如 ./test.sh A B C， 则`$1`就是A   `$2`就是B

##### 条件字句 if

```bash
if condition
then
	command1 
	command2
	...
	commandN 
fi
```

##### if else

```bash
if condition
then
	command1 
	command2
	...
	commandN
else
	comand
fi
```

##### 整数之间判断

| -eq  | 兩數值相等 (equal)                     |
| ---- | :------------------------------------- |
| -ne  | 兩數值不等 (not equal)                 |
| -gt  | n1 大於 n2 (greater than)              |
| -lt  | n1 小於 n2 (less than)                 |
| -ge  | n1 大於等於 n2 (greater than or equal) |
| -le  | n1 小於等於 n2 (less than or equal)    |

example:

```bash
if [ "$#" -ne 1 ]; then     #表示如果输入的参数数量 不为1, 则echo.....
    echo "Usage: $0 <Install Folder>"
    exit
```





#### script的执行

1. `source`

   - Source 是在当前的shell中执行文件， 也就是在父程序中执行
   - 可简写为 `.` , ex : `. test.sh`, 主要中间有空格
   - 让某些指令写入 `~/.bashrc`时候， 就必须使用`source ~/.bashrc`

2. `bash` / `sh` / `./`

   是在当前shell中的child shell中执行脚本， 并且不会影响到父shell

   `./`通常权限不够， 需要透过chmod添加





------

<h3 id="">27. python 安装基于Linux </h3>

```cbash
# wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz 
# tar -xzvf Python-3.7.2.tgz
# cd Python-3.7.2
# ./configure --enable-optimizations
# make
# make install
```

------

<h3 id="">28. onnx (Open Neural Network Exchange) 模型相关 </h3>

- Netron 
  - 在线导入onnx模型可视化 [lutzroeder](https://lutzroeder.github.io/netron/)
  - [lutzroeder github](https://github.com/lutzroeder/netron)
- onnx - tensorrt : 将onnx转换为tensorrt
  - [official onnx-tensorrt github](https://github.com/onnx/onnx-tensorrt)

------

<h3 id="">29. PytorchToCaffe </h3>

- [PytorchToCaffe](https://github.com/xxradon/PytorchToCaffe)









------

<h2 id="" align="center"> others </h2>

#### 查看版本

查看Ubuntu版本

- 三个指令任选一 ： `lsb_release -a`  / `uname -a`  /  `cat /proc/version`

查看Cmake版本

- `cmake --version`

查看OpenCV 版本 (Ubuntu下)

- `pkg-config --modversion opencv`

查看TensorRT 版本 （Ubuntu下）

- `dpkg -l | grep TensorRT`



#### 推荐好用

Sourcegraph : chrome浏览器插件， 可以用来浏览github 项目代码



#### 查看Nvidia GPU 使用率

Nvidia自带了一个nvidia-smi的命令行工具，会显示显存使用情况

```
nvidia-smi
```

如果想不间断持续监控可以使用watch 指令

基础用法：

```
watch [options]  command
```

最常用的参数是 -n， 后面指定是每多少秒来执行一次命令。

监视显存：我们设置为每 3s 显示一次显存的情况：

```
watch -n 3 nvidia-smi
```



### ffmpeg 使用

参考 https://c7sky.com/common-ffmpeg-commands.html

参考 http://www.mikewootc.com/wiki/sw_develop/multimedia/ffmpeg_app_param.html

[FFmpeg](http://ffmpeg.org/) 是一个处理视频和音频内容的开源工具库，可以实现编码、解码、转码、流媒体和后处理等服务

#### 转换格式

Ex. 将mov转换为mp4

```shell
ffmpeg -i input.mov output.mp4
```

- -i ：表示输入文件



#### 视频剪切

Ex. 剪切前10秒

```
ffmpeg -ss 0:0 -t 0:10 -i input.mov output.mp4
```

- -ss : 表示视频开始时间
- -t : 表示持续时间



Ex. 裁剪视频最后10秒

```
ffmpeg -sseof -0:10 -i input.mov output.mp4
```

- -sseof ：表示视频最末尾的开始时间

  

#### 视频画面编辑

EX.画面缩放 1080p - 480p

```
ffmpeg -i input.mov -vf scale=853:480 -acodec aac -vcodec h264 out.mp4
```

- -vf : 用来指定视频滤镜
  - Scale : 缩放比例 ex. 853:480 为了将视频高度变为480px， 则宽度计算为 = 1920*480 / 1080 = 853
  - -acodec = 指定音频使用acc编码
  - -vcodec h264 = 指定视频使用h264编码

EX.剪裁视频画面

假设目前input为1280*720的， 需要剪裁画面正中间640x640的部分则如下

因为xy默认为视频正中间， 所以不需要设置

```
ffmpeg -i input.mov -strict -2 -vf crop=640:640:x:y out.mp4
```

- crop : 表示剪裁视频的画面， 格式为width:height: x:y, width:height表示剪裁后的尺寸， x:y表示剪裁区域的左上角坐标



#### 视频提取帧数

Ex.从视频中提取帧数, 每秒提取24幅图

```
ffmpeg -i twice_v2.mp4 -ss 00:00 -r 24 -f image2 test/image-%05d.jpg
```

- -r 指定抽取的帧率，即从视频中每秒钟抽取图片的数量。1代表每秒抽取一帧，５就表示一秒抽5张图
- -f 指定保存图像的格式
- image-%05d.jpg ：表示存取的图像名称 05d表示00000的格式计数， 前面可以添加路劲



#### 設置幀率

Ex. 設置視頻的幀率

```
ffmpeg -i input.avi -codec:v mpeg4 -r 30 -qscale:v 2 -codec:a copy C.avi
```

- `-codec:v mpeg4` :  使用mpeg4的encoder
- `-qscale:v n` : 或者`-q:v n`表示输出的video的quality， n从1~31， 1为最高级品质， 建议2~5为mp4
- `-qscale:a  `: 或者 `-q:s ` 表示audio的quality， nn
- `-codec:a copy` : 複製

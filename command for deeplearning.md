<h1 align=center>Basic All You Need For Deep</h1>
<p align=right>update 2022.11.2</p>
<h2 align = 'center'>目錄</h2>

> ### Linux

1. [移动到指定目录 cd](#1)

2. [查看文件下的资料 ls, du显示目录或文件大小 ](#2)

3. [創建文件夾 mkdir or rmdir](#3)

4. [创建文件夹 touch](#4)

5. [搜寻文件or文件夹](#5)

6. [查看当前目录下的文件数](#6)

7. [rm 删除指令](#7)

8. [解压缩及打包](#8)

9. [移動文件及复制文件移动, 修改文件名mv](#9)

10. [建立资料连接, 软连接  In](#10)

11. [chmod / chown](#11)

12. [查看CPU，内存占用率, 磁盘占用](#12)

    - swapfile 增加虚拟内存

13. [杀死用户进程 kill PID](#13)

14. [确认本机IP位置及端口查看](#14)

15. dd 拷贝文件， 刻录iso

16. dpkg 安装包指令

17. lsof 查询行程开启的文件列表

18. grep 筛选

19. rename 修改文件名, 后缀

20. Ubuntu终端Terminal快捷键（16.04）

21. SSH 使用方式

22. sftp 使用方式

23. Tmux 终端会话插件

24. Rsync 远程传输文件

25. [其他](#15)

    1. 查看Ubuntu系统版本

    2. Nm 目标文件格式分析

    3. 使用者权限/创建删除用户

    4. Screen 后台执行程序

    5. ldd / readelf 指令查询程序或者依赖的共享库

    6. Linux 查看装置

    7. ldconfig 使用

    8. 在桌面建立快捷图示 desktop

    9. Ubuntu 下翻墙 安装v2ray

    10. `/etc/crontab` 设定定时执行

    11. Ubuntu 清楚缓存脚本

    12. Ubuntu 安装类似win 绘图paint的软件

    13. zsh 终端美化

    14. 好用的rgb值提取插件 colour-pick

        ​      

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
18. [g++ / gcc 编译器相关](#18)

------

> ### Anaconda3

19. [anaconda3 / mini-conda 安装/使用](#19)

------

> ### Git

20. [git 指令集](#20)

------

21. [cuda使用](#21)
22. [python pip 包安装及管理](#23)
23. Jupyter-notebook SSH远程连接 （待完成）
24. Docker指令 ( 镜像, 容器操作 )
25. Shell 脚本(Shell script) 语法与执行 
26. python 安装 基于Linux
27. onnx 模型相关
28. PytorchToCaffe
29. TensorboardX 使用

------

> #### Linux 程序安装

汇整所有需要apt-get 安装的程序安装方法

1. 安装Ubuntu
2. 安装pip
3. 安装openCV
4. 安装cmake
5. Google Protocol buffer

------

> ### Others

1. 查看版本
2. 推荐好用
3. Nvidia 查看GPU使用率
4. ffmpeg 使用
5. pdb debug模式

------

> ### DeepLearning

1. 数据集格式
2. CUDA, CUDNN 安装,移除, 共存

------



> #### IDE 使用技巧（Pycharm, CLion…)

1. SSH 连接方式

------

> ### Socket 网路编程

1. 实现服务端与客户端webcam 实时传输图像



---

> #### 使用nginx 本地实时推流rtmp

---

> #### redis-py使用

<h2 align=center>目录结束<h2>






# Linux

<h3 id="1">1. 移动到指定目录</h3>

1. 到指定目录 ：`cd ./path` 

2. 返回上级目录 ：`cd .. `

3. 如果要指定的目录在上面多层 可以用 ```../../```, 放回基层就放多少个```../```

   Example: 

   ```
   ../../input/dataset
   ```

4. 回到主目录 ```cd ~```

5. 返回上一次所在的目录， 而不是上一级 `cd -`

6. `pwd`获取当前绝对路径

<h3 id="2">2. ls查看文件下的资料、du显示目录或文件大小, df 磁盘大小</h4>






- `ls -a` 列出文件夹下所有文件

- `ls -lah` 列出文件下的文件 以及 效果信息例如文件大小

  

  

du  表示 disk usage

Linux du命令也是查看使用空间的，但是与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看，还是和df命令有一些区别的

```
du [选项][文件]
```

查看一个目录下， 各个文件or文件夹的大小, -s表示 --summarize  仅显示总计，只列出最后加总的值。

-h或--human-readable  以K，M，G为单位，提高信息的可读性。

```
du -sh *
```

—max-depth 可以设置查看目录的深度

```
du -lh --max-depth=1 #这就只会查看当前目录下的文件或者文件夹的大小
```







Df 表示 disk free

```shell
# df -h 
Filesystem      Size  Used   Avail Use% Mounted on 
/dev/sda6       29G   4.2G   23G   16%     / 
udev            1.5G  4.0K   1.5G   1%     /dev 
tmpfs           604M  892K   603M   1%     /run 
none            5.0M     0   5.0M   0%     /run/lock 
none            1.5G  156K   1.5G   1%     /run/shm 
```





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



一般最简单的方式可以用, 直接用grep过滤打印出来的文件

```
find / |grep "name"
```



依照文件名找

```find / -iname "文件名"```：

找包含xxx字符的文件, 加单引号跟星号

`find / -iname '*xxx*'`



依照目录找

```find /  -iname "文件夹名" -type d```

依照档案名找

`find / -iname "档案" -type f`

`find / -iname '*.log' -type f ` : 找副档名为log, 可以依照需求自己改



**查找大文件**

参考 https://www.jb51.net/article/109696.htm

```
find . -type f -size +800M
```





------

<h3 id="6">6. 查看当前目录下的文件数</h4>
```
ls path |wc -l
```




统计path directories下的文件个数

wc表示wordcount

以上指令相当于先用ls将文件每行print出， 然后wc计算多少列



```
find path/ -type f |wc -l
```

列出path下所有文件总共的数目





------

<h3 id="7">7. rm 删除指令</h4>




rm + 要删除的文件 一个也可以多个

```
rm file1 file2
```

rm -f 表示强制删除

rm -r 表示递归删除， 可以删除包含文件夹下面的所有文件

```
rm -rf path/
```

ps.使用时请千万谨慎



如果想在某个文件夹下， 保留特定文件不删除， 其他删除

参考 https://www.cnblogs.com/wjoyxt/p/10408423.html

```shell
rm -rf !(file1 | file2) #删除除了file1, file2的文件rm -rf !(*jpg)  #删除文件名不以jpg结尾的文件rm -rf *@(jpg|png) #删除文件名以jpg或png结尾的文件, 千万记得 | 中间不要有空格
```



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



#### 移动文件除了某个

除了xxxxx 之外， 其他移动到dst_path

```
mv !(xxxxx) dst_path
```



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

<h3 id="11">11. chmod / chown</h3>

#### chmod

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
u ： user g ：group o ：othersa ： all(所有身份)
```

```
r : readw : writex : execute
```



##### 将底下包含子文件夹都改变权限

加个-R 

```
chmod -R xxx 
```



#### chown

Linux chown（英文全拼：**change owner**）命令用于设置文件所有者和文件关联组的命令

所有的文件皆有拥有者。利用 chown 将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户 ID，组可以是组名或者组 ID，文件是以空格分开的要改变权限的文件列表

chown 需要超级用户 **root** 的权限才能执行此命令



将文件 file1.txt 的拥有者设为 runoob，群体的使用者 runoobgroup :

```
chown runoob:runoobgroup file1.txt
```

将当前前目录下的所有文件与子目录的拥有者皆设为 runoob，群体的使用者 runoobgroup:

```
chown -R runoob:runoobgroup *
```





------

<h3 id="12">12. 查看CPU、内存占用率 </h3>

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





#### swapfile 設置 增加虛擬內存

切割原磁盤空間， 例如需要4GB空間作為內存

1. `sudo fallocate -l 4G /swapfile` :  l用來指定分配大小， /swapfile作為swap使用的檔案存放路徑
2. `. sudo chmod 600 /swapfile` ：为设置好的档案添加root权限
3. `sudo mkswap /swapfile` :  告知系统将此档案作为swap使用
4. `sudo swapon /swapfile` ：启用swap

将swap设定为开机时自动挂载（ubuntu 18之下）

```
sudo vim /etc/fstab添加以下到最下排/swapfile   none swap    sw 0 0
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

<h3 id="14">14. IP 内网及外网地址及端口查看 </h3>

`-a`: -a (all)显示所有选项，默认不显示LISTEN相关
`-t`: (tcp)仅显示tcp相关选项
`-u`: (udp)仅显示udp相关选项(all)显示所有选项，默认不显示LISTEN相关
`-t`: (tcp)仅显示tcp相关选项
`-u`: (udp)仅显示udp相关选项
`-n`: 拒绝显示别名，能显示数字的全部转化成数字。
`-l`: 仅列出有在 Listen (监听) 的服務状态
`-p`: 显示建立相关链接的程序名
`-r`: 显示路由信息，路由表
`-e`: 显示扩展信息，例如uid等
`-s`: 按各个协议进行统计
`-c`: 每隔一个固定时间，执行该netstat命令。

```ifconfig -a``` : 确认内网IP位置，如果是连接wifi 请看wlan0底下inet的位置

```netstat -anptl``` : 确认端口

`netstat -an |grep {portnum}` :  可查看portnum端口连接的情况

``` bash
$ netstat -an |grep 8000
>>>
tcp        0      0 0.0.0.0:8000            0.0.0.0:*               LISTEN     
tcp6       0      0 :::8000                 :::*                    LISTEN   
```

`netstat -nultp` : 查看已经正在使用的端口

```bash
$ netstat -nultp #主要是加了-l
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:8002            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:10050           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:3947          0.0.0.0:*               LISTEN      2942980/python      
tcp        0      0 0.0.0.0:622             0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:31983         0.0.0.0:*               LISTEN      2705969/node        
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:22705         0.0.0.0:*               LISTEN      2942980/python      
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      -            
```



`curl ifconfig.me` : 确认外网ip地址

------

<h3 id="15">15. dd 拷贝文件， 刻录iso </h4>


参考 https://www.cnblogs.com/linuxde/p/8719253.html

dpkg 是Debian package的简写，为”Debian“ 操作系统 专门开发的套件管理系统，用于软件的安装，更新和移除。 所有源自"Debian"的Linux的发行版都使用 dpkg,   例如"Ubuntu"

------

<h3 id="17">17. lsof 查询行程开启的文件列表</h4>




参考 https://blog.gtwang.org/linux/linux-lsof-command-list-open-files-tutorial-examples/

列出一位user或者多位 所开启的文件

```
lsof -u user1, user2, user3
```

列出指定程式所开启的文件

```
lsof -c xxxx
```

根据PID列出开启的文件

```
lsof -p 14662,14678,14979
```

列出所以程序开启的文件列表

```
lsof
```

依照上面， 可以用grep筛选 xxxx 关键字的文件

```
lsof | grep xxxxx
```

------

<h3 id="18">18. grep 筛选</h4>




参考https://blog.gtwang.org/linux/linux-grep-command-tutorial-examples/

再指定的文件汇总搜寻关键字

```
grep 搜寻关键字 文件1 文件2
```

ex

```shell
# 在 /etc/os-release 檔案中搜尋 Ubuntu 關鍵字grep Ubuntu /etc/os-release
```

```shell
# 在 /etc/*.conf 中搜尋 network 關鍵字grep network /etc/*.con
```

```shell
# 篩選含有 network 關鍵字的檔案名稱ls /etc/ | grep network
```



不分大小写 -i

```shell
# 不分大小寫grep -i Ubuntu /etc/os-release
```

标示出行数 -n

```shell
# 標示行號
grep -n Ubuntu /etc/os-release
```

排除关键字 -v

```shell
# 顯示不包含 Ubuntu 關鍵字的行
grep -v Ubuntu /etc/os-release
```

递归搜寻 -r

```shell
# 在 /etc/ 下所有檔案中搜尋 ubuntu
grep -r ubuntu /etc/
```

更多用法查阅参考链接

------

<h3 id="19">19. rename 修改文件名, 后缀</h4>




参考http://einverne.github.io/post/2018/01/rename-files-batch.html

rename 可以用来批量重新命名

命令组成如下

```
rename [options] "s/oldname/newname/" file
```

看选项options 部分，`rename` 支持以下的选项：

- `-v` 将重命名的内容都打印到标准输出，v 可以看成 verbose
- `-n` 测试会重命名的内容，将结果都打印，但是并不真正执行重命名的过程
- `-f` force 会覆盖本地已经存在的文件
- `-h` `-m` `-V` 分别为帮助，帮助，版本
- `-e` 比较复杂，可以通过该选项，写一些脚本来做一些复杂的事情



当命令中最后 file 为 `*` 时表示，匹配当前文件夹下所有文件，如果为 `?` 时则匹配只有一个字符的文件名



修改文件名中特定的字段

```
rename "s/AA/aa/" *  # 把文件名中的AA替换成aa
```

修改文件后缀名

```
rename "s/.html/.php/" *     # 把.html 后缀的改成 .php后缀
rename "s/.png/.jpg/" *      # 将 png 改为 jpg
```

批量添加文件后缀

```
rename "s/$/.txt/" *     # 把所有的文件名都以txt结尾
```

批量删除文件名

```
rename "s/.txt//" *      # 把所有以.txt结尾的文件名的.txt删掉
```



------

<h3 id="20">20. Ubuntu终端Terminal快捷键</h4>




#### 版本16.04

参考https://blog.csdn.net/qq_15166261/article/details/104469606?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161032979616780277019548%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=161032979616780277019548&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-2-104469606.first_rank_v2_pc_rank_v29&utm_term=Ubuntu%20%E7%BB%88%E7%AB%AF%E5%B8%B8%E7%94%A8%20%E5%BF%AB%E6%8D%B7&spm=1018.2226.3001.4187

画面中按下win, 可以跳出快捷提示

```
常用的快捷键
| 快捷键 |功能 |
Tab 自动补全
Ctrl+a 光标移动到开始位置
Ctrl+e 光标移动到最末尾
Ctrl+k 删除此处至末尾的所有内容
Ctrl+u 删除此处至开始的所有内容
Ctrl+d 删除当前字符
Ctrl+h 删除当前字符前一个字符
Ctrl+w 删除此处到左边的单词
Ctrl+y 粘贴由Ctrl+u， Ctrl+d， Ctrl+w删除的单词
Ctrl+l 相当于clear，即清屏
Ctrl+r 查找历史命令
Ctrl+b 向回移动光标
Ctrl+f 向前移动光标
Ctrl+t 将光标位置的字符和前一个字符进行位置交换
Ctrl+& 恢复 ctrl+h 或者 ctrl+d 或者 ctrl+w 删除的内容
Ctrl+S 暂停屏幕输出
Ctrl+Q 继续屏幕输出
Ctrl+Left-Arrow 光标移动到上一个单词的词首
Ctrl+Right-Arrow 光标移动到下一个单词的词尾
Ctrl+p 向上显示缓存命令
Ctrl+n 向下显示缓存命令
Ctrl+d 关闭终端
Ctrl+xx 在EOL和当前光标位置移动
Ctrl+x@ 显示可能hostname补全
Ctrl+c 终止进程/命令
Shift+上或下 终端上下滚动
Shift+PgUp/PgDn 终端上下翻页滚动
Ctrl+Shift+n 新终端
alt+F2 输入gnome-terminal打开终端
Shift+Ctrl+T 打开新的标签页
Shift+Ctrl+W 关闭标签页
Shift+Ctrl+C 复制
Shift+Ctrl+V 粘贴
Alt+数字 切换至对应的标签页
Shift+Ctrl+N 打开新的终端窗口
Shift+Ctrl+Q 管壁终端窗口
Shift+Ctrl+PgUp/PgDn 左移右移标签页
Ctrl+PgUp/PgDn 切换标签页
F1 打开帮助指南
F10 激活菜单栏
F11 全屏切换
Alt+F 打开 “文件” 菜单（file）
Alt+E 打开 “编辑” 菜单（edit）
Alt+V 打开 “查看” 菜单（view）
Alt+S 打开 “搜索” 菜单（search）
Alt+T 打开 “终端” 菜单（terminal）
Alt+H 打开 “帮助” 菜单（help）
另外一些小技巧包括：在终端窗口命令提示符下，连续按两次 Tab 键、或者连续按三次 Esc 键、或者按 Ctrl+I 组合键，将显示所有的命令及工具名称。Application 键即位置在键盘上右 Ctrl 键左边的那个键，作用相当于单击鼠标右键。

Terminal终端
CTRL + ALT + T: 打开终端
TAB: 自动补全命令或文件名
CTRL + SHIFT + V: 粘贴（Linux中不需要复制的动作，文本被选择就自动被复制）
CTRL + SHIFT + T: 新建标签页
CTRL + D: 关闭标签页
CTRL + L: 清楚屏幕
CTRL + R + 文本: 在输入历史中搜索
CTRL + A: 移动到行首
CTRL + E: 移动到行末
CTRL + C: 终止当前任务
CTRL + Z: 把当前任务放到后台运行（相当于运行命令时后面加&）
~: 表示用户目录路径
```

------

<h3 id="16">21. SSH 使用方式</h4>




#### 端口映射

例如将本地地址映射到服务器上， 假设服务器节点为g01n02, port为22

```shell
ssh -NfL 127.0.0.1:2222:g01n02:22 username@host
```

映射完之后， 可以直接

```
ssh username@127.0.0.1 -p 2222
```

就能够访问g01n02 服务器节点





#### 透过ssh上传文件or下载文件到服务器

上传文件到服务器 

```
scp -P 端口号 要传输的文件 用户名@主机ip:路径
```

Ex.

```
scp -P 22 darkent.zip root@123.45.2.345:/home/username/workplace
```



从服务器下载文件到本地

```
scp -P 端口号 用户名@主机ip:要下载文件的路径 空格 本地路径
```

如果要下载or上传整个目录 加上-r







#### 端口被占用

参考 https://blog.csdn.net/lsr40/article/details/77775858

1. 查找被占用的端口的pid进程， 最后一排就是pid

   ```
   netstat -apn | grep 2181
   ```

2. 通过kill -9 杀掉

   ```
   kill -9 pid
   ```

3. 也可以先用ps -ef | grep pid 查看哪一个用户创建该进程， 避免杀错

   ```
   ps -ef | grep pid
   ```

------

<h3 id="16">22. sftp使用</h4>




通常可以用来从服务器上下载数据

例如 `get`

```
sftp username@host #先登入

然后可以cd到需要的地方下载比如 下载xxx文件
get xxxx.后缀
```

下载可以用`put`

------

<h3 id="16">23. Tmux 终端会话插件</h4>




启动会话 删除等操作

```
tmux new-session -s "name" 启动会话
tmux at -t "name" 回复会话
tmux ls 列出会话
tmux kill-session -t 'name' 删除会话
crtl + B + D离开会话
```

启动控制台进行操作

```
ctrl + B 启动控制台
p ： previous bash 
n : next bash
shift + & : close current bash
```

在一个窗格中进行分割

```
% 垂直分割" 水平分割x : 关闭窗格[ : 在窗口中上下滚动, 支持直接按page up or page down上下, 因为在窗口中无法正常shift+up查看终端信息， 按q 可以退出模式ctrl + B + up or down : 调整窗口的高度
```



#### 在tmux会话下 vim 功能失效

例如visual block模式下无法显示highlight的文字

到`~/.vimrc` 下添加

```
set term=xterm-256colorset t_Co=256
```

------

<h3 id="16">25. Rsync 传输文件</h4>



```
rsync 参数 来源文件 目的文件
```

-v : verbose 输出较详细文件

-r : 递归子目录下所有文件

-z : 启用压缩, 可将要传输的文件先打包， 然后到目的之后解压缩

-avzh : 类似scp， 远程传输文件, 将上述命令综合输入

-u ：不会修改目标目录下已经有的文件

```
rsync -avzh /path/xxx.xx root@192.168.xx.xx:/path/
```

`--include`: 仅传输固定的文件or目录 ex. `--include '*.py'` 

`--exclude`: 排除不上传的文件or目录 ex. `--exclude '*.py'`

```
rsync -avzh /path/xxx.xx --exclude 'path/*.py' root@192.168.xx.xx:/path/
rsync -avzh /path/xxx.xx --exclude={'*.txt', 'dir3', 'dir4'} root@192.168.xx.xx:/path/ #排除多个文件
```



如果是长期需要进行两地同步推荐使用

 `--exclude-from=` : 从txt文件中指定需要**排除**的文件

`--files-from=` ： 从txt文件中指定需要**传输**的文件

**transfered_files.txt** 定义了需要传输的文件

``` 
AlphaPose/
yolov5/
yolov5_4.0/
spc_project.py
SPC_config.py
spc_run_server.py
models/
```

使用--files-from=指定定义好的文件

```
rsync -avzh ./ --files-from=transfered_files.txt user@127.0.0.1:/home/user/VScodeProjects/path/
```



---

<h3 id="16">25. 其他</h4>








#### 查Ubuntu系统版本

1. `cat /etc/issue （简单）`
2. `cat /etc/lsb-release（具体）`
3. `uname -a（内核）`

#### nm 目标文件格式分析

主要用于列出目标的符号清单， 对于每一个符号，nm列出其值(the symbol value)，类型（the symbol type）和其名字(the symbol name)

查看动态库 (so) ：`nm -D /path/to/xxxx.so`

這樣會打印出全部內容， 可以用grep篩選 如

`nm -D /path/to/xxxx.so | grep xxxx`



| **符号类型** | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| A            | 该符号的值是绝对的，在以后的链接过程中，不允许进行改变。这样的符号值，常常出现在中断向量表中，例如用符号来表示各个中断向量函数在中断向量表中的位置。 |
| B            | 该符号的值出现在非初始化数据段(bss)中。例如，在一个文件中定义全局static int test。则该符号test的类型为b，位于bss section中。其值表示该符号在bss段中的偏移。一般而言，bss段分配于RAM中 |
| C            | 该符号为common。common symbol是未初始话数据段。该符号没有包含于一个普通section中。只有在链接过程中才进行分配。符号的值表示该符号需要的字节数。例如在一个c文件中，定义int test，并且该符号在别的地方会被引用，则该符号类型即为C。否则其类型为B。 |
| D            | 该符号位于初始话数据段中。一般来说，分配到data section中。例如定义全局int baud_table[5] = {9600, 19200, 38400, 57600, 115200}，则会分配于初始化数据段中。 |
| G            | 该符号也位于初始化数据段中。主要用于small object提高访问small data object的一种方式。 |
| I            | 该符号是对另一个符号的间接引用。                             |
| N            | 该符号是一个debugging符号。                                  |
| R            | 该符号位于只读数据区。例如定义全局const int test[] = {123, 123};则test就是一个只读数据区的符号。注意在cygwin下如果使用gcc直接编译成MZ格式时，源文件中的test对应_test，并且其符号类型为D，即初始化数据段中。但是如果使用m6812-elf-gcc这样的交叉编译工具，源文件中的test对应目标文件的test,即没有添加下划线，并且其符号类型为R。一般而言，位于rodata section。值得注意的是，如果在一个函数中定义const char *test = “abc”, const char test_int = 3。使用nm都不会得到符号信息，但是字符串“abc”分配于只读存储器中，test在rodata section中，大小为4。 |
| S            | 符号位于非初始化数据区，用于small object。                   |
| T            | 该符号位于代码区text section。                               |
| U            | 该符号在当前文件中是未定义的，即该符号的定义在别的文件中。例如，当前文件调用另一个文件中定义的函数，在这个被调用的函数在当前就是未定义的；但是在定义它的文件中类型是T。但是对于全局变量来说，在定义它的文件中，其符号类型为C，在使用它的文件中，其类型为U。 |
| V            | 该符号是一个weak object。                                    |
| W            | The symbol is a weak symbol that has not been specifically tagged as a weak object symbol. |
| -            | 该符号是a.out格式文件中的stabs symbol。                      |
| ?            | 该符号类型没有定义                                           |



#### 使用者权限/创建删除用户

#### 创建用户

参考https://blog.gtwang.org/linux/linux-useradd-command-tutorial-examples/

```shell
$ sudo useradd -m jason -s /bin/bash 
$ sudo passwd jason
$ sudo adduser jason sudo
$ su jason
```

- 创建了可以登录的meow用户并使用/bin/bash作为shell。
- 设置密码。
- 为jason用户增加管理员权限。
- 切换登录用户为jason。

如果要让用户只能访问一个文件夹可以加上 -d

```
sudo useradd -m jason -s /bin/bash -d /home/jason
```

为用户指定id

```
sudo useradd -m jason -s /bin/bash -d /home/jason -u 999
```

为用户加入既有的group, 除了主要group 一位user能同时在不同的group中

```
sudo useradd -m jason -s /bin/bash -d /home/jason -g team
```



如果想为jason添加管理员权限可以

```
sudo vi /etc/sudoers
```

找到

root ALL=(ALL:ALL) ALL 

下面添加

jason ALL=(ALL:ALL) ALL



#### 删除用户

```
killall -u username
```

可以删除用户所有信息

如果碰到用户进程还在的时候， 可以直接杀光用户进程

```
killall -u username
```



#### su 指令

有些文件必须要root权限才能修改， 因此

`su [-fmp] [-c command] [-s shell] [--help] [--version] [-] [USER [ARG]]`

- 求换为root使用者: `su root`, 假如用户名为stephen则 `su stephen`

  - 初此设定需要`sudo passwd root` 设定密码， 设定好之后， 才能`su root`

- 显示当前用户：`whoami`

  如果以上出现`su: Authentication failure`, 就需要为root设定pwd

  `sudo passwd root`， 接着设定好密码

- 退出当前用户 ： `exit` / `logout` / `ctrl + d`





#### screen 后台执行程序

ref https://blog.csdn.net/weixin_42331537/article/details/89962801?depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-2&utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-2

这个绝对是常用云服务器执行任务的利器， 将程序放在后台运行即使登出或者与本地断开连线还是在运行

下面是几个常用的指令

```shell
screen -S session_name           # 新建一个叫session_name的sessionscreen -ls（或者screen -list）    # 列出当前所有的sessionscreen -r session_name           # 回到session_name这个sessionscreen -d session_name           # 远程detach某个sessionscreen -d -r session_name        # 结束当前session并回到session_name这个session
```

通常先用 `screen -S xxxxx`（任意取名）创建session 并且进入

然后进入之后就可以执行需要在后台执行的程序， 比如开始训练模型

接着`ctrl + A + D`可以退出当前的session， 但是只是离开并不影响程序继续运行

如果想在进去观察的话可以`screen -r 你的session_name` ， 如果进不去， 可以

`screen -D -r session_name` 就可以



如果遇到任务没结束需要强制删除会话（比如占用显存的情况）

`screen -S session_name -X quit`











#### ldd / readelf 指令查询程序或者依赖的共享库

`ldd 选项 file`

一般可以查询程序或者库文件依赖的共享库列表

比如 有一个执行文件 test

可以

```shell
ldd test>>> 输出， 就可以看到连接许多共享库linux-vdso.so.1 =>  (0x00007ffe3dbc1000)libm.so.6 => /lib64/libm.so.6 (0x00007f17a5b55000)libselinux.so.1 => /lib64/libselinux.so.1 (0x00007f17a592e000)libtinfo.so.5 => /lib64/libtinfo.so.5 (0x00007f17a5704000)
```

选项

`-v` : 看更多资讯包含版本信息

#### readelf

参考 https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/readelf.html


就是用于读取elf文件的，一般elf文件是


- 可重定位的对象文件(Relocatable file)
由汇编器汇编生成的 .o 文件

- 可执行的对象文件(Executable file)
可执行应用程序

- 可被共享的对象文件(Shared object file)
动态库文件，也即 .so 文件

如果想知道一个程序可运行的架构平台

`$readelf -h main| grep Machine`

想知道一个库需要什么其他的库支持

`$readelf -d xxxxx.so`


#### alias 环境变量设置别名

在环境变量中添加

```
alias 快捷指令="终端指令"

EX.
alias wo="cd /home/workplace"
则在终端输入wo， 就相当于 cd /home/workplace
```



#### Linux查询装置

1. 查主机板 ：`sudo dmidecode | more` 



#### ldconfig

ref https://www.cnblogs.com/schips/p/10183111.html

ldconfig是一个动态链接库管理命令，其目的为了让动态链接库为系统所共享。

ldconfig的主要用途：

**默认搜寻/lilb和/usr/lib，以及配置文件/etc/ld.so.conf内所列的目录下的库文件。**

搜索出可共享的动态链接库，库文件的格式为：lib***.so.**，进而创建出动态装入程序(ld.so)所需的连接和缓存文件。

缓存文件默认为/etc/ld.so.cache，该文件保存已排好序的动态链接库名字列表

ldconfig通常在系统启动时运行，而当用户安装了一个新的动态链接库时，就需要手工运行这个命令。

ldconfig需要注意的地方：

```
1、往/lib和/usr/lib里面加东西，是不用修改/etc/ld.so.conf文件的，但是添加完后需要调用下ldconfig，不然添加的library会找不到。

2、如果添加的library不在/lib和/usr/lib里面的话，就一定要修改/etc/ld.so.conf文件，往该文件追加library所在的路径，然后也需要重新调用下ldconfig命令。比如在安装[MySQL](http://lib.csdn.net/base/14)的时候，其库文件/usr/local/mysql/lib，就需要追加到/etc/ld.so.conf文件中。命令如下：

\# echo "/usr/local/mysql/lib" >> /etc/ld.so.conf

\# ldconfig -v | grep mysql

3、如果添加的library不在/lib或/usr/lib下，但是却没有权限操作写/etc/ld.so.conf文件的话，这时就需要往export里写一个全局变量LD_LIBRARY_PATH，就可以了。
```



#### 桌面建立快捷图示

比如ubuntu下安装clion, 解压缩之后， 只有文件夹， 要到bin下 `./clion.sh`启动程序， 我们可以在桌面建立一个clion.desktop， 连接到这个sh进行快捷启动



```
[Desktop Entry]Encoding=UTF-8Name=xxx//可执行文件Exec=sh  /路径/你前面生成的可执行的shell文件.sh       //.sh可执行文件的绝对路径, 前面的sh 命令不要丢哦Icon=/usr/local/share/icons/jesh.png  //软件的图标文件路径 ico也可Info="Spark"Categories=GTK;Network;message; //可写可不写Comment="Gtk+ based like QQ"  //提示性信息 ，可写可不写Terminal=falseType=ApplicationStartupNotify=true
```





#### apt-file 寻找依赖

首先安装

```
apt-get updateapt-get install apt-fileapt-file update
```





使用方法： 

如果在运行过程中报错例如缺少 xxxx.so

则利用寻找文件

```
apt-file search xxxx.so
```

接着就会先找这个文件在哪？

如果没有， 就会找这个库文件是属于哪个包的



如果想看这个包中有哪些文件可以

```
apt-file list opencv-python
```



#### Ubuntu 下翻墙 安装 v2ray

参考 https://mahongfei.com/1776.html

1. 下载`V2ray`客户端，这里以最简单的`AppImage`文件为例,下载链接：

https://github.com/Qv2ray/Qv2ray/releases/download/v1.99.6/Qv2ray-refs.tags.v1.99.6-linux.AppImage



2. 下载核心文件，下载链接：

   https://github.com/v2ray/v2ray-core/releases/download/v4.22.1/v2ray-linux-64.zip

3. 3:进入v2ray下载的根目录，执行以下命令：

   ```
   sudo chmod +x ./Qv2ray-refs.tags.v1.99.6-linux.AppImage
   ```

4. 仍然在v2ray根目录下打开终端，输入以下命令， 会启动软件

```
sudo ./Qv2ray-refs.tags.v1.99.6-linux.AppImage
```

执行4后会出现主界面，点击`首选项` preferences

然后在V2ray Settings 

```
-> Core Executable Path 设定核心文件中的v2ray-> V2ray Assets Directory 设定 核心文件路径 
```

设置好之后按OK



5. 接着在订阅（subscription)
   1. 左下角新增
      1. 输入服务商名字
      2. 输入订阅url
      3. 然后update subscription Data
6. 首选项（preference）中， 入站设置（Inbound Settings)
   1. 勾选 Set system Proxy
7. 点击**连接 connect** ， 开始上网





#### /etc/crontab

设置定时执行程序

```
vim /etc/crontab
```

比如每隔3分钟执行一次xxx.sh脚本

```shell
#打开文件后在最下面添加

#依序是min hour day month dayofwork user command
#这里只列出每隔几分钟执行的方法

*/3 *   * * * root sh /home/path/to/xxx.sh
```

参考 https://blog.csdn.net/lxz978161079/article/details/80662346?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param







#### Ubuntu 清除缓存脚本

以下内容复制到sh文件， 可以搭配crontab 定时使用

```shell
#!/bin/bash
sync; echo 1 > /proc/sys/vm/drop_caches
sync; echo 2 > /proc/sys/vm/drop_caches
sync; echo 3 > /proc/sys/vm/drop_caches
swapoff -a && swapon -a
```





#### Ubuntu 安装类似win 绘图paint的软件

如下安装

```
sudo apt-get install kolourpaint4
```

然后在左下角Show Applications中可以找到KolourPaint的图示





#### Zsh 终端美化插件

项目地址 ：https://github.com/ohmyzsh/ohmyzsh

1. Ubuntu安装步骤

```shell
sudo apt install zshsh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"#如果该步骤遇到ssl 无法建立问题， 尝试sh -c "$(wget -O- https://gitee.com/mcornella/ohmyzsh/raw/master/tools/install.sh)"#参考 https://github.com/ohmyzsh/ohmyzsh/issues/9528
```

2. 当前terminal 简单切换

```shell
exec bash或者 exec zsh
```

3. 设置default的shell样式， 设置完之后restart terminal就可以

```shell
chsh -s /bin/bash或者chsh -s /bin/zsh
```



#### 好用的rbg 值提取插件 colour pick

项目参考 github.com/sturatlangridge/ColourPicker

ubuntu下安装

```shell
sudo snap install pick-colour-picker
```



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
- `yy`: 快速复制当前行
- `p`:贴上
- `pp` : 快速黏贴上yy复制的内容
- `:X` : 大写的X主要是用来加密文件
  - 输入之后， 会要求输入二次密码， 两次都需要一样
  - 下次开启就需要输入密码才能打开文件， 密码务必要记住， 否则解不开
  - 如果需要重新设置为无密码， 只要`:X`之后， 直接回车两次就行， 就设置为无密码了

- 多行注释的步骤
  - 首先Ctrl + V 进行visual block模式 然后上下选择需要注释的行， 然后shift + i(insert)， 之后输入注释符号 # or // 在按esc， 就能发现多行已经注释了， 如果解除注释一样多行选择要删去的注释地方， 然后按 `d` , 就能删除注释符号
- 搜索功能 
  - 首先：进行模式，然后输入`/` 然后在输入要搜索的名test
  - 例如 `/test`, 就会搜索test这个字符
  - `n` 表示搜索下一个， `N` 表示搜索上一个

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



#### 单行Shift 移动

参考 http://blog.wildsky.cc/posts/vim-move-line/

可以类似Pycharm 用shift 移动代码行的效果， 也能可视模式下选定多行 然后移动

```vim
"move line up/down"
nnoremap <S-DOWN> :m .+1<CR>==
nnoremap <S-UP> :m .-2<CR>==
inoremap <S-DOWN> <Esc>:m .+1<CR>==gi
inoremap <S-UP> <Esc>:m .-2<CR>==gi
vnoremap <S-DOWN> :m '>+1<CR>gv=gv
vnoremap <S-UP> :m '<-2<CR>gv=gv
```









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
mkdir -p ~/.vim/pack/git-plugins/startgit clone --depth 1 https://github.com/w0rp/ale.git ~/.vim/pack/git-plugins/start/ale
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
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git' #可不加
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin' #可不加
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

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
```



之后任何要安装的插件放在#begin #end 之间

通常格式会是Plugin xxx/xxxxx

然后命令行输入

```
vim +PluginIndstall +qall
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



#### vimplus

项目链接 https://github.com/chxuan/vimplus

安装介绍 https://www.cnblogs.com/highway-9/p/5984285.html

------

<h3 id = "18">18.  g++ / gcc 编译器相关</h3>
<h4>g++編譯/執行</h4>

1. 使用g++ 編譯cpp文件：

- ```g++ filename.cpp``` : 自動生成a.out文件
- ```g++ filename.cpp``` -c 生成文件名 : 自動生成指定的文件名

2. ```./檔名``` ：執行文件



#### 搭配参数

`-I` : 表示依照指定的路径搜索头文件, Ex. `-I./include/` 表示将./include/目录作为第一个寻找头文件的目录，寻找的顺序是： ./include/ --> /usr/include --> /usr/local/include

`-L` : 指定库文件的搜索路径 Ex. `-L/lib/ -lapple` 表示到/lib/目录下寻找libapple.so库文件， 可以连接多个

`-l` :  小写L ， 指定要连接的库名称

Ex. 输出compress 执行文件， 源文件compress.cpp 连接头文件在/home/include, -L连接库文件 libz.so, 

```shell
g++ -o compress  compress.cpp  -I/home/include/  -L/lib/  -lz
```





#### 连接头文件与源文件的编译方式

假设有三个文件包含

**function.cpp** ：源文件

**function.h** ： 头文件

**main.cpp** ： main 执行文件

```g++ -o out Function.cpp Function.h mian.cpp``` ： 将三个文件一起编译

接着会生成out(这个out是可以指定名称的)

然后执行 ```./out``` 就能运行文件



#### gcc / g++生降版本

以下用gcc举例， 记得gcc, g++需要同版本



1. 当前版本为7.3，需要降到4.8

`sudo apt-get install gcc-4.8` （如果是g++  `g++-4.8`)

2. 装完后进入到/usr/bin目录下
3. 输入`ls -l gcc*` 查看连接状况

发现gcc链接到gcc-7.0, 需要将它改为链接到gcc-4.8，方法如下:

```shell
sudo mv gcc gcc.backup #备份sudo ln -s gcc-4.8 gcc #利用软连接重新链接
```

完成, 可以在用`ls -l gcc*`检查一下





<h4 id = >g++ 其它</h4>

查看g++ 版本 ：`g++ -v`

版本安装， 例如4.8版  `sudo apt-get install gcc-4.8`

查看安装是否成功 `ls /usr/bin/gcc*`

设置优先级 `sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 100`

查看设置结果 `sudo update-alternatives --config gcc`

------

<h3 id="19">19. anaconda3/ Mini-Conda 安装/使用 </h3>

#### 安装miniconda3

下载  https://conda.io/en/latest/miniconda.html 下载完会是sh脚本

sudo sh 安装之后 如果选择不开机自动激活

则需要去 /home/yourusername/miniconda3/bin/下 source activate激活一下之后就可以使用







#### 虚拟环境

查看当前所有环境 ： `conda info --env` or `conda env list`

创建虚拟环境 virtualEnv : `conda create -n 环境名 python=3.6 -y`

如果无法创建出现NotWritableError : The current user does not have write permissions to a required path.



可以改变目录owner

```shell
sudo chown -R 你的用户名 miniconda3///或者sudo chown -R 你的用户名 anaconda3
```





启动环境 ：`source activate 环境名`

关闭环境 ：`conda deactivate`

删除环境 :`conda env remove -n 环境名`

安装package ：`conda install 包名` 用conda设置的源安装

安装package 方法二 : `pip install` 



#### 复制原来创建好的环境

假设有一个AAA环境， 要创建一个与AAA相同环境的BBB环境

```shell
conda create -n BBB --clone AAA
```



#### 将conda 自创建的环境打包到其他服务器上使用

先安装conda-pack

```
pip install conda-pack
```

利用这个包将想打包的环境打包

```
conda pack -n 环境名 #输入之后会在当前目录下生成一个tar.gz文件
```



<span style="color:red">如果遇到CondaPackError:Cannot pack an environment with editable packages</span>

```shell
conda pack -n 环境名 --ignore-editable-packages
```





然后在把这个文件放到需要使用环境的服务器下解压缩, 比如解压缩出来是haha文件

```
source haha/bin/activate
```

激活后 环境就已经改变了



#### 导出现在的环境为yaml文件

在想要导出的环境下

conda导出已有环境：
`conda env export > environment.yaml`

环境会被保存在 environment.yaml文件中。当我们想再次创建该环境，或根据别人提供的.yaml文件复现环境时，可以：

`conda env create -f environment.yaml`

就可以复现安装环境。移植过来的环境只是安装了你原来环境里用conda install等命令直接安装的包，你用pip之类装的东西没有移植过来，需要你重新安装。







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
# CUDA 9.0conda install pytorch==1.0.1 torchvision==0.2.2 cudatoolkit=9.0 -c pytorch# CUDA 10.0conda install pytorch==1.0.1 torchvision==0.2.2 cudatoolkit=10.0 -c pytorch# CPU Onlyconda install pytorch-cpu==1.0.1 torchvision-cpu==0.2.2 cpuonly -c pytorch
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
ssh-keygen -t rsa -C youremail@example.com
或者
ssh-keygen -t ed25519 -C youremail@example.com
#-t 表示用不一样的method加密
```

3. 到github上， 打开Account settings > SSH and GPG keys>点击Add SSH keys ， 将id_rsa.pub或者id_ed25519.pub的内容复制并贴在空格内， title可以取名比如xxx's mac 之类的



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

#### 过滤指定的文件不上传 使用.gitignore

在文件下新增.gitignore
输入需要过滤的文件or目录

```
test.pytest/*.log
```



#### 把untracked file 删除

删除未追踪的文件

```
git clean -f
```

删除未追踪的文件跟文件夹

```
git clean -df
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



查看本地链接到远程哪一个仓库地址

```
git remote show origin
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



#### fork 原作者仓库后 如何更新



#### 删除文件

cd到repo的文件夹后， 一般的删除本地以及远程文件的方式

```
git rm 文件名
```



如果只想删除远程而保留本地的方式

```
git rm -r --cached 文件名
```



#### 删除commit

参考https://www.cnblogs.com/lwcode6/p/11809973.html





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

#### 克隆特定的子文件夹
```
1. mkdir test
2. cd test
3. git init
4. git remote add origin http:xxxx.git
5. git config core.sparsecheckout true
6. echo "文件夹路径" >> .git/info/sparse-checkout
7. git pull origin master
```




#### 问题解决

**错误信息 ： error: failed to push some refs to**

问题在于远程版本比本地的还要新， 需要先pull回来做更新才能重新push

先进行更新, 然後合併

1. `git fetch `
2. `git merge `

然后在push即可



**错误信息 ： Changes not staged for commit:**

因為要提交的提交的档案尚未track， 需要对该档案 git add 档案名， 然后在重新执行commit, push等



**错误信息 ： Git修改密码后命令行push代码报“fatal: Authentication failed for **

```shell
git config --system --unset credential.helpergit config --global credential.helper store
```

然后在 git push 就会要求输入使用者的账号和密码



	
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

<h3 id="22">22. python pip 安装及管理 </h3>

#### pip 包管理升级

```
pip install --upgrade pip
```

如果是升级pip3 

```
pip3 install --upgrade pip
```

#### 换源加速 （清华或者阿里）

推荐使用阿里源， 速度飞快

1. 清华源

`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

2. 阿里源

```
pip config set global.index-url http://mirrors.aliyun.com/pypi/simple
```

在进行包的安装即可飞速

如果出现 找不到config 这个command ， 说明pip的版本太旧了， 利用`pip install --upgrade pip` 来升级

2. 也可以找到pip.conf这个文件用vim修改源

   Pip.conf这个文件通常在 /home/yourname/.config/pip/pip.conf



PS.如果有些比较不知名的包在清华源无法下载， 可以用

`pip install [module] -i https://pypi.org/simple`

临时将链接替换回默认镜像



#### 阿里镜像源加速

在install后面添加例如

```
pip install torch==1.5 -i http://mirrors.aliyun.com/pypi/simple --trusted-host mirrors.aliyun.com
```





#### ReadTimeoutError(“HTTPSConnectionPool）

遇到这样的问题通常就是国外源从国内访问太慢

建议可以直接下载包的whl 或者 tar.gz压缩包到本地进行安装

```
sudo pip3 install xxxxx.whl
```

或者是将压缩包解压缩后进行安装

```
tar -xvzf xxxxx.tar.gzcd xxxxxxsudo python3 setup.py install
```

即可完成安装



#### Ubuntu下 安装常用的包

##### matplotlib

```
sudo apt-get python3-matplotlib 
```











------

<h3 id="23">23. Jupyter notebook SSH 远程连接 </h3>

1. 首先确保本机与远程主机都已经安装且可以正常使用jupyter notebook
2. 待完成。。。

------

<h3 id="">24. Docker 安装及指令 </h3>

#### 安装

参考 https://segmentfault.com/a/1190000022374119

安装如下需要的包

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

利用deb的方式安装

进入右侧网址 https://download.docker.com/linux/ubuntu/dists/

进去之后点选Ubuntu版本， bionic 是ubuntu 18.04

然后pool/stable/amd64，选择docker 19.03的版本 （可支持GPU）

下载好之后安装

```
sudo dpkg -i dokcer-ce_19.03.9_3-0-ubuntu-bionic_amd64.deb
```

执行以下进行测试看安装成功了

```
sudo docker run hello-world
```





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

主要从官方的Docker hub pull镜像下来实在太慢, 所以需要其他厂商提供的加速镜像

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



2. Linux 添加加速器（亲测有效）

推荐用阿里云镜像加速

登入阿里云（用支付宝登入）-> 搜寻镜像服务器 -> 控制台->左侧找到镜像加速器， 会看到属于自己的镜像地址

```shell
cd /etc/docker
vim daemon.json

#添加下面内容, 文件必须符合json规范
{
	"registry-mirrors": [
	"这边贴上阿里镜像加速器的地址"
	]
}

#然后wq保存

```

reload一下docker

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

然后在pull images 就会发现速度提升非常多

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





#### 镜像images操作篇

`docker image ls` 查看目前本机有的images

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZEbvlc/caffe          cpu                 0b577b836386        18 months ago       1.64GB
```

`docker image ls -a` 可以看到中间层镜像， 也就是无标签镜像， 不需要删除

`docker image rm xxxxx/xxxxxx` : 删除不需要的镜像

`docker commit containerID newname` : 每次在容器中想要保存目前的内容，创建成新的镜像，  可以使用， newname可以自己定义

#### container  容器常用指令 

**查看容器container**

`docker ps -a`或是`docker container ls -a` : 可以查看到终止状态的容器，前者为旧的指令

`docker container ls` ：可以查看运行中的容器



**跳出运行中的容器**

`ctrl + D` : 可以跳出正在执行中的容器， 容器状态为EXIT终止， 但是并没有被删除



**重新进入跳出的容器** 

跳出容器之后， 容器会是exit的状态， 需要重新启动start

启动的方式

`docker start <container ID>` : 启动停止的容器

接着进入容器

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



#### 本机传输文件到容器

```
sudo docker cp 本地文件路径 容器ID:容器路径
```







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
sudo pip3 uninstall ipythonsudo pip3 install ipython
```

2. 重新强制安装prompt-toolkit

```
sudo pip3 install 'prompt-toolkit<2.1.0,>=2.0.0' --force-reinstall
```

3. 执行完毕后记得存储镜像

------

<h3 id="25">25. Shell 脚本语法与执行 </h3>

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
	comandfi
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

<h3 id="26">26. python 安装基于Linux </h3>

参考https://blog.csdn.net/baidu_37973494/article/details/88324236?utm_medium=distribute.pc_relevant.none-task-blog-baidulandingword-7&spm=1001.2101.3001.4242

```cbash
# wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz 
# tar -xzvf Python-3.7.2.tgz
# cd Python-3.7.2
# ./configure --enable-optimizations
# make
# make install
```



基于Ubuntu安装， 参考[这里](https://blog.csdn.net/u014775723/article/details/85213793?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

**常见问题**

如果安装遇到zlib 问题 可以

```shell
sudo apt-get install zlib*
```

接着可能遇到

```
ModuleNotFoundError: No module named '_ctypes'
make: *** [install] Error 1
```

可以

```
sudo apt-get install --reinstall zlibc zlib1g zlib1g-dev sudo apt-get install libffi-dev libssl-dev libreadline-dev -y
```

然后重新configure, 编译

------

<h3 id="27">27. onnx (Open Neural Network Exchange) 模型相关 </h3>

- Netron 
  - 在线导入onnx模型可视化 [lutzroeder](https://lutzroeder.github.io/netron/)
  - [lutzroeder github](https://github.com/lutzroeder/netron)
- onnx - tensorrt : 将onnx转换为tensorrt
  - [official onnx-tensorrt github](https://github.com/onnx/onnx-tensorrt)

------

<h3 id="">28. PytorchToCaffe </h3>

- [PytorchToCaffe](https://github.com/xxradon/PytorchToCaffe)



<h3 id="">29. TensorboardX使用</h3>

参考 https://stackoverflow.com/questions/37987839/how-can-i-run-tensorboard-on-a-remote-server

这边具体如何使用看官方文档

只教安装

```
pip3 install tensorboardX
pip3 install tensorflow-gpu
```

如果要查看, terminal 输入

```
tensorboard --logdir "这边输入记录的路径"
```

如果要在服务器上使用，在服务器上启动tensorboardX 并在本地显示的话

在本地用以下指令链接服务器

```shell
ssh -L 16006:127.0.0.1:6006 username@hot -p 端口
#(ssh -L 本地端口:本地IP:远程端口 远程服务器用户名@远程服务器ip -p 服务器端口)
```

然后在本地浏览器打开, 就能显示tensorboard界面

```
http://localhost:16006
```



------

<h2 id="" align="center"> Linux 程序安装 </h2>

#### 安装Ubuntu 教程

https://blog.csdn.net/baidu_36602427/article/details/86548203?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2

#### 换成清华源

安装完Ubuntu系统之后， 执行以下命令下载一个文件到本地

```shell
wget https://tuna.moe/oh-my-tuna/oh-my-tuna.py
```

下载完之后 python执行

接着pip 的源也默认变成清华的



#### 将apt源换成阿里源

如果不用清华源， 可以如下方式改成阿里

参考https://blog.csdn.net/u012308586/article/details/102953882

1. 备份原始源文件source.list

```shell
sudo  cp   /etc/apt/sources.list   /etc/apt/sources.list.bak
```

2. 修改源文件sources.list

```
sudo  chmod  777  /etc/apt/sources.list   #更改文件权限使其可编辑
sudo  vim  /etc/apt/sources.list #记得先安装vim
```

```shell
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```



3. 最后更新一下源

   ```shell
   sudo apt update
   ```

   



#### Ubuntu 安装chrome 

对于64位版本可以使用如下链接下载：

wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

然后 `sudo dpkg -i google-chrome-stable_current_amd64.deb`

就安装好了



#### Ubuntu 识别 移动硬盘 exFAT格式

刚装好的ubuntu必须按照插件才能识别移动硬盘

```
sudo apt-get install exfat-utils
```



#### 安装pip or pip3

```shell
sudo apt-get install python-pip

sudo apt-get install python3-pip
```





#### 安装openCV

先安装各种依赖

```shell
sudo apt-get install cmake libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff5-dev libdc1394-22-dev # 处理图像所需的包
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev liblapacke-dev

sudo apt-get install libxvidcore-dev libx264-dev # 处理视频所需的包
sudo apt-get install ffmpeg

sudo apt-get install libcanberra-gtk-module


```

clone github上的openCV 从源码编译安装

git clone https://github.com/opencv/opencv.git



接着键入以下指令， 记得在第五行的地方， 最后是库文件的安装位置， 也可以是usr/local 这是默认的位置

```shell
cd opencv //切换到目录下
mkdir build 
cd build //切换到该文件夹
//配置输出的参数
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/user/local ..
sudo make //编译， 这一步会花比较长的时间
sudo make install //安装
sudo ldconfig //更新动态库
```

最终如果cmakelist中find_package(OpenCV REQUIRED)找不到opencv时候

要以下主动设置路径

```
set(OpenCV_DIR /path/to/opencv-master/build) 
```



移除opencv的方法

去原先build的文件夹中， 执行sudo make uninstall, 就会自动删除系统路径下lib里面的库文件



问题1 ：

如果在cmake 的地方出现

```
ICV: Failed to download ICV package: ippicv_linux_20151201.tgz.
```

到链接: https://pan.baidu.com/s/1tUn4so6BZc8MdVz0FbtWLA 提取码: sktn 

下载ippicv_linux_20151201.tgz

并替换

`./opencv-3.2.0/3rdparty/ippicv/downloads/linux-808b791a6eac9ed78d32a7666804320e`下的文件



问题2:

因为cuda9 不支持opencv3.2 所以参考以下做法

https://blog.csdn.net/qq_39315153/article/details/103282762?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-1

#### 安装Cmake

[官网](https://cmake.org/download/)下载cmake-3.15.3.tar.gz

安装之前先安装

```
sudo apt-get isntall libssl-dev
```



```shell
tar -xvzf cmake-3.15.3.tar.gz
cd cmake-3.15.3
./bootstrap    #执行引导文件
#该命令执行需要一定时间，请耐心等待。成功执行结束之后，末尾提示:CMake has bootstrapped.  Now run make.
make
sudo make install
cmake --version
```



#### 通用

ld转载器

Linux 使用这个ld-linux.so*（虚拟机x86的ubuntu 是使用ld-linux.so2）中的来装载（其实这只是一个链接）其他库。所以这个库必须放在  linux中/lib下。对于其他，通常我们共享库放在/lib这个路径下，而且也是系统默认的搜索路径。

Linux共享库的搜索路径先后顺序：
1、编译目标代码时指定的动态库搜索路径：在编译的时候指定-Wl,-rpath=路径
2、环境变量LD_LIBRARY_PATH指定的动态库搜索路径
3、配置文件/etc/ld.so.conf中指定的动态库搜索路径
4、默认的动态库搜索路径/lib
5、默认的动态库搜索路径 /usr/lib



安装完新的库之后， 要让程序搜寻库之前可以先

```shell
vim /etc/ld.so.conf  #进入
/usr/local/lib #添加这行进去， 因为开源库安装后都会放到这个下面
sudo ldconfig -v  #进行一下更新
```



#### google protocol buffer 

1. 安装参考 ： https://www.codetd.com/article/5545049
2. 去official github https://github.com/protocolbuffers/protobuf 从release中找到要的版本下载source code版本， zip 或者tar档案
3. 注意安装3.8左右以上， 如果从源码安装， 需要额外下载googletest， 否则会报错
   1. 参考· https://www.twblogs.net/a/5c9bf5e2bd9eee73ef4b1238
   2. https://github.com/google/googletest/releases/tag/release-1.8.1
   3. 将下载好的googletest解压缩之后， 在丢进protobuf里面的thirdparty文件夹中， 并且重新将googletest..xxxx 命名为googletest就好
   4. `./autogen.sh` 重新开始接下来参考第一点安装参考继续就可以

PS. 注意3.0.0以上版本需要用autogen.sh, 不能直接./configure

卸载 ： 

```shell
sudo apt-get remove libprotobuf-dev

which protoc #找到路径
rm -rf /path/to/protoc #删除路径下的protoc
```











------

<h2 id="" align="center"> others </h2>

#### 查看版本

| 程序      | Command                                                |
| --------- | ------------------------------------------------------ |
| Ubuntu    | `lsb_release -a`  / `uname -a`  /  `cat /proc/version` |
| Cmake     | `cmake --version`                                      |
| openCV    | `pkg-config --modversion opencv`                       |
| TensorRT  | `dpkg -l grep TensorRT`                                |
| protobuf  | `protoc --version`                                     |
| g++ / gcc | `g++ --version`                                        |



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

如果想释放显存 如下步骤

1. 先找到占用GPU的进程

   ```
   fuser -v /dev/nvidia*
   ```

2. 然后找到以后 比如是PID 12345

   ```
   kill -9 12345
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
ffmpeg -ss 0:0 -t 0:10 -i input.mov -c copy output.mp4
```

- -ss : 表示视频开始时间
- -t : 表示持续时间
- -c : 表示复制出一份剪切好的视频

PS. 把参数ss -t 放在参数-i 之后， 输入文件会逐帧解码， 直到-ss设置的时间点位置， 虽然时间点是精准的但是容易出现黑屏

PS. 如果把-ss -t 放在-i之前， 会对输入文件执行seek, 直接找到-ss设置的时间点上， 不会出现黑屏



Ex. 裁剪视频最后10秒

```
ffmpeg -sseof -0:10 -i input.mov output.mp4
```

- -sseof ：表示视频最末尾的开始时间

  

  

Ex. 剪切视频中 6分15秒到7分23秒

```
ffmpeg -i xxx.mp4 -ss 06:15 -to 07:23 -strict -2 output.mp4
```

- strict -2 是为了防止报错`the encoder 'aac' is experimental but experimental codecs are not enabled`



#### 视频画面编辑

EX.画面缩放 1080p - 480p

```
ffmpeg -i input.mov -vf scale=854:480 -acodec aac -vcodec h264 out.mp4
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
ffmpeg -i twice_v2.mp4 -ss 00:00 -to 00:00 -q:v 1 -r 24 test/image-%05d.jpg
```

- -q : v * : 表示抽取的图片质量， * 为1~5, 1通常是最高质量
- -r 指定抽取的帧率，即从视频中每秒钟抽取图片的数量。1代表每秒抽取一帧，５就表示一秒抽5张图
- image-%05d.jpg ：表示存取的图像名称 05d表示00000的格式计数， 前面可以添加路劲
- -to : 表示抽帧的结束时间 跟-ss搭配



搭配`-start_number` 可以指定特定的开始数字， 比如设置



#### 隔N秒取帧

```
ffmpeg -i test.mp4 -q:v 1 -vf fps=25 test/image-%05d.jpg
```

- -vf fps ：  每秒取截取多少帧数





#### 视频合成

参考 https://blog.csdn.net/u011636440/article/details/78031734?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control

Ex.从抽出的帧 重新合成视频

```
ffmpeg -i path/to/%d.jpg -vcodec libx264 -r 帧率 output.mp4
```

因为ffmpeg 默认图片编号从0开始， 如果图片第一张不是0开始， 则可pyhton脚本修改或者是添加 

-start_number



#### 多视频合并

参考https://blog.csdn.net/u011086331/article/details/82966476

首先，把要合并的视频按顺序写到files.txt里，例如

```
file '1.mp4'
	
file '2.mp4'
```

注意必须单引号

然后

```
ffmpeg -f concat -safe 0 -i files.txt -c copy output.mp4
```

注意：这一行指令使用了-c copy，说明他只适用于视频切割产生的分段，被合并的视频必须是相同的参数！！如果你需要合并参数不同的视频，把-c copy去掉或者自己写压制参数，参考参数：-c:v libx264 -crf 23 -profile:v high -level 5 -c:a aac -b:a 240k



#### 設置幀率

Ex. 設置視頻的幀率

```
ffmpeg -i input.avi -codec:v mpeg4 -r 30 -qscale:v 2 -codec:a copy C.avi
```

- `-codec:v mpeg4` :  使用mpeg4的encoder
- `-qscale:v n` : 或者`-q:v n`表示输出的video的quality， n从1~31， 1为最高级品质， 建议2~5为mp4
- `-qscale:a  `: 或者 `-q:s ` 表示audio的quality， nn
- `-codec:a copy` : 複製



#### 调整视频比特率(码率) -b

可以用来压缩视频大小, 将视频压缩到约2500kbps左右

````
ffmpeg -i input.mp4 -b:v 2500k -b:a 2500k output.mp4
````





#### 视频流录制

```
ffmpeg -i rstp://user:password@ipaddress/Streams/Channels/101 -acodec copy -vcodec copy output/path/xxx.mp4
```



#### 视频转换格式

Dav2mp4 (通常只有大华摄像头才是dav格式)

```
ffmpeg -i video.dav -c:v libx264 -crf 24 output.mp4
```

- `-s` : 1280x720



#### 编码格式

H.264 : 



### Pdb debug 模式

```python
import pdb #import 这个包
在需要断点的地方 set_trace()
```

快捷键如下

`n`: next step 下一步

`c` : 执行下一个断点

`l` : 查看当前代码段

`step` : step in 进入

`q`: quit debug

`dir(类)` : 利用dir可以查看类的所有属性

------

# DeepLearning

#### 标注工具

Labellmg （https://github.com/tzutalin/labelImg）

Labelme (https://github.com/wkentaro/labelme#ubuntu)

#### 自定义数据集格式 //TODO

以下均以目标检测为例

VOC

```
VOC2007	|	Annotations (存放目标在图片里的坐标信息)	| ImageSets 				| Layout				|	Main	| JPEGImages (存放图片， 按照顺序)	| SegementationClass	| SegementationObjects
```





#### Segmentation 

多边形分割标注推荐使用labelme, labelme 自带将json文件转换成png



Labelme_json_to_dataset 可以处理单个文件， 编辑以下脚本可以批量处理多个json

在json目录下生成sh文件， 将以下内容复制进入， 然后保存执行就能批量产出png

```shell
#！/bin/bash
let i=1
path=./
cd ${path}
for file in *.json
do
     labelme_json_to_dataset ${file}
     let i=i+1
 done
```





#### CUDA / CUDNN 安装与移除

cuda版本需要与 Nvidia 显卡驱动匹配才能

参考 `docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html`



#### 显卡驱动安装

- 测试平台 Ubuntu 18.04.4

利用ppa 安装显卡驱动

```shell
sudo add-apt-repository ppa:graphics-drivers/ppa #添加入ppa
sudo apt-get update #更新一下
ubuntu-drivers devices #这会显示出哪些驱动可以安装

#比如看到
driver : nvidia-driver-440 
#
sudo apt install nvidia-driver-440
#如果出现软件包有未满足的依赖关系
sudo apt-get install libnvidia-compute-440

#然后重新再一次
sudo apt install nvidia-driver-440
#装完nvidia-smi测试一下， 出现以下属于正常， 重启电脑就行了
Failed to initilize NVML:Driver/library version mismatch




sudo ubuntu-drivers autoinstall
```

上述执行完之后, 

```
sudo reboot //重启电脑
nvidia-smi //查看是否安装成功， 如果有会出现熟悉的界面
```



#### CUDA安装

到官网 developer.nvidia.com/cuda-toolkit-archive

下载需要的版本，选择runfile格式

依照官方给的指令进行下载及安装

例如如下

```shell
wget http://developer.download.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.243_418.87.00_linux.run
sudo sh cuda_10.1.243_418.87.00_linux.run
```

过程会有一些协议需要accept

**然后记得显卡安装驱动的地方可以取消, 如果之前已经安装过的话**



如果报错提示空间不足

可以将tmpdir改成, 如下重新执行

```
sudo sh cuda_10.1.243_418.87.00_linux.run --tmpdir=/home
```



一般安装的路径位置会是在 /usr/local/cuda-10.x

然后会生成symbolic link 在/usr/local/cuda



然后在环境变量中设置路径

```bash
vim ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda


source ~/.bashrc //最后更新一下
```

以上这一步很重要如果没设置好， nvcc会找不到， 并且报错

```
bash : /usr/bin/nvcc: No such file or directory
```





#### CUDA共存版本

假设又想装一个cuda10.2 版本

一样记得驱动不要选择， 然后提示会跳出 已经有symbolic link, 就是原先已经有的/usr/local/cuda/

如果选择Y， 则会覆盖之前旧版本的cuda， 如果选择N， 就不会覆盖， 只会装到/usr/local/cuda-10.2

要看安装了哪些版本就到/usr/local 下

要查看现在使用那一个版本的cuda可以查看

```
stat cuda #查看当前符号链接的位置File： 'cuda' -> 'usr/local/cuda-10.1'Size : xxxx..etc
```

可以看到目前符号链接到 10.1的版本

如果想切换到 10.2 可以

```shell
sudo rm -rf cuda #删除之前的连接sudo In -s /usr/local/cuda-10.2 /usr/local/cuda # 也就是将10.2链接到cuda
```









#### cudnn 安装

到 Developer.nvidia.com/rdp/cudnn-download （应该需要登入）

找到对应cuda的版本， 下载 cuDNN Library for Linux 这会是一个tgz包

解压缩后得到cuda文件夹， 将里面的Include/ 以及 lib64/下的内容丢到/usr/local/cuda/下对应的文件夹下面就完成



#### CUDA 移除

1. 找到cuda自动的卸除脚本， 通常在`/usr/local/cuda/bin/`下面， 并且执行

```
sudo find / -iname '*uninstall_cuda*'
```

2. 直接删除/usr/local/下 cuda版本的文件夹就可以





如果使用sudo apt-get 安装的nvidia-cuda-toolkit

可以

```
sudo apt-get autoremove nvidia-cuda-toolkit
```





其他参考链接

https://www.jianshu.com/p/fd0f84f858f8





------

# IDE 使用技巧（Pycharm / VScode / Clion)



#### Pycharm SSH 远程链接

以下技巧pycharm clion都使用

1. 将pycharm IDE SSH连线到远程服务器段

- 服务器需要提供host ip地址，port端口号， host name(root)， 登入密码
  - 例如远程服务器的为 test@192.168.3.2， test为hostname, 192.168.3.2为host 地址
  - Pycharm setting中的project interpreter中可以新增服务器上的python解释器，找到SSH Interpreter地方， 输入以上服务器提供的资料进行连接



2. 实现资料互传

- Pycharm>tools > deployment>configuation 的mapping可以设置映射
  - Local path是本地项目的路径
  - Deplotment path是服务器的路径
  - Pycharm>tools > deployment>Brose remote host 可以访问远程的资料夹

3. 开启服务器的终端

   Pycharm > tools > start SSH session 可以在pycharm下方开启远程终端



更新远程解释器的包

```
File -> Invalidate Caches / Restart #可以删除移除之前的cache然后重启
```





**问题记录**

**问题**

1. Unable to save settings, restart Pycharm
2. Clion 直接open 文件会无法找到变量定义 cannot find declaration to go to

**解决**

1. 问题发生通常是因为对project文件没有权限， 所以将权限改变一下就行
2. 开启project需要用File->New CMake Project from Sources, 才会自动的找变量的定义



#### VScode

快捷技巧：

shift + Alt + 滚轮， 实现如pycharm多光标 下拉

##### 在vscode上编写 python

插件 ： Python 、 jupyter

1. SSH

   实现远程ssh, 从官方搜索ssh, 然后链接到本地下载Remote - SSH extension

   并且选择远程python解释器的时候相当简单（前提先安装好python extension)， 在最下方一条蓝色的地方可以选择python 解释器， 而且插件已经自动的帮你找到远程解释器的位置

   

2. 远程python解释器

   在服务器上装python extension ， 可以现在vscode中插件市场找到python, 然后右键copy 到浏览器贴上， 保留地址后面link的部分， 就能跳转到下载插件包的网站， 右侧找到download的地方把包下载下来丢到服务器，然后打开vscode 里面的终端 输入如下指令进行安装

   ```
   code --install-extension /path/to/extension.vsix
   ```

   **问题**

   如果遇到无法识别插件的问题，可以将主目录下的`.vscode-server`文件夹删除或者改名

   然后重新启动vscode让他自动建立一个新的`.vscode-server`

   

3. python extension 

   在extension 中搜索 python 并安装， 就能让IDE支持 快速查看函数定义等功能

4. 打开快捷键一览表

   ```
   ctrl K + ctrl S
   ```

   ##### 快捷

   ```
   Alt + up / down 移动上下行

   shift + Alt + up/down 可实现多行列选择
   ```

5. 安装智能补全插件 Kite

   ```
   Kite AutoComplete Al Code
   ```

   记得去Kite 官方下载engine, 搭配vscode 的插件一起使用

6. **python**带参数的debug模式

   Run -> open configurations , 主要将args添加进去， 以下为示例

   ```json
   {
       // Use IntelliSense to learn about possible attributes.
       // Hover to view descriptions of existing attributes.
       // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
       "version": "0.2.0",
       "configurations": [
           {
               "name": "Python: Current File",
               "type": "python",
               "request": "launch",
               "program": "${file}",
               "console": "integratedTerminal",
               "args": ["-n","***", "-u", "**", "-p", "**", "-s", "***", "-k", "**", "-g", "acadci_prod"],
           }
       ]
   }
   
   
   #### torch 分布式训练debug, 主要将launch替换为torch底下的
   {
       // Use IntelliSense to learn about possible attributes.
       // Hover to view descriptions of existing attributes.
       // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
       "version": "0.2.0",
       "configurations": [
           {
               "name": "Python: Current File",
               "type": "python",
               "request": "launch",
               "program": "/path/to/site-packages/torch/distributed/launch.py",
               "console": "integratedTerminal",
               "args": ["--nproc_per_node=4",
                       "/path/train.py",
                       ],
           }
       ]
   }
   ```



#### 在vscode 上写C++

插件：C++ 、 CMake、Cmake tools

修改`launch.json` configuration

以下为示例， 主要修改program 也就是程序运行的路径，然后args 为列表

详情参考 https://code.visualstudio.com/docs/cpp/launch-json-reference

```c++
{
  "name": "C++ Launch (Windows)",
  "type": "cppvsdbg",
  "request": "launch",
  "program": "C:\\app1\\Debug\\app1.exe",
  "symbolSearchPath": "C:\\Symbols;C:\\SymbolDir2",
  "args":["-a", "-b"],
  "externalConsole": true,
  "logging": {
    "moduleLoad": false,
    "trace": true
  },
  "visualizerFile": "${workspaceFolder}/my.natvis",
  "showDisplayString": true
}

```





#### Vscode 必装插件

1. Markdown All in one : github.com/yzhang-gh/vscode-markdown

2. Remote-SSH

3. Todo-Tree 可以实现在代码中进行各种标记， 方便快速跳转

   

   

#### 遇到问题

1. 如果从不同版本的vscode 登入remote server， 可能导致原先的extensions 无法正常加载， 解决方案是到.vscode_server/extensions/ms.python.python  ms.jupyter 两个rm -rf 删除， 然后重新装

 



------

<h2 id="" align="center"> Socket 网路编程 </h2>

#### 1. 实现服务端与客户端webcam 实时传输图像

参考 https://gist.github.com/kittinan/e7ecefddda5616eab2765fdb2affed1b

为实现两段进行connect， 需要知道host 以及 port端口

客户端例子如下

```python
import cv2
import io
import socket
import struct
import time
import pickle
import zlib

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('192.168.1.124', 8485))
connection = client_socket.makefile('wb')

cam = cv2.VideoCapture(0)

cam.set(3, 320);
cam.set(4, 240);

img_counter = 0

encode_param = [int(cv2.IMWRITE_JPEG_QUALITY), 90]

while True:
    ret, frame = cam.read()
    result, frame = cv2.imencode('.jpg', frame, encode_param)
#    data = zlib.compress(pickle.dumps(frame, 0))
    data = pickle.dumps(frame, 0)
    size = len(data)


    print("{}: {}".format(img_counter, size))
    client_socket.sendall(struct.pack(">L", size) + data)
    img_counter += 1

cam.release()
```



服务端例子如下

```python
import socket
import sys
import cv2
import pickle
import numpy as np
import struct ## new
import zlib

HOST=''
PORT=8485

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
print('Socket created')

s.bind((HOST,PORT))
print('Socket bind complete')
s.listen(10)
print('Socket now listening')

conn,addr=s.accept()

data = b""
payload_size = struct.calcsize(">L")
print("payload_size: {}".format(payload_size))
while True:
    while len(data) < payload_size:
        print("Recv: {}".format(len(data)))
        data += conn.recv(4096)

    print("Done Recv: {}".format(len(data)))
    packed_msg_size = data[:payload_size]
    data = data[payload_size:]
    msg_size = struct.unpack(">L", packed_msg_size)[0]
    print("msg_size: {}".format(msg_size))
    while len(data) < msg_size:
        data += conn.recv(4096)
    frame_data = data[:msg_size]
    data = data[msg_size:]

    frame=pickle.loads(frame_data, fix_imports=True, encoding="bytes")
    frame = cv2.imdecode(frame, cv2.IMREAD_COLOR)
    cv2.imshow('ImageWindow',frame)
    cv2.waitKey(1)
```



#### 2. 服务端与客户端之间信息通信

python3中必须先将要send 的信息encode, 然后接收信息的一段需要decode才能得到接收的信息

参考https://stackoverflow.com/questions/33003498/typeerror-a-bytes-like-object-is-required-not-str

```python
Client Side:
>>> host='127.0.0.1'
>>> port=1337
>>> import socket
>>> s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
>>> s.connect((host,port))
>>> st='connection done'
>>> byt=st.encode()
>>> s.send(byt)
15
>>>
Server Side:

>>> host=''
>>> port=1337
>>> import socket
>>> s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
>>> s.bind((host,port))
>>> s.listen(1)
>>> conn ,addr=s.accept()
>>> data=conn.recv(2000)
>>> data.decode()
'connection done'
>>>
```

如果要传送dict可以用json包的dump

```d

```

---

<h2 id="" align="center">使用nginx 本地实时推流rtmp </h2>

参考 : https://blog.csdn.net/kingroc/article/details/50839994

```
sudo apt-get update
sudo apt-get install libpcre3 libpcre3-dev
sudo apt-get install openssl libssl-dev
```

找到合适的版本 先下载nginx

```
nginx.org/download/
```

将nginx的rtmp模块clone来

``` 
git clone https://github.com/arut/nginx-rtmp-module
```

解压缩nginx， 然后

```
./configure --add-module=指定你rtmp包的位置
make
sudo make install
```



启动服务

```
cd /usr/local/nginx./sbin/nginx
```



配置conf, 将rtmp添加到， application 名称可以随意输入

```json
worker_processes  1;events {    worker_connections  1024;}rtmp {    server {         listen 1935;	chunk_size 4096;        	application vod {		play /opt/video/vod;	}	application live{ #第一处添加的直播字段		live on;	}    }}http {    include       mime.types;    default_type  application/octet-stream;    sendfile        on;    keepalive_timeout  65;    server {        listen       80;        server_name  localhost;        location / {            root   html;            index  index.html index.htm;        }        error_page   500 502 503 504  /50x.html;        location = /50x.html {            root   html;        }    }}
```



接着利用ffmpeg 将python处理过的每一帧数进行推流

```python
rtmp_url = 'rtmp://localhost:1935/live/test'ffmpeg_command = [  'ffmpeg',  '-y',  '-f', 'rawvideo',  '-vcodec', 'rawvideo',  '-pix_fmt', 'bgr24',  '-s', "{}x{}".format(img_width, img_height),  '-r', str(fps),  '-i', '-',  '-c:v', 'libx264',  '-pix_fmt', 'yuv420p',  '-tune', 'zerolatency',  '-f', 'flv',  rtmp_url]process = sp.Popen(ffmpeg_command, stdin=sp.PIPE)process.stdin.write(frame.string)
```



---

<h2 id="" align="center">redis-py 使用 </h2>

Redis 为open source 的key-value数据存储方案

1. https://redis.io/download 下载stable 版本

2. ```shell
   $ tar xzf redis-6.2.5.tar.gz
   $ cd redis-6.2.5
   $ sudo make
   $ cd src
   $ sudo make install 
   ```

安装完毕后可以测试启动

```shell
src/redis-server
```

也能使用内建的客户端进行测试

```shell
src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

我们可以在/usr/local/下建立redis目录存储必须的配置文件

```shell
mkdir -p /usr/local/redis/bin
mkdir -p /usr/local/redis/etc

#复制src 下的文件到etc
cp redis.conf /usr/local/redis/etc
cp mkreleasehdr.sh redis-benchmark redis-check-aof redis-check-dump redis-cli redis-sentinel /usr/local/redis/bin
```



遇到问题

1. redis failed opening .rdb for saving permission denied

解决：

主要是权限不够

1. vim 开启/usr/local/redis/etc ， 搜索dbfilename dump.rdb
2. 找到#Note that you must specify a directory here, not a file name

```
dir ./
```

就能找到指定文件夹， 然后chmod 777 开启最大权限就行

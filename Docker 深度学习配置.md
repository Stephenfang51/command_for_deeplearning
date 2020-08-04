#### 注意 ！ 本文档推荐使用typora 阅读

测试环境 ：

1. Docker 19.03版本
2. GPU 2080Ti





### 注意 ！1~7全部配置完在看 ！

启动容器的方式

```
sudo docker run --gpus all -itd -p 23:22 nv_cbt:latest
```

每次启动ssh需要重新开启, 这样才能远程连接

```
sudo /etc/init.d/ssh start
```

每次启动都要激活miniconda3

```
cd /root/miniconda3/bin
. ./activate
```



依照以下流程开始走

### 1. 采用nvidia官方镜像 nvidia/cuda-10.0:base

主要是让Docker支持GPU

现在本地安装**nvidia container tookit**

参考 https://github.com/NVIDIA/nvidia-docker

```
# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

然后启动

```shell
#### Test nvidia-smi with the latest official CUDA image
docker run --gpus all nvidia/cuda:10.0-base nvidia-smi
#会自动下载nvidia这个镜像


# Start a GPU enabled container on two GPUs
docker run --gpus 2 nvidia/cuda:10.0-base nvidia-smi

# Starting a GPU enabled container on specific GPUs
docker run --gpus '"device=1,2"' nvidia/cuda:10.0-base nvidia-smi
docker run --gpus '"device=UUID-ABCDEF,1"' nvidia/cuda:10.0-base nvidia-smi

# Specifying a capability (graphics, compute, ...) for my container
# Note this is rarely if ever used this way
docker run --gpus all,capabilities=utility nvidia/cuda:10.0-base nvidia-smi
```



上面指令会自动pull nvidia的镜像， `nvidia/cuda:10.0-base`

后续要启动的话， 

```
sudo docker run --gpus all -itd nvidia/cuda:10.0-base
```



接着以下开始配置镜像， 也就是安装自己需要的东西

### 2. 在容器内配置阿里 apt源

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

   

### 3. 安装杂七杂八gcc ..cmake等

```
apt-get gcc
apt-get g++
apt-get install automake autoconf libtool make
apt-get install cmake

apt-get install git

```



### 4. 安装python3.7 and pip3

```
#先按装
apt-get install zlib*
sudo apt-get install libffi-dev


wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz
tar -zxvf Python-3.7.1.tgz
cd Python-3.7.1
./configure --prefix=/usr/local/python3.7.1
make
make test
sudo make install

```

可执行文件会在/usr /local/python3.7.1/bin

```
PATH=$PATH:$HOME/bin:/usr/local/python3.7.1/bin

```



```shell
vim ~/.bashrc
添加 export PATH=$PATH:$HOME/bin:/usr/local/python3.7.1/bin
source ~/.bashrc

python3 #测试一下有没有进入python的交互环境

```

安装pip3

```
apt-get install python3-pip

```

安装好直接修改pip源, 替换成阿里源

```
pip config set global.index-url http://mirrors.aliyun.com/pypi/simple

```





### 5. 安装ssh， 为了让pycharm or 其他IDE可以连接到docker进行配置

```
apt-get install openssh-server
```

```
passwd root #设置密码为了让ssh远程登入的时候 要输入验证
#假设密码设置为xxx
xxx
xxx


```

```
vim /etc/ssh/sshd_config #进行修改

RSAAuthentication yes #启用 RSA 认证
PubkeyAuthentication yes #启用公钥私钥配对认证方式
AuthorizedKeysFile .ssh/authorized_keys #公钥文件路径（和上面生成的文件同）
PermitRootLogin yes #root能使用ssh登录

service ssh restart
sudo /etc/init.d/ssh start
完成以上几步后，本机地址是127.0.0.1

```



在container中安装好ssh之后， 下次启动的时候， 用如下

```
sudo docker run -itd -p 16022:22 cbt::latest

```

也就是设置端口号



然后如下两种方式远程连接到docker中

第一种 本地电脑连接本地docker如下

pycharm 连接的时候或者用ssh连接

```
ssh root@127.0.0.1 -p 16022

```

然后输入密码就能登入



第二种 外面电脑远程连接本地docker

在本机利用

```
ifconfig -a

```

查看enp3s0下的

Inet 地址是为何 比如是`192.168.1.8`



然后一样外部的电脑中的pycharm设置ssh 连线

```
SFTP
root@192.168.1.8 端口设置16022 
就能远程连线

```





2375端口

https://blog.csdn.net/qq_18881987/article/details/95441352?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase





#### 6. CUDA 安装

#### 安装CUDA

把cuda 跟 cudnn 安装压缩包放置到home下， 然后

```
sudo sh cuda_10.0.130_410.48_linux.run

//注意 ！ 如果遇到空间不足在用以下的方法
sudo sh cuda_10.0.130_410.48_linux.run --tmpdir=/home 

#会遇到如下问题
1. EULA? accept
2. driver？ no
3. cuda10.0 toolkit？ yes
4. localtion /usr/local/cuda-10.0 ? 直接回车
5. symbolic yes
6. install samples ? no

vim ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda


source ~/.bashrc //最后更新一下
nvcc -V #如果有效果就表示安装成功

```

#### 安装CUDNN

```
解压缩cu

得到cuda文件夹， 将里面的Include/ 以及 lib64/下的内容丢到/usr/local/cuda/下对应的文件夹下面就完成

```



#### 7. 安装conda

```d
wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod 777 Miniconda3-latest-Linux-x86_64.sh #给执行权限
./Miniconda3-latest-Linux-x86_64.sh

```

安装好之后 直接输入conda 是command not found， 需要激活一下

找到安装路径 

`/root/miniconda3/bin`

```
chmod 777 activate
. ./activate #注意有两个点 之间还有空格

```

激活之后， 前面会出现base , 这就表示已经进入的conda环境

```
(base) root@xxxxxxx:

```


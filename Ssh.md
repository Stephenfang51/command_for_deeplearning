SSH 学习


ssh-genkey 能够在～/.ssh/下建立 私有钥匙和公有钥匙

id_rsa (private), id_rsa.pub(public)



将本地产生的key复制到服务器上的 authorized_keys 文件夹下，服务器上的每一个user都有自己的ssh key
```
cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
以上command为一连串的， ssh到server， 然后mkdir .ssh文件夹，接着将key public的内容copy到authorized_keys文件中

也可以分开作业
比如手动复制key
```
cat ～/.ssh/id_rsa.pub #这时候会显示key，手动copy起来
```
然后到服务器上touch一个key文件，用vim或者nano贴上复制的key
然后在
```
ssh-add 将刚刚新增的key， 新增到 ssh-agent
```


新增私钥
```
ssh-add ~/.ssh/xxxxx
```
删除私钥
```
ssh-add -d ~/.ssh/xxxx
```


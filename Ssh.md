SSH 学习


ssh-genkey 能够在～/.ssh/下建立 私有钥匙和公有钥匙

id_rsa (private), id_rsa.pub(public)



将本地产生的key复制到服务器上的 authorized_keys 文件夹下
```
cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
以上command为一连串的， ssh到server， 然后mkdir .ssh文件夹，接着将key public的内容copy到authorized_keys文件中

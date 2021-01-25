### 前言
> 在我们日常使用git仓库是进行推送或者拉取时都时需要输入账号和密码，这样很繁琐，那有什么办法解决了：
>-  1.可以把账号和密码保存到本地，但是这种在多账号的情况下会不方便切换账号。
>- 2.可以在git服务器配置SSH公钥，使用SSH协议访问仓库。

## 1.生成单个SSH公钥
1.可以使用以下命令生成SSH公钥：
``` git
ssh-keygen -t rsa -C 'xxxxxxx@xxx.com'
# xxxxxxx@xxx.com 为你的邮箱帐号。
```

输入后按照提示完成三次回车，就可以生成ssh key。

可以通过查看```~/.ssh/coding_id_rsa.pub```文件内容，获取到公钥。**coding_id_rsa.pub** 为保存公钥的文件名。一般默认为 **id_rsa.pub**

输入以下命令查看公钥：
```
cat ~/.ssh/coding_id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc...
```


![image](https://github.com/tao111222333444/image/blob/master/GIT_SSH.png?raw=true)

2.然后就可以把查看的公钥添加到对应的git仓库中了。

添加完成后可以通过以下命令确认是否配置成功：
```
ssh -T git@xxxx.xxx
# 列如码云的  ssh -T git@gitee.com
```
首次连接时需要确认并添加主机到本机SSH可信列表。若返回通过认证，则证明添加成功。如下图

![](https://github.com/tao111222333444/image/blob/master/GIT_SSH_1.png?raw=true)

这就完成了Git SSH的配置了

## 2.配置多个SSH公钥
上面我们讲了如何生成和配置单个SSH公钥，但是我们很多时候都会有多个git账号，所以我们需要配置和生成多个SSH公钥。

1.不过在生成时我们需要设置SSH的文件名，如下：
```
 ssh-keygen -t rsa -C '******@qq.com' -f ~/.ssh/coding_t_id_rsa

```
这是把生产的SSH密钥文件命名为coding_t_id_rsa，其他的和单个配置一样

2.生成密钥后需要在~/.ssh（密钥的文件同一目录下）创建一个 config 文件进行配置 添加如下内容：(Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径)
```
# coding
Host e.coding.net
HostName e.coding.net
PreferredAuthentications publickey
IdentityFile ~/.ssh/coding_t_id_rsa
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_id_rsa
```
3. 配置完成后  可以通过如下命令进行测试：
```
$ ssh -T git@e.coding.net
$ ssh -T git@github.com
```
如果成功的话对应的服务器会返回对应的应答。
到这里就完成了多个SHH公钥的配置了
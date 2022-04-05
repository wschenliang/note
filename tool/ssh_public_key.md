## 说明：
- SSH是建立在应用层和传输层基础上的安全协议，其目的是专为远程登录会话和其他网络服务提供安全性的保障，用过SSH远程登录的人都比较熟悉，可以认为SSH是一种安全的Shell。
- SSH登录是需要用户名和密码的，要实现无密码登录，就需要创建SSH 密钥（ssh key），SSH 密钥可以认为是和另一台电脑通信时的唯一的识别证，SSH 密钥对可以让我们方便的登录到 SSH 服务器，而无需输入密码。

1.检查SSH密钥是否存在

输入下面命令
```shell
ls -l ~/.ssh
cd ~/.ssh
ls -l ~/
```
如果有文件id_rsa.pub 或 id_dsa.pub，则密钥存在。


2.生成新的ssh密钥

```shell
ssh-keygen -t rsa -C "your_email@example.com"
```

- 在git等源代码管理中，使用cat ~/.ssh/id_rsa.pub命令，打印并将相应内容复制到源代码管理服务器即可实现git的无密码管理。
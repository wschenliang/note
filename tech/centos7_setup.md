## 更换yum源为阿里源
```shell
# 进入到yum的源目录下
cd /etc/yum.repos.d/

# 将原来的CentOS-Base.repo进行备份
mv CentOS-Base.repo CentOS-Base.repo_back

# 下载替换阿里源
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# 生成缓存
yum makecache

# 更新
yum -y update
```

## 开放端口

```shell
systemctl start firewalld # 启动
systemctl status firewalld # 状态
systemctl disable firewalld # 停止
systemctl stop firewalld # 禁用

systemctl enable firewalld.service # 在开机时启用一个服务
systemctl disable firewalld.service # 在开机时禁用一个服务
systemctl is-enabled firewalld.service # 查看服务是否开机启动
systemctl list-unit-files|grep enabled # 查看已启动的服务列表
systemctl --failed # 查看启动失败的服务列表

firewall-cmd --zone=public --add-port=27017/tcp --permanent # 永久添加端口
firewall-cmd --reload # 刷新
firewall-cmd --zone=public --list-ports # 查看开放端口列表
firewall-cmd --zone= public --remove-port=80/tcp --permanent # 删除端口
```

## 网络设置

可以输入命令 "dhclient"，可以自动获取一个IP地址，再用命令"ip addr"查看IP

```shell
vim /etc/sysconfig/network-scripts/ifcfg-ens0
```
动态网络地址
```
BOOTPROTO=dhcp
DEVICE=eth0
HWADDR=52:54:00:b2:28:12
ONBOOT=yes
PERSISTENT_DHCLIENT=yes
TYPE=Ethernet
USERCTL=no
```
设置为静态：将ONBOOT=no改为yes，将BOOTPROTO=dhcp改为BOOTPROTO=static,并在后面增加几行内容：
IPADDR=192.168.127.128
NETMASK=255.255.255.0
GATEWAY=192.168.127.2
DNS1=119.29.29.29

静态地址模版
```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=eth0
UUID=f81f8b22-c87c-4e66-867d-da5fb05ac7af
DEVICE=eth0
ONBOOT=yes
IPADDR=10.0.101.23
PREFIX=24
GATEWAY=10.0.101.254
#NETMASK=255.255.255.0
DNS1=221.228.255.1
```

保存后退出，然后输入命令：
```shell
systemctl restart network.service
```
# ubuntu常用操作

## ubuntu相关指令
- sudo apt-get update 更新源
- sudo apt-get install package 安装包
- sudo apt-get remove package 删除包
- sudo apt-cache search package 搜索软件包
- sudo apt-cache show package 获取包的相关信息，如说明、⼤⼩、版本等
- sudo apt-get install package --reinstall 重新安装包
- sudo apt-get -f install 修复安装
- sudo apt-get remove package --purge 删除包，包括配置⽂件等
- sudo apt-get build-dep package 安装相关的编译环境
- sudo apt-get upgrade 更新已安装的包
- sudo apt-get dist-upgrade 升级系统
- sudo apt-cache depends package 了解使⽤该包依赖那些包
- sudo apt-cache rdepends package 查看该包被哪些包依赖
- sudo apt-get source package 下载该包的源代码
- sudo apt-get clean && sudo apt-get autoclean 清理⽆⽤的包
- sudo apt-get check 检查是否有损坏的依赖
- sudo apt-get install openssh-server 安装ssh
- sudo apt-get install net-tools 安装ifconfig
- sudo apt update 更新
- uname -a 查看系统版本
- free [-m/-g]查看内存占用 带上m是已Mb为单位情况 -g以Gb为单位
- df -hl 查看磁盘占用情况
- sudo dpkg -i package.deb 安装deb包命令


## ubuntu更换镜像源
> /etc/apt/sources.list

阿里源
```shell
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

清华源
```shell
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security multiverse
```
刷新软件信息
```shell
sudo apt update
# 更新软件
sudo apt upgrade
```

## ubuntu安装mysql：
### 卸载
1.删除mysql的数据文件
```shell
sudo rm /var/lib/mysql/ -R
```
2.删除mysql的配置文件
```shell
sudo rm /etc/mysql/ -R
```
3.卸载mysql（包括server和client）
```shell
sudo apt-get autoremove mysql* --purge
sudo apt-get remove apparmor 
```
4.然后在终端中查看MySQL的依赖项：
```shell
dpkg --list|grep mysql
```
### 安装mysql
1.安装：
```shell
sudo apt install mysql-server mysql-client
# 查看：
sudo systemctl status mysql
# 安全操作：
sudo mysql_secure_installation
# 安装依赖：
sudo apt install libmysqlclient-dev
```
2.登入
```shell
sudo mysql
mysql -u root -p 
# 或者进入/etc/mysql目录下，查看debian.cnf文件可以看到初始账号密码
Mysql -u Debian-sys-maint -p 123
```
3.修改密码
```sql
use mysql;
update user set authentication_string=PASSWORD("root") where user='root';
Alter user 'root'@'localhost' identified with mysql_native_password by 'root';
update user set plugin="mysql_native_password";
```
以上报错得话，按照以下方式执行
```sql
--添加个新的root主机是%
create user 'root'@'%' identified by 'root';
-- 报错：you are not allowed to create a user with grant 则我们把localhost改为%
Update user set host='%' where user = 'root';
Grant all on *.* to 'root'@'%'; --这句要执行两次才成功。
update user set authentication_string='' where user='root';
alter user 'root'@'%' identified with mysql_native_password by 'root';
grant all on *.* to 'root'@'%';
flush privileges;
```
4.设置远程访问：
```sql
Use mysql;
select host,user from user;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION
flush privileges;
quit
```
5.防火墙开放3306端口
```shell
iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```
6.修改MySQL监听
```shell
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
#注释掉bind-address = 127.0.0.1
```

### 重启动MySQL 
```shell
sudo /etc/init.d/mysql restart
# 或者
sudo service mysql restart
```

### 其他命令：
- 创建用户：CREATE USER 'username'@'host' IDENTIFIED BY 'password';
- 查看用户：SELECT User,Host FROM mysql.user;
- 用户授权：grant all on *.* to 'dimp'@'%' identified by '12345678';
- 查看密码策略SHOW VARIABLES LIKE 'validate_password%'
- 设置密码强度为lowset global validate_password_policy=LOW;

## ubuntu防火墙管理：
```shell
sudo ufw disable # 防火墙在系统启动时自动禁用 
sudo ufw status # 状态：不活动
~ sudo ufw enable # 在系统启动时启用和激活防火墙 
sudo ufw allow 81 # 规则已添加
sudo ufw deny 81 # 规则已更新
```

## ubuntu安装telnet:
1.安装openbsd-inetd 安装telnetd
```shell
sudo apt-get install openbsd-inetd -y
sudo apt-get install telnetd -y
```
2.重启openbsd-inetd
```shell
sudo /etc/init.d/openbsd-inetd restart
```
3.查看telnet运行状态
```shell
sudo netstat -a | grep telnet
```
4.登录
```shell
telnet 127.0.0.1  #（本机）
```

## ubuntu安装wireshark
```shell
sudo apt-get install wireshark
sudo usermod -aG wireshark 你的用户名
```




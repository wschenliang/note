Ubuntu使用方法：
调整窗口大小：设备—安装增强功能
安装ifconfig： sudo apt-get install net-tools
更新：sudo apt update
安装mysql：
删除mysql的数据文件sudo rm /var/lib/mysql/ -R
删除mysql的配置文件sudo rm /etc/mysql/ -R
卸载mysql（包括server和client）
sudo apt-get autoremove mysql* --purge
sudo apt-get remove apparmor 
然后在终端中查看MySQL的依赖项：dpkg --list|grep mysql
1、安装：sudo apt install mysql-server mysql-client
2、查看：sudo systemctl status mysql
安全操作：sudo mysql_secure_installation
3、安装依赖：sudo apt install libmysqlclient-dev
4、登入：sudo mysql  或者 mysql -u root -p 或者进入/etc/mysql目录下，查看debian.cnf文件可以看到初始账号密码
Mysql -u Debian-sys-maint -p …
5、use mysql;
6、修改密码
update user set authentication_string=PASSWORD("root") where user='root';
上面可能不适用mysql8
Alter user ‘root’@’localhost’ identified with mysql_native_password by ‘root’;
设置plugin字段
update user set plugin="mysql_native_password";
以上报错得话，按照以下方式执行
//添加个新的root主机是%
create user 'root'@'%' identified by 'root';
报错：you are not allowed to create a user with grant
则我们把localhost改为%
Update user set host=’%’ where user = ‘root’;
Grant all on *.* to ‘root’@’%’;这句要执行两次才成功。
update user set authentication_string='' where user='root';
alter user 'root'@'%' identified with mysql_native_password by 'root';
grant all on *.* to 'root'@'%';
flush privileges;
7、设置远程访问：
Use mysql；
select host,user from user;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION
flush privileges;
quit
8、防火墙开放3306端口iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
9、修改MySQL监听sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
10、#注释掉bind-address = 127.0.0.1
11、重启动MySQL 
sudo /etc/init.d/mysql restart  或者 sudo service mysql restart
其他命令：
创建用户：CREATE USER 'username'@'host' IDENTIFIED BY 'password';
查看用户：SELECT User,Host FROM mysql.user;
用户授权：grant all on *.* to 'dimp'@'%' identified by '12345678';
查看密码策略SHOW VARIABLES LIKE 'validate_password%'
设置密码强度为lowset global validate_password_policy=LOW;
防火墙管理：
sudo ufw disable 防火墙在系统启动时自动禁用 
sudo ufw status 状态：不活动
~ sudo ufw enable 在系统启动时启用和激活防火墙 
sudo ufw allow 81 规则已添加
sudo ufw deny 81 规则已更新
安装ssh:
sudo apt-get install openssh-server
安装telnet:
安装openbsd-inetd
sudo apt-get install openbsd-inetd -y
安装telnetd
sudo apt-get install telnetd -y
重启openbsd-inetd
sudo /etc/init.d/openbsd-inetd restart
查看telnet运行状态
sudo netstat -a | grep telnet
登录
telnet 127.0.0.1（本机）
安装wireshark
Sudo apt-get install wireshark
Sudo usermod -aG wireshark 你的用户名
安装deb包命令
sudo dpkg -i package.deb
查看系统版本: uname -a
查看内存占用:
free 
free -m 带上m是已Mb为单位情况
free -g以Gb为单位
查看磁盘占用情况: df -hl

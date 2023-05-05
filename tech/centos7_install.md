
## 环境安装 jdk/golang

```shell
vim /etc/profile

# 结尾处添加
export MONGODB_HOME=/opt/apps/mongodb
export ZOOKEEPER_HOME=/opt/apps/zookeeper/
export JAVA_HOME=/env/jdk-11.0.11
export GO_HOME=/env/go
export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin:$GO_HOME/bin:$MONGODB_HOME/bin:$ZOOKEEPER_HOME/bin

source /etc/profile
```

## mysql

卸载系统自带的Mariadb
```shell
rpm -qa | grep mariadb
# mariadb-libs-5.5.44-2.el7.centos.x86_64

rpm -e --nodeps mariadb-libs-5.5.44-2.el7.centos.x86_64
```

下载4个mysql安装包
- mysql-community-common-5.7.40-1.el7.x86_64.rpm 1
- mysql-community-libs-5.7.40-1.el7.x86_64.rpm 2
- mysql-community-client-5.7.40-1.el7.x86_64.rpm 3
- mysql-community-server-5.7.40-1.el7.x86_64.rpm 4

并且按照标号顺序进行安装，安装命令：
```shell
rpm -ivh ...
```

另外一种方式：
```shell
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

# 本地安装yum源
yum localinstall mysql

yum search mysql

yum install mysql-community-server.x86_64

# 查看是否安装成功
ps -ef | grep mysql
```

启动mysql

```shell
service mysqld start

service mysqld restart

service mysqld stop
```
查找默认登录密码
```shell
cat /var/log/mysqld.log | grep password

# 密码直接复制过来即可
mysql -uroot -p密码
```
更改密码和远程
```mysql
# 修改密码要设置复杂一点
SET PASSWORD = PASSWORD(' xxxx ');

use mysql;

select host, user from user;

update user set host="%" where Host='localhost' and user = "root";

# 更新权限
flush privileges;
```
重启mysql
```shell
service mysqld restart
```

## redis

```shell
# 下载fedora的 epel 仓库
yum install epel-release

# 安装 redis
yum install redis

# 启动redis
service redis start
# 停止redis
service redis stop
# 查看redis运行状态
service redis status
# 查看redis进程
ps -ef | grep redis

# 设置 redis 开启启动
chkconfig redis on

# 开启6379
/sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT
# 开启6380
/sbin/iptables -I INPUT -p tcp --dport 6380 -j ACCEPT
# 保存
/etc/rc.d/init.d/iptables save
# centos 7下执行
service iptables save

# 打开配置文件
vi /etc/redis.conf
# 修改默认密码，查找 requirepass foobared 将 foobared 修改为你的密码

# 使用配置文件启动
redis-server /etc/redis.conf &
```

## zookeeper

zookeeper官方网站https://downloads.apache.org/zookeeper/

```shell
# 防火墙开放 端口
firewall-cmd --permanent --zone=public --add-port=2181/tcp

cd /opt
wget https://downloads.apache.org/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz

tar -zxvf  apache-zookeeper-3.6.2-bin.tar.gz

mv apache-zookeeper-3.6.2-bin /usr/local/zookeeper-3.6.2

# 创建文件夹
cd zookeeper-3.6.2/
mkdir data
mkdir logs

# 修改配置文件
cd conf/
cp zoo_sample.cfg zoo.cfg
vim zoo.cfg

```
zoo.cfg内容
```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/opt/apps/zookeeper/data
dataLogDir=/opt/apps/zookeeper/logs
admin.serverPort=8888
clientPort=2181
```

启动
```shell
./zkServer.sh start
```

## mongodb

官网下载地址：https://www.mongodb.com/try/download/community

```shell
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-4.0.27.tgz

tar -zxvf mongodb-linux-x86_64-rhel70-4.0.27.tgz

mv mongodb-linux-x86_64-rhel70-4.0.27 /usr/local/mongodb

# 进入目录
cd /usr/local/mongodb/

# 创建三个文件夹
mkdir data data/db data/log

# 设置可读写权限
sudo chmod 666 data/db data/log/
```

mongodb 目录下新建配置文件 mongodb.conf
```
# 数据库数据存放目录
dbpath=/usr/local/mongodb/data/db
# 日志文件存放目录
logpath=/usr/local/mongodb/data/log/mongodb.log
# 日志追加方式
logappend=true
# 端口
port=27017
# 是否认证
auth=true
# 以守护进程方式在后台运行
fork=true
# 远程连接要指定ip，否则无法连接；0.0.0.0代表不限制ip访问
bind_ip=0.0.0.0
```
配置环境变量，使用 sudo vi /etc/profile 命令打开系统文件
启动和关闭 mongo
```shell
mongod -f /usr/local/mongodb/mongodb.conf 

# 查看服务进程
ps aux | grep mongo
netstat -lanp | grep 27017

mongod -f /usr/local/mongodb/mongodb.conf  --shutdown
```

开机自启动
```shell
vi /lib/systemd/system/mongodb.service
```
```
Unit]
    Description=mongodb
    After=network.target remote-fs.target nss-lookup.target
[Service]
    Type=forking
    ExecStart=/usr/local/mongodb/bin/mongod -f /usr/local/mongodb/mongodb.conf
    ExecReload=/bin/kill -s HUP $MAINPID
    ExecStop=/usr/local/mongodb/bin/mongod -f /usr/local/mongodb/mongodb.conf --shutdown
    PrivateTmp=true
[Install]
    WantedBy=multi-user.target
```

```shell
systemctl start mongodb.service
systemctl status mongodb.service
systemctl enable mongodb.service
systemctl daemon-reload
systemctl stop mongodb.service
```
启动 MongoDB 服务默认是没有账号密码的，即连接上即可进行各种操作。

但是我们在启动配置文件中，指定了 auth=true，即开启了认证，所以链接后需要认证才能执行操作。默认情况下，MongoDB 是没有管理员账户的，所以我们需要在 admin 数据库中使用 db.createUser() 命令添加管理员帐号或其他角色。

切换到 admin 数据库，使用以下命令创建管理账号，拥有操作所有数据库权限。

```
> use admin
switched to db admin
> db.createUser({user:"admin",pwd:"123456",roles:[{role:"userAdminAnyDatabase",db:"admin"}]})
Successfully added user: {
        "user" : "admin",
        "roles" : [
                {
                        "role" : "userAdminAnyDatabase",
                        "db" : "admin"
                }
        ]
}
> 

使用 mongo 命令连接上之后，如果不进行 db.auth("用户名","密码") 进行用户验证的话，是执行不了任务命令的，只有通过认证才可以。
注意，每一个用户都需要在创建这个用户的认证库下进行认证。

> use admin
switched to db admin
> db.auth("admin","123456")
1
> show tables
system.users
system.version
> 

以下演示创建 chenpi 用户，密码为123456，并设置对 nobody 数据库读写的权限。
注意，创建新用户前，先使用 admin 用户登录，因为我们刚才为 admin 用户设置了 userAdminAnyDatabase 权限。
# 先使用有创建用户权限的用户登录
> use admin
switched to db admin
> db.auth("admin","123456")
1
# 新用户的认证库
> use nobody
switched to db nobody
# 创建chenpi用户，密码123465，对nobody数据库有读写权限
> db.createUser({user:'chenpi',pwd:'123456',roles:[{role:'readWrite',db:'nobody'}]})
Successfully added user: {
        "user" : "chenpi",
        "roles" : [
                {
                        "role" : "readWrite",
                        "db" : "nobody"
                }
        ]
}
> 
```

```shell
/opt/apps/mongodb/bin/mongod -f /opt/apps/mongodb/mongodb.conf

mongod -config /opt/apps/mongodb/rs/conf/node1/mongodb.cfg
mongod -config /opt/apps/mongodb/rs/conf/node1/mongodb.cfg --replSet=rs0

mongo --host 127.0.0.1 --port 27017 -u cc -p cc --authenticationDatabase cmdb
```

## es

```shell
cd /home/software
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.2-linux-x86_64.tar.gz
tar -zxvf elasticsearch-7.6.2-linux-x86_64.tar.gz
```
笔者的 Elasticsearch 最终安装路径为：/home/software/data/elasticsearch-7.6.2。

注意 Elasticsearch7.6 已经内置了JDK，所以机器不需要本地 JDK 环境支持，如果服务器有本地 JDK ，尽量保持和 Elasticsearch 版本匹配的 JDK 版本。

如果非的使用 root 用户启动，当然也是可以的，但是只能针对 Elasticsearch5 之前的版本。

```shell
./elasticsearch -Des.insecure.allow.root=true
```
Elasticsearch5 之后，Elasticsearch 官方明确禁止用 root 用户启动 Elasticsearch 了，所以我们需要单独为 Elasticsearch 建立系统用户。
```shell
#创建用户
adduser espuxin
passwd 123456

#切换 Elasticsearch 用户 espuxin
su espuxin

#启动 Elasticsearch
./elastcsearch
```
Elasticsearch 的账户权限不足，所以在启动 ElasticSearch 之前，需要给 espuxin 用户赋予对应的执行权限。
```shell
chown -R esadmin:esadmin /home/software/data/elasticsearch-7.6.2

vi /home/esadmin/es/elasticsearch-7.6.2/config/elasticsearch.yml
# network.host: 0.0.0.0
```
切换到 root用户，在 /etc/sysctl.conf 文件添加 vm.max_map_count 参数值为 262144
```shell
su root
echo "vm.max_map_count=262144" > /etc/sysctl.conf
# sysctl -p 查看是否生效

vi /home/software/data/elasticsearch-7.6.2/config/elasticsearch.yml
```
把 cluster.name、node.name、initial_master_nodes 参数设置默认值。
```
cluster.name: wooola-es
node.name: node-1
cluster.initial_master_nodes: ["node-1"]
```

```shell
# ./elasticsearch: Permission denied
# 需要授权执行命令:
chmod +x bin/elasticsearch
```

es看lience
```shell
curl -ues -XGET "http://localhost:9200/_xpack?pretty"
```

## rabbitmq

官网地址 https://www.rabbitmq.com/download.html
- RabbitMQ(3.8.8): https://github.com/rabbitmq/rabbitmq-server/releases/tag/v3.8.8
- erlang(22.3)：https://www.erlang-solutions.com/downloads/

```shell
# 安装erlang
rpm -ivh esl-erlang_22.3.1-1_centos_7_amd64.rpm
```
注意：安装时出现这个错误

warning: esl-erlang_22.3.1-1_centos_7_amd64.rpm: Header V4 RSA/SHA1 Signature, key ID a14f4fca: NOKEY
error: Failed dependencies:
执行以下命令：
```shell
sudo yum install epel-release
sudo yum install unixODBC unixODBC-devel wxBase wxGTK SDL wxGTK-gl

yum install socat -y

#安装RabbitMQ
rpm -ivh rabbitmq-server-3.8.8-1.el7.noarch.rpm

#添加开机启动 RabbitMQ 服务
chkconfig rabbitmq-server on

#启动服务
/sbin/service rabbitmq-server start

#查看服务状态
/sbin/service rabbitmq-server status

#停止服务(选择执行)
/sbin/service rabbitmq-server stop

#开启 web 管理插件(执行这个，需要先关闭mq服务)
rabbitmq-plugins enable rabbitmq_management

```

用默认账号密码(guest)访问地址 http://192.168.56.10:15672/出现权限问题
解决方式：使用guest登陆（不推荐，建议采用添加一个新用户）

rabbitMQ默认的安装目录：/usr/lib/rabbitmq/lib/rabbitmq_server-3.8.8/sbin

```
#进入安装目录
[root@OY software]# cd /usr/lib/rabbitmq/lib/rabbitmq_server-3.8.8/sbin

#该目录下面的文件
[root@OY sbin]# ll
total 40
-rwxr-xr-x 1 root root 1245 Sep  4  2020 rabbitmqctl
-rwxr-xr-x 1 root root 1027 Aug 21 15:56 rabbitmq-defaults
-rwxr-xr-x 1 root root 1254 Sep  4  2020 rabbitmq-diagnostics
-rwxr-xr-x 1 root root 6948 Sep  4  2020 rabbitmq-env
-rwxr-xr-x 1 root root 1250 Sep  4  2020 rabbitmq-plugins
-rwxr-xr-x 1 root root 1249 Sep  4  2020 rabbitmq-queues
-rwxr-xr-x 1 root root 7042 Sep  4  2020 rabbitmq-server
-rwxr-xr-x 1 root root 1250 Sep  4  2020 rabbitmq-upgrade
```

```shell
#创建账号
rabbitmqctl add_user admin 123

#设置用户角色
rabbitmqctl set_user_tags admin administrator

#设置用户权限
set_permissions [-p <vhostpath>] <user> <conf> <write> <read>
rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"
#户 user_admin 具有/vhost1 这个 virtual host 中所有资源的配置、写、读权限

#当前用户和角色
rabbitmqctl list_users  
```

使用admin登入

```shell
#关闭应用的命令为
rabbitmqctl stop_app
#清除的命令为
rabbitmqctl reset
#重新启动命令为
rabbitmqctl start_app  
```


## prometheus

```shell
# install prometheus
mkdir -p /opt/prometheus
wget https://s3-gz01.didistatic.com/n9e-pub/prome/prometheus-2.28.0.linux-amd64.tar.gz -O prometheus-2.28.0.linux-amd64.tar.gz
tar xf prometheus-2.28.0.linux-amd64.tar.gz
cp -far prometheus-2.28.0.linux-amd64/*  /opt/prometheus/

# service 
cat <<EOF >/etc/systemd/system/prometheus.service
[Unit]
Description="prometheus"
Documentation=https://prometheus.io/
After=network.target

[Service]
Type=simple

ExecStart=/opt/prometheus/prometheus  --config.file=/opt/prometheus/prometheus.yml --storage.tsdb.path=/opt/prometheus/data --web.enable-lifecycle --enable-feature=remote-write-receiver --query.lookback-delta=2m 

Restart=on-failure
SuccessExitStatus=0
LimitNOFILE=65536
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=prometheus


[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable prometheus
systemctl restart prometheus
systemctl status prometheus
```

## node_exporter

参考https://www.prometheus.wang/quickstart/use-node-exporter.html

```shell
curl -OL https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz
tar -xzf node_exporter-0.17.0.linux-amd64.tar.gz
cd node_exporter-0.17.0.linux-amd64/
mv node_exporter /usr/local/bin/

```
部署脚本
```shell
cat >> /etc/rc.d/init.d/node_exporter <<EOF
#!/bin/bash
#
# /etc/rc.d/init.d/node_exporter
#
#  Prometheus node exporter
#
#  description: Prometheus node exporter
#  processname: node_exporter

# Source function library.
. /etc/rc.d/init.d/functions

PROGNAME=node_exporter
PROG=/opt/prometheus/$PROGNAME
USER=root
LOGFILE=/var/log/prometheus.log
LOCKFILE=/var/run/$PROGNAME.pid

start() {
    echo -n "Starting $PROGNAME: "
    cd /opt/prometheus/
    daemon --user $USER --pidfile="$LOCKFILE" "$PROG &>$LOGFILE &"
    echo $(pidofproc $PROGNAME) >$LOCKFILE
    echo
}

stop() {
    echo -n "Shutting down $PROGNAME: "
    killproc $PROGNAME
    rm -f $LOCKFILE
    echo
}


case "$1" in
    start)
    start
    ;;
    stop)
    stop
    ;;
    status)
    status $PROGNAME
    ;;
    restart)
    stop
    start
    ;;
    reload)
    echo "Sending SIGHUP to $PROGNAME"
    kill -SIGHUP $(pidofproc $PROGNAME)#!/bin/bash
    ;;
    *)
        echo "Usage: service node_exporter {start|stop|status|reload|restart}"
        exit 1
    ;;
esac
EOF
```

运行node exporter
```shell
service node_exporter start
```
启动成功后，查看端口
```shell
netstat -anplt|grep 9100
```
访问http://localhost:9100/

编辑prometheus.yml并在scrape_configs节点下添加以下内容:
```
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  # 采集node exporter监控数据
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```
重新启动Prometheus Server


## nginx

```shell
whereis nginx

yum install -y nginx

nginx -v

nginx # 启动
nginx -s stop # 停止
nginx -s reload # 重启 
```
此时/etc/nginx则是nginx配置文件存放位置
进入 目录查看 nginx.conf

```
user  root;
worker_processes  auto;
error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```
进入conf.d
```shell
vim default.conf
```
```
server {
    listen       80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

```shell
vim n9e.conf
```
```
server {
    listen       8765;
    server_name  _;

    #add_header Access-Control-Allow-Origin *;
    #add_header 'Access-Control-Allow-Credentials' 'true';
    #add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    #root   /root/n9e/pub;

    location / {

        add_header Access-Control-Allow-Origin *;
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';

        root /root/n9e/pub;
        try_files $uri /index.html;
    }
   location /api/ {
        proxy_pass http://127.0.0.1:18000;
    }
}
```

离线安装nginx教程

检查gcc,g++是否安装
```shell
gcc --version
g++ --version
```
有网的条件
安装gcc g++
```shell
yum -y install gcc
yum -y install gcc-c++
```

离线安装：gcc环境安装包：gcc离线安装包
- nginx-1.17.2.tar.gz
- openssl-1.1.1b.tar.gz
- pcre-8.43.tar.gz
- zlib-1.2.11.tar.gz

完成之后 我们就可以一下用下面的命令运行一下运行所有的rpm
```shell
rpm -ivh *.rpm --nodeps --force
```
nginx安装所需的文件 地址：nginx安装文件
下好文件之后，统一上传到/opt/nginx(没有这个目录，新建)

pcre安装
执行如下命令：
```shell
tar -zxvf pcre-8.44.tar.gz
cd pcre-8.44/
./configure
make
make install
```
zlib安装
执行如下命令：
```shell
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11/
./configure
make
make install
```
openssl安装
执行如下命令：
```shell
tar -zxvf openssl-1.1.1g.tar.gz
cd openssl-1.1.1g/
./config
make
make install
```

nginx安装
执行如下命令：
```shell
tar -zxvf nginx-1.14.0.tar.gz
cd nginx-1.14.0/
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-pcre=…/pcre-8.44 --with-zlib=…/zlib-1.2.11 --with-openssl=…/openssl-1.1.1g
make&&make install
```

nginx启动
```shell
cd /usr/local/nginx/sbin
./nginx
./nginx -s reload
./nginx -s stop
```


## jenkins

```shell
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```
如果您之前从 Jenkins 导入了密钥，则会失败，因为 您已经有钥匙了。请忽略这一点并继续前进。rpm --import
```shell
yum install fontconfig java-11-openjdk
yum install jenkins
```

rpm 软件包是使用以下密钥签名的：
pub   rsa4096 2020-03-30 [SC] [expires: 2023-03-30]
62A9756BFD780C377CF24BA8FCEF32E745F2C3D5
uid                      Jenkins Project
sub   rsa4096 2020-03-30 [E] [expires: 2023-03-30]
您需要从发行版中显式安装受支持的 Java 运行时环境 （JRE）

cd2de7b802324fc0904cd8b3bc0db235

## 安装包下载

链接: https://pan.baidu.com/s/1riuzjWOPYrQRwUaGyi6-xg?pwd=8023 提取码: 8023
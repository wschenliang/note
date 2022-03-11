# 系统性能监控 psutil
`
pip3 install psutil
import psutil
psutil.users() 获取⽤户的信息
psutil.boot_time() # 获得开机时间（LINUX格式返回）
datetime.datetime.fromtimestamp(psutil.boot_time()).strftime("%Y-%m-%d %H:%M:%S")
'2017-06-30 17:21:04'
`

# 自动发邮件yagmail
pip3 install yagmail

# 安装虚拟环境
sudo pip install virtualenv
sudo pip install virtualenvwrapper

*安装完虚拟环境后，如果提示找不到mkvirtualenv命令，须配置环境变量：*
`
mkdir
$HOME/.virtualenvs
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
source ~/.bashrc
`


创建虚拟环境
提示：如果不指定python版本，默认安装的是python2的虚拟环境
在python2中，创建虚拟环境：mkvirtualenv 虚拟环境名称
例：mkvirtualenv py_flask
在python3中，创建虚拟环境：mkvirtualenv -p python3 虚拟环境名称
例：mkvirtualenv -p python3 py3_flask
注意：
1、创建虚拟环境需要联⽹
2、创建成功后, 会⾃动⼯作在这个虚拟环境上
3、⼯作在虚拟环境上, 提示符最前⾯会出现 “虚拟环境名称”

使⽤虚拟环境
查看虚拟环境命令：workon + 两次tab  或者  lsvirtualenv
启动/切换虚拟环境：workon + 虚拟环境名
退出虚拟环境：deactivate
删除虚拟环境：rmvirtualenv + 环境名
先退出，再删除

在虚拟环境中安装⼯具包
提示 : ⼯具包安装的位置 :
python2版本下：~/.virtualenvs/py_flask/lib/python2.7/site-packages/
python3版本下：~/.virtualenvs/py3_flask/lib/python3.5/site-packages
安装flask-0.10.1的包:pip install flask==0.10.1
查看虚拟环境中安装的包:pip freeze


# ⽹络通信
## 常见端口：
FTP 21, SSH 22, TELNET 23, SMTP 25, DNS 53, HTTP 80, POP3 110, HTTPS 443
netstat －an查看端⼝状态
netstat -ntl （可以查看服务器socket）
sudo lsof -i : 端口号  可以查看端口号被哪个程序占用

## 创建socket
socket即是⼀种特殊的⽂件，⼀些socket类就是对其进⾏的操作（读/写IO、打开、关闭）
import socket
socket.socket(AddressFamily, Type)
AddressFamily(地址簇):socket.AF_INET IPv4（默认）, socket.AF_INET6 IPv6, socket.AF_UNIX
Type(类型):socket.SOCK_STREAM 流式socket for TCP（默认）, socket.SOCK_DGRAM 数据报式socket for UDP

'''
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # 创建tcp的套接字
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # 创建udp的套接字
s.close() # 不⽤的时候，关闭套接字
'''

##socket对象方法
###服务器端套接字：
- s.bind() # 绑定地址（host,port）到套接字， 在AF_INET下,以元组（host,port）的形式表示地址。
- s.listen() # 开始TCP监听。backlog指定在拒绝连接之前，操作系统可以挂起的最⼤连接数量。该值>=1，⼤部分应⽤程序设为5。
- s.accept() # 被动接受TCP客户端连接,(阻塞式)等待连接的到来
###客户端套接字:
- s.connect() # 主动初始化TCP服务器连接。⼀般address的格式为元组（hostname,port）
- s.connect_ex() # connect()函数的扩展版本,出错时返回出错码，不抛出异常。
###公共⽤途的套接字函数：
- s.recv() # 接收TCP数据，数据以字符串形式返回，bufsize指定要接收的最⼤数据量。flag提供有关消息的其他信息，通常可以忽略。
- s.send() # 发送TCP数据，将string中的数据发送到连接的套接字。
- s.sendall() # 完整发送TCP数据。将string中的数据发送到连接的套接字，但在返回之前会尝试发送所有数据。成功返回None，失败则抛出异常。
- s.recvfrom() # 接收UDP数据，与recv()类似，但返回值是（data,address）
- s.sendto() # 发送UDP数据，将数据发送到套接字，address是形式为（ipaddr，port）的元组，指定远程地址。
- s.close() # 关闭套接字
- s.getpeername() # 返回连接套接字的远程地址。返回值通常是元组（ipaddr,port）
- s.getsockname() # 返回套接字⾃⼰的地址。通常是⼀个元组(ipaddr,port)
- s.setsockopt(level,optname,value) # 设置给定套接字选项的值。
- s.getsockopt(level,optname[.buflen]) # 返回套接字选项的值。
- s.settimeout(timeout) # 设置套接字操作的超时期，timeout是⼀个浮点数，单位秒。
- s.gettimeout() # 返回当前超时期的值，单位是秒，如果没有设置超时期，则返回None。
- s.fileno() # 返回套接字的⽂件描述符。
- s.setblocking(flag) # 如果flag为0，则将套接字设为⾮阻塞模式，否则将套接字设为阻塞模式（默认值）。⾮阻塞模式下，如果调⽤recv()没有发现任何数据，或send()调⽤⽆法⽴即发送数据，那么将引起socket.error异常。
- s.makefile() # 创建⼀个与该套接字相关连的⽂件


# udp⽹络程序

## 发送数据:
1.socket.socket —— 建⽴套接字
udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
2. socket.sendto —— 发送数据
udp_socket.sendto("你好".encode(), ("127.0.0.1", 8888))
3. socket.close —— 关闭套接字
udp_socket.close()

##接受数据:
socket.recvfrom(缓冲区⼤⼩) —— 接受UDP套接字的数据
socket.recvfrom(1024) 使⽤udp⽅式接收数据，每次接收1024个字节
decode() —— 解码，把⼆进制数据解码为 字符串 类型
str = data.decode() 把data 解码为字符串并且保存到 str变量中

from socket import *
1. 创建udp套接字
udp_socket = socket(AF_INET, SOCK_DGRAM)
2. 准备接收⽅的地址
dest_addr = ('192.168.236.129', 8080)
3. 发送数据到指定的电脑上
udp_socket.sendto(send_data.encode('utf-8'), dest_addr)
4. 等待接收对⽅发送的数据
recv_data = udp_socket.recvfrom(1024) # 1024表示本次接收的最⼤字节数
5. 显示对⽅发送的数据
接收到的数据recv_data是⼀个元组,第1个元素是对⽅发送的数据,第2个元素是对⽅的ip和端⼝
print(recv_data[0].decode('gbk'))
print(recv_data[1])
6. 关闭套接字
udp_socket.close()
   
##UDP绑定端口
⼀个udp⽹络程序，可以不绑定，此时操作系统会随机进⾏分配⼀个端⼝，如果重新运⾏此程序端⼝可能会发⽣变化
也可以绑定信息（ip地址，端⼝号），如果绑定成功，那么操作系统⽤这个端⼝号来进⾏区别收到的⽹络数据是否是此进程的
udp_socket.bind(address)
address 是一个元组,元组的第一个元素是字符串类型的IP地址，第二个元素 整数端口号。 
udp_socket.bind(("", 8888)) # ip地址可以省略，省略后表示自己的ip地址

# FTP
安装vsftpd服务器: sudo apt-get install vsftpd -y
检查是否启动有端⼝为21的进程: netstat -tnl
ps -ef | grep ftp
配置vsftpd.conf⽂件
sudo gedit /etc/vsftpd.conf
匿名⽤户登录配置anonymous_enable=NO (默认为NO,不⽤修改)
设置本机访问local_enable=YES (默认即为YES，不⽤修改)
安装vsftpd服务器write_enable=YES 此⾏默认被注释了，去除前⾯的“#” 使得此句配置起作⽤
指定上传下载的⽬录local_root=/home/⽤户名/ftp
设置允许登录ftp服务器的⽤户
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd.chroot_list
创建“vsftpd.chroot_list”⽂件 sudo touch /etc/vsftpd.chroot_list
编辑⽂件，加⼊允许登录的⽤户名 直接写用户名
在ftp⽂件夹中建⽴share⽂件夹 mkdir share
减去ftp⽬录⽂件夹 的拥有者（u）权限w权限 chmod u-w ftp
重启 vsftpd 服务 sudo /etc/init.d/vsftpd restart
测试上传功能，登陆ftp服务器
打开linux 终端，输⼊ ftp ftp服务器ip 测试，即可进⼊ ftp 命令⾏模式:ftp IP
上传命令:put somefile
下载命令:get somefile
卸载vsftpd:sudo apt-get remove --purge vsftpd
(--purge 选项表示彻底删除改软件和相关⽂件)

# UDP编程
##服务的步骤
1、创建⼀个socket，⽤函数socket()；
2、设置socket属性，⽤函数setsockopt();* 可选
3、绑定IP地址、端⼝等信息到socket上，⽤函数bind();
4、循环接收数据，⽤函数recvfrom();
5、关闭⽹络连接；

##客户端步骤
1、创建⼀个socket，⽤函数socket()；
2、设置socket属性，⽤函数setsockopt();* 可选
3、绑定IP地址、端⼝等信息到socket上，⽤函数bind();* 可选
4、设置对⽅的IP地址和端⼝等属性;
5、发送数据，⽤函数sendto();
6、关闭⽹络连接；

# TCP简介
⾯向连接,基于字节流的传输层通信协议
过创建连接、数据传送、终⽌连接
这种连接是⼀对⼀的，因此TCP不适⽤于⼴播的应⽤程序，基于⼴播的应⽤程序请使⽤UDP协议。
发送应答机制:TCP发送的每个报⽂段都必须得到接收⽅的应答才认为这个TCP报⽂段传输成功
超时重传:发送端发出⼀个报⽂段之后就启动定时器，如果在定时时间内没有收到应答就重新发送这个报⽂段。
错误校验:TCP⽤⼀个校验和函数来检验数据是否有错误
流量控制和阻塞管理:流量控制⽤来避免主机发送得过快⽽使接收⽅来不及完全收下。

# TCP编程
##服务器端步骤
1、创建⼀个socket，⽤函数socket()；
2、设置socket属性，⽤函数setsockopt(); * 可选
3、绑定IP地址、端⼝等信息到socket上，⽤函数bind();
4、开启监听，⽤函数listen()；
5、接收客户端上来的连接，⽤函数accept()；
6、收发数据，⽤函数send()和recv()，或者read()和write();
7、关闭⽹络连接；
8、关闭监听；

##客户端步骤:
1、创建⼀个socket，⽤函数socket()；
2、设置socket属性，⽤函数setsockopt();* 可选
3、绑定IP地址、端⼝等信息到socket上，⽤函数bind();* 可选
4、设置要连接的对⽅的IP地址和端⼝等属性；
5、连接服务器，⽤函数connect()；
6、收发数据，⽤函数send()和recv()，或者read()和write();
7、关闭⽹络连接；

##TCP客户端发送/接收信息：
import socket # 1、导⼊socket模块
tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # 2、创建socket
tcp_client_socket.connect(("192.168.31.247", 7878)) # 3、建⽴tcp连接，这边服务端要启动才能链接成功
recv_data = tcp_client_socket.recv(1024)# 开始接收对⽅回复的数据
tcp_client_socket.send("哈哈哈，打不过我吧！？".encode("utf-8")) # 4、开始发送数据
tcp_client_socket.close() # 5、关闭套接字

##TCP服务端接收/回复信息：
from socket import *
tcp_server_socket = socket(AF_INET, SOCK_STREAM) # 创建socket
tcp_server_socket.bind(('', 7788)) # 绑定
tcp_server_socket.listen(128) # 使⽤socket创建的套接字默认的属性是主动的，使⽤listen将其变为被动的，这样就可以接收别⼈的链接了
client_socket, ip_port = tcp_server_socket.accept() # client_socket⽤来为这个客户端服务 # tcp_server_socket就可以省下来专⻔等待其他新客户端的链接
recv_data = client_socket.recv(1024) # 接收对⽅发送过来的数据 接收1024个字节
client_socket.send("好的，已经收到！～".encode('gbk')) # 发送⼀些数据到客户端
client_socket.close() # 关闭为这个客户端服务的套接字，只要关闭了，就意味着为不能再为这个客户端服务了，如果还需要服务，只能再次重新连接


# ⻓连接和短连接
在HTTP/1.0中, 默认使⽤的是短连接.也就是说, 浏览器和服务器每进⾏⼀次HTTP操作, 就建⽴⼀次连接, 但任务结束就中断连接.如果客户端浏览器访问的某个HTML或其他类型的 Web ⻚中包含有其他的Web资源，如js⽂件、图像⽂件、CSS⽂件等；当浏览器每遇到这样⼀个Web资源，就会建⽴⼀ 个HTTP会话。
但从 HTTP/1.1起，默认使⽤⻓连接，⽤以保持连接特性。使⽤⻓连接的HTTP协议，会在响应头 有加⼊这⾏代码:Connection:keep-alive
1在真正的读写操作之前，server与client之间必须建⽴⼀个连接，
2当读写操作完成后，双⽅不再需要这个连接时它们可以释放这个连接，
3连接的建⽴通过三次握⼿，释放则需要四次握⼿，


# tcp注意点
1. tcp服务器⼀般情况下都需要绑定端⼝和IP，否则客户端找不到这个服务器
2. tcp客户端⼀般不绑定，因为是主动链接服务器，所以只要确定好服务器的ip、port等信息就好，本地客户端可以随机
3. tcp服务器中通过listen可以将socket创建出来的主动套接字变为被动的，这是做tcp服务器时必须要做的
4. 当客户端需要链接服务器时，就需要使⽤connect进⾏链接，udp是不需要链接的⽽是直接发送，但是tcp必须先链接，只有链接成功才能通信
5. 当⼀个tcp客户端连接服务器时，服务器端会有1个新的套接字，这个套接字⽤来标记这个客户端，单独为这个客户端服务
6. listen后的套接字是被动套接字，⽤来接收新的客户端的链接请求的，⽽accept返回的新套接字是标记这个新客户端的
7. 关闭listen后的套接字意味着被动套接字关闭了，会导致新的客户端不能够链接服务器，但是之前已经链接成功的客户端正常通信。
8. 关闭accept返回的套接字意味着这个客户端已经服务完毕
9. 当客户端的套接字调⽤close后，服务器端会recv解堵塞，并且返回的⻓度为0，因此服务器可以通过返回数据的⻓度来区别客户端是否已经下线

# 模拟浏览器请求web过程
`
import socket
tcp_socket = socket.socket()
tcp_socket.connect(("www.itcast.com", 80)) # DNS解析和连接HTTP服务器
request_line = "GET / HTTP/1.1\r\n" # 组包，发送HTTP请求报文
request_header = "Host:www.icoderi.com\r\n" # 4.2 请求头
request_blank = "\r\n" # 4.3 请求空行
request_data = request_line + request_header + request_blank  # 整体拼接
tcp_socket.send(request_data.encode()) # 发送请求报文
response_data = tcp_socket.recv(4096) # 接收响应报文
response_str_data = response_data.decode() # 对响应报文进行解析
index = response_str_data.find("\r\n\r\n") # '\r\n\r\n'之后是响应体数据
html_data = response_str_data[index + 4:]
with open("index.html", "wb") as file:
    file.write(html_data.encode())
tcp_socket.close()
`

# 使用终端启动web服务器
启动格式：python3 webServer.py 8080
## 实现思路
`
1.导入sys模块
2.sys.argv获取参数列表
3.判断列表长度是否为2，如果不等于2给出提示启动失败
4.去除第二个参数，判断是否为一个数字，如果不是一个数组，则给出提示。
5.接受启动参数端口号
6.使用提供的端口号启动web服务器
`
sys.argv[]是⽤来获取命令⾏参数的
sys.argv[0]⽐如在CMD命令⾏输⼊ “python test.py -help”，那么sys.argv[0]就代表“test.py”
sys.startswith() 是⽤来判断⼀个对象是以什么开头的，⽐如在python命令⾏输⼊“'abc'.startswith('ab')”就会返回True。
检测字符串是否只由数字组成：str.isdigit()  ==> True False
```python
import sys
def main():
    if len(sys.argv) != 2:  # 判断参数个数是否合法
        print("启动失败，参数个数错误！正确格式:python xxx.py 8080")
        return
    if not sys.argv[1].isdigit():  # 判断端⼝是否是纯数字
        print("启动失败，端⼝不是数字!")
        return
    port = int(sys.argv[1])  # 保存端⼝
```

# 实现网游服务器
## 实现思路
`
1、定义初始化项⽬的⽅法
2、定义字典保存项⽬路径 字典格式：key=项⽬名称 value=路径
3、循环获取字典中的所有key
4、根据key获取游戏的主⽬录
5、设置实例属性，保存主⽬录
6、修改Web服务器的⽬录为实例属性保存的主⽬录
`



## 目录解释：
- /bin、/usr/bin：可执⾏⼆进制⽂件的⽬录，如常⽤的命令 ls、tar、mv、cat 等
- /boot：放置 linux 系统启动时⽤到的⼀些⽂件，如 linux 的内核⽂件： /boot/vmlinuz ，系统引导管理器： /boot/grub
- /dev：存放linux系统下的设备⽂件，访问该⽬录下某个⽂件，相当于访问某个设备，常⽤的是挂载光驱 mount /dev/cdrom /mnt
- /etc：系统配置⽂件存放的⽬录，不建议在此⽬录下存放可执⾏⽂件，重要的配置⽂件有：/inittab，/fstab，/init.d，/sysconfig，/xinetd.d，/X11
- /lib、/usr/lib、/usr/local/lib：系统使⽤的函数库的⽬录，程序在执⾏过程中，需要调⽤⼀些额外的参数时需要函数库的协助
- /lost+fount：系统异常产⽣错误时，会将⼀些遗失的⽚段放置于此⽬录下
- /mnt: /media：光盘默认挂载点，通常光盘挂载于 /mnt/cdrom 下，也不⼀定，可以选择任意位置进⾏挂载
- /opt：给主机额外安装软件所摆放的⽬录
- /proc：此⽬录的数据都在内存中，如系统核⼼，外部设备，⽹络状态，不占⽤磁盘空间：/proc/cpuinfo、/proc/interrupts、/proc/dma、/proc/ioports、/proc/net/*
- /sbin、/usr/sbin、/usr/local/sbin：放置系统管理员使⽤的可执⾏命令，如 fdisk、shutdown、mount 等。与 /bin 不同的是，这⼏个⽬录是给系统管理员 root 使⽤的命令，⼀般⽤户只能"查看"⽽不能设置和使⽤
- /tmp：⼀般⽤户或正在执⾏的程序临时存放⽂件的⽬录，任何⼈都可以访问，重要数据不可放置在此⽬录下
- /srv：服务启动之后需要访问的数据⽬录，如 www 服务需要访问的⽹⻚数据存放在 /srv/www 内
- /usr：应⽤程序存放⽬录，/usr/bin：存放应⽤程序，/usr/share：存放共享数据，/usr/lib：存放不能直接运⾏的，却是许多程序运⾏所必需的⼀些函数库⽂件，/usr/local：存放软件升级包，/usr/share/doc：系统说明⽂件存放⽬录，/usr/share/man：程序说明⽂件存放⽬录
- /var：放置系统执⾏过程中经常变化的⽂件，/var/log：随时更改的⽇志⽂件，/var/spool/mail：邮件存放的⽬录，/var/run：程序或服务启动后，其 PID 存放在该⽬录下

*命令格式：命令名 [-options] 命令参数*

```shell
pwd # 查看路径
ls # 查看列表
tree # 查看目录结构
```
- ls -a 显示隐藏文件
- ls -l 以列表方式显示文件详细信息
- ls -h配合-l以人性化方式显示文件大小
- ls -l -h == ls -lh ls -al == ll

## 文件操作
- mkdir -p 递归创建目录
- gedit 文件名 ==> 打开文件
- rm -rf: -r ==> 递归删除目录下内容 -f==>强制删除，忽略不存在的文件
- cp: -a 保留链接、文件属性，并且递归复制目录。-f对存在的目标文件不提示。 -i交互复制，提示用户确认。-r递归复制目录下文件
- mv：-f 禁止交互操作。-i 确认交互方式操作。-v 显示移动速度。
- clear == CTRL + L
- which：查看命令的绝对路径
- cal：日历 -3 显示三个月的日历。-j显示当年中第几天。-y 显示当前年份的日历
- data：日期显示，格式化显示：data ""+%Y年%m⽉%d⽇ %H时%M分%S秒"
- history:查看历史的指令，显示10条历史信息，history 10，执⾏历史命令： !编号 如：!102

- cat 命令⽤于 查看 、 连接⽂件 并 打印到标准输出设备 上。-n 由1开始对所有输出行编号。-b 空白行不编号：cat -n 1.txt
- cat 1.txt 2.txt ：用cat方式将1和2文件合并查看

- more 以 全屏幕的⽅式 分⻚(屏) 显示⽂本⽂件的内容
- Enter 向下n⾏，需要定义。默认为1⾏.
- Ctrl+F 向下滚动⼀屏，F(front,前进)
- Ctrl+B 返回上⼀屏,B(back,后退)
- 空格键 向下滚动⼀屏
- q 退出 more

## 数据流重定向：>
> 命令 > a.txt 把执行的结果重定向输出到a.txt文件里面

### 管道：|
分为两端，左端塞东西，右端取东西
它只能处理经由前⾯⼀个指令传出的正确输出信息，对错误信息信息没有直接处理能⼒。然后，传递给下⼀个命令，作为标准的输⼊.

### 软链接：软链接不占⽤磁盘空间，源⽂件删除则软链接失效。
> ln -s 源⽂件 链接⽂件

### 硬链接(hard link, 也称链接)：就是⽂件的⼀个或多个⽂件名
> ln 源⽂件 链接⽂件

如果 没有-s 选项代表建⽴⼀个硬链接⽂件，两个⽂件占⽤相同⼤⼩的硬盘空间，即使删除了源⽂件，链接⽂件还是存在，所以-s选项是更常⻅的形式。
注意：如果软链接⽂件和源⽂件不在同⼀个⽬录，源⽂件要使⽤绝对路径，不能使⽤相对路径。

**软连接和硬链接的区别**
- 软链接可以跨⽂件系统，硬链接不可以;
- 软链接可以对⼀个不存在的⽂件名(filename)进⾏链接(当然此时如果你vi这个软链接⽂件，
linux会⾃动新建⼀个⽂件名为filename的⽂件),硬链接不可以(其⽂件必须存在，inode必须存在);
- 软链接可以对⽬录进⾏连接，硬链接不可以。

## 查询操作
- find ./ -name test.sh 查找当前⽬录下所有名为test.sh的⽂件
- find ./ -name '*.sh' 查找当前⽬录下所有后缀为.sh的⽂件
- find ./ -name "[A-Z]*"查找当前⽬录下所有以⼤写字⺟开头的⽂件，* 表示任意字符，？表示任意⼀个字符，[列举字符] 表示列举出的任意⼀个字符
- find /tmp -size 2M 查找在/tmp ⽬录下等于2M的⽂件
- find /tmp -size +2M 查找在/tmp ⽬录下⼤于2M的⽂件
- find /tmp -size -2M 查找在/tmp ⽬录下⼩于2M的⽂件
- find ./ -size +4k -size -5M 查找当前⽬录下⼤于4k，⼩于5M的⽂件
- find ./ -perm 777 查找当前⽬录下权限为 777 的⽂件或⽬录


## tar使⽤格式
- 多⽂件归档： tar [参数] 打包⽂件名 ⽂件1 ⽂件2
- ⽬录归档： tar [参数] 打包⽂件名 ⽬录
- tar命令很特殊，其参数前⾯可以使⽤“-”，也可以不使⽤。
- -c ⽣成档案⽂件，创建打包⽂件
- -v 列出归档解档的详细过程，显示进度
- -f 指定档案⽂件名称，f后⾯⼀定是.tar⽂件，所以必须放选项最后
- -t 列出档案中包含的⽂件
- -x 解开档案⽂件
*注意：除了f需要放在参数的最后，其它参数的顺序任意。*

- tar -cvf 打包⽂件名.tar 要打包的内容 打包⼀个⽂件
- tar -xvf 打包⽂件名.tar 解包，得到包中的内容
- tar -zcvf 压缩包⽂件名.tar.gz 待压缩⽂件或⽬录
- tar -zxvf 压缩包⽂件名.tar.gz

- zip -r a.zip a 把a⽬录压缩为 a.zip
- unzip a.zip 把a.zip 解压到当前⽬录下
- unzip -d test a.zip 把a.zip 解压到 test ⽬录中

- tar.gz 的打包和压缩⽅式 相⽐ zip 或者 bz2 产⽣的压缩包⽂件更⼩


## 文件权限
- r可读，w可写，x可执行
- drwxr-xr-x：d后面的2-9位，代表文件权限，三个一组。分为三组u，g，o
- rwx：u属用户（文件拥有者）， r-x：g属组（同组用户）， r-x：o其他人
- chmod 数字/字母 修改文件权限
### 字母法：
> chmod u/g/o/a +/-/= rwx ⽂件

- u：user表示文件的所有者
- g：group表示该文件所有者属于一个组
- o：other表示其他以外的人
- a：all表示这三者皆是
- +：增进权限
- -：撤销权限
- =：设定权限
同时设定三组权限：chmod u=,g=,o= 1.txt

### 数字法：
> r:4, w:2, x:1

如执⾏：chmod u=rwx,g=rx,o=r filename 就等同于：chmod u=7,g=5,o=4 filename
如果要递归，就在文件后面加上-R
chmod 777 test/ -R

- whoami ==> 查看当前账户
- passwd 用户名 ==> 修改账户密码

## reboot 重新启动操作系统
- shutdown –r now 重新启动操作系统，shutdown会给别的⽤户提示
- shutdown -h now ⽴刻关机，其中now相当于时间为0的状态
- shutdown -h 20:25 系统在今天的20:25 会关机
- "shutdown -c" 可以取消关机
- shutdown -h +10 系统再过⼗分钟后⾃动关机

## 安装软件
- make install 源代码安装包
- sudo dpkg -i package.deb
- sudo apt-get install xxxx

## 下载和上传
- 下载：scp [-r] ⽬标⽤户名@⽬标主机IP地址：/⽬标⽂件的绝对路径 /保存到本机的绝对/相对路径
- 远程⽂件复制到本地：scp teacher@192.168.31.163:/home/teacher/test.py ./test.py
- 远程⽂件夹复制到本地：scp -r teacher@192.168.31.163:/home/teacher/test ./test
- 本地⽂件复制到远程：scp test.py teacher@192.168.31.163:/home/teacher/test2.py
- 本地⽬录复制到远程：scp -r ./test teacher@192.168.199.122:/home/teacher/test

## 配置jdk
```shell
vim /etc/profile
```
添加信息
```shell
export JAVA_HOME=/usr/local/jdk/jdk1.8.0_181
export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
```
刷新
```shell
source /etc/profile
```

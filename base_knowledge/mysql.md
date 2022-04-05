## 数据库的安装

1.ubuntu安装mysql

```shell
sudo apt-get install mysql-server
```
- 当前使⽤的ubuntu镜像中已经安装好了mysql服务器端，⽆需再安装，并且设置成了开机⾃启动
- 服务器⽤于接收客户端的请求、执⾏sql语句、管理数据库
- 服务器端⼀般以服务⽅式管理，名称为mysql
  
- 启动服务

```shell
sudo service mysql start
```

- 查看进程中是否存在mysql服务

```shell
ps ajx|grep mysql
```

- 停⽌服务

```shell
sudo service mysql stop
```

- 重启服务

```shell
sudo service mysql restart
```

2. MySQL配置⽂件

> 路径：为/etc/mysql/mysql.conf.d/mysql.cnf

- 主要配置项如下
    - bind-address表示服务器绑定的ip，默认为127.0.0.1
    - port表示端⼝，默认为3306
    - datadir表示数据库⽬录，默认为/var/lib/mysql
    - general_log_file表示普通⽇志，默认为/var/log/mysql/mysql.log
    - log_error表示错误⽇志，默认为/var/log/mysql/error.log

3. ubuntu安装navicat

- 可以到Navicat官⽹下载
- 将压缩⽂件拷⻉到ubuntu虚拟机中，放到桌⾯上，解压

```shell
tar zxvf navicat112_mysql_cs_x64.tar.gz
```
- 进⼊解压的⽬录，运⾏如下命令
```shell
./start_navicat
```

- 问题⼀：中⽂乱码
    - 打开start_navicat⽂件 将export LANG="en_US.UTF-8"改为export LANG="zh_CN.UTF-8"
    - 打开软件，选择⼯具-->选项-->
- 问题⼆：试⽤期
    - 删除⽤户⽬录下的.navicat64⽬录 cd ~   rm -r .navicat64


```shell
sudo apt-get install mysql-client
```

## 数据库完整性

约束介绍：约束作⽤是保证数据的完整性和⼀致性，分为表级约束和列级约束。

|约束类型 |约束说明|
|:---|---:|
|NOT NULL |⾮空约束（设置⾮空约束，该字段不能为空）|
|PRIMARY KEY |主键约束（唯⼀性，⾮空性）|
|UNIQUE KEY |唯⼀约束（唯⼀性，可以空，但只能有⼀个）|
|DEFAULT |默认约束（该数据的默认值）|
|FOREIGN KEY |外键约束（需要建⽴两表间的关系）|

## 数据类型
1. 数值
- 浮点型float(M,D) 、 double (M,D)  M代表总数字的位数，最⼤值是255。D代表其中⼩数的位数。
- float只保证6位有效数字的准确性 double只保证16位有效数字的准确性
- 定点数decimal(M,D)，M代表总的数字位数【最⼤为65】，D代表其中的⼩数位。

2. 字符串
- char最多能保存255个字符(⽆论中⽂还是英⽂还是任何符号
- varchar最多能保存65535个字节
- char也叫做定⻓字符串，指的是在创建表时，char字段占⽤硬盘空间的⼤⼩就已经固定了。
- varchar也叫做变⻓字符串，指的是字段占⽤硬盘空间的⼤⼩并不是固定的，⽽是由内容决定的，等于内容的⻓度+1个字节(字符串结束'\0')。
- text:与char和varchar不同的是，text不可以有默认值，其最⼤⻓度是2的16次⽅-1
经常变化的字段⽤varchar知道固定⻓度的⽤char超过255字符的只能⽤varchar或者text

3.  枚举类型

- 枚举类型enum，在定义字段时就预告规定好固定的⼏个值，然后插⼊记录时值只能这⼏个固定好的值中选择⼀个。
- ⼀个enum最多可以设置65535个值
- 当值是⼏个固定可选时，⽐如：性别、星期、⽉份、表示状态时(⽐如:是、否)

4. 时间类型
- datetime保存时间的范围： '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'
- timestamp保存时间的范围： '1970-01-01 00:00:01' 到 '2038-01-19 03:14:07'



## 数据库操作

1.数据库基本操作
```sql
show databases;
select database();
create database 数据库名 charset=utf8;
use 数据库名;
drop database 数据库名;
```

2.创建表

```sql
CREATE TABLE table_name(
-- 字段名称 数据类型 可选的约束条件,
id int unsigned auto_increment primary key not null,
column1 datatype contrai,
column2 datatype,
column3 datatype,
.....
columnN datatype,
-- 主键说明可以放在字段中单独说明 也可以放在最后统⼀说明
PRIMARY KEY(one or more columns)
);
```

3.查看当前数据库中所有的表
```sql
show tables;
-- 查看表结构
desc 表名;
-- 查看表的创建语句
show create table 表名;
```

4.修改表
```sql
--  添加字段
alter table 表名 add 列名 类型;
-- 重命名字段
alter table 表名 change 原名 新名 类型及约束;
-- 修改字段类型
alter table 表名 modify 列名 类型及约束;
--  删除字段
alter table 表名 drop 列名;
```
5.删除表

```sql
drop table 表名;
```

6.表数据操作-CRUD

```sql
select * from 表名;
select 列1,列2,... from 表名;
insert into 表名 values (...);
insert into 表名 (列1,...) values(值1,...);
-- 全列多⾏插⼊
insert into 表名 values(...),(...)...;
-- 部分列多⾏插⼊
insert into 表名(列1,...) values(值1,...),(值1,...)...;
update 表名 set 列1=值1,列2=值2... where 条件;
delete from 表名 where 条件;
```

7.as起别名

> 注意: 在这⾥给表起别名看起来并没有什么意义,然⽽并不是这样的，我们在后期学习⾃连接的时候，必须要对表起别名。

```sql
select id as 序号, name as 名字, gender as 性别 from students;
select s.id,s.name,s.gender from students as s
```

8.distinct可以消除重复的⾏

```sql
select distinct 列1,... from 表名;
-- 看到了很多重复数据 想要对其中重复数据⾏进⾏去重操作可以使⽤ distinct
select distinct gender from students;
```

9.where

> where后⾯⽀持多种运算符，进⾏条件的处理：⽐较运算符 逻辑运算符 模糊查询 范围查询 空判断

```sql
-- ⽐较运算符
select * from students where id > 3;
--  逻辑运算符
select * from students where id > 3 and gender=0;
-- 模糊查询 %表示任意多个任意字符 / _表示⼀个任意字符
select * from students where name like '⻩%';
select * from students where name like '⻩_';
-- 范围查询 bettween-and 和in
select * from students where id in(1,3,8); -- 非连续
select * from students where id between 3 and 8;  -- 连续
select * from students where (id between 3 and 8) and gender=1;
-- 空判断
select * from students where height is null;
select * from students where height is not null and gender=1;
```

**总结**

1. 常⻅的⽐较运算符有 >,<,>=,<=,!=
2. 逻辑运算符and表示and连接的多个条件同时成⽴则为真，or表示or连接的多个条件有⼀个成⽴则为真，not表示对条件取反
3. like和%结合使⽤表示任意多个任意字符，like和_结合使⽤表示⼀个任意字符
4. bettween-and限制连续性范围 in限制⾮连续性范围
5. 判空⽤IS NULL 判⾮空⽤ IS NOT NULL

## 排序 order by
> select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...]

1. 将⾏数据按照列1进⾏排序，如果某些⾏ 列1 的值相同时，则按照 列2 排序，以此类推
2. asc从⼩到⼤排列，即升序
3. desc从⼤到⼩排序，即降序
4. 默认按照列值从⼩到⼤排列（即asc关键字）

```sql
select * from students where gender=1 and is_delete=0 order by id desc;
select * from students where is_delete=0 order by name;
-- 先按照年龄从⼤-->⼩排序，当年龄相同时 按照身⾼从⾼-->矮排序
select * from students order by age desc,height desc;
```

## 聚合函数

> count(*)  max(列)  min(列)  sum(列)  avg(列)

特点：
- 每个组函数接收⼀个参数（字段名或者表达式）
- 统计结果中默认忽略字段为NULL的记录 要想列值为NULL的⾏也参与组函数的计算，必须使⽤IFNULL函数对NULL值做转换。
- 不允许出现嵌套 ⽐如sum(max(xx))

```sql
select count(*) from students;
select max(id) from students where gender=2;
select min(id) from students where is_delete=0;
-- 平均年龄
select sum(age)/count(*) from students where gender=1;
select avg(id) from students where is_delete=0 and gender=2;
```

## 分组 group by
分组就是将⼀个“数据集”划分成若⼲个“⼩区域”，然后针对若⼲个“⼩区域”进⾏数据处理。

使⽤特点
- group by的含义:将查询结果按照1个或多个字段进⾏分组，字段值相同的为⼀组
- group by可⽤于单个字段分组，也可⽤于多个字段分组

1.group by + group_concat()

> group_concat(字段名)根据分组结果，使⽤group_concat()来放置每⼀个分组中某字段的集合

```sql
select gender,group_concat(name) from students group by gender;
```
+--------+-----------------------------------------------------------+
| gender | group_concat(name) |
+--------+-----------------------------------------------------------+
| 男 | 彭于晏,刘德华,周杰伦,程坤,郭靖 |
| ⼥ | ⼩明,⼩⽉⽉,⻩蓉,王祖贤,刘亦菲,静⾹,周杰 |
| 中性 | ⾦星 |
| 保密 | 凤姐 |
+--------+-----------------------------------------------------------+

2.group by + 聚合函数

> 聚合函数在和group by结合使⽤的时候 统计的对象是每⼀个分组。

```sql
select gender,group_concat(age) from students group by gender;
-- 分别统计性别为男/⼥的⼈年龄平均值
select gender,avg(age) from students group by gender;
-- 分别统计性别为男/⼥的⼈的个数
select gender,count(*) from students group by gender;
```

3.group by + having

> having 条件表达式：⽤来过滤分组结果

```sql
select gender,count(*) from students group by gender having count(*)>2;
```

4.group by + with rollup

> with rollup的作⽤是：在最后新增⼀⾏，来记录当前表中该字段对应的操作结果，⼀般是汇总结果。

```sql
select gender,count(*) from students group by gender with rollup;
```

```sql
select gender,group_concat(age) from students group by gender with rollup;
```

**总结：**
1. group by 关键字能根据1个或多个字段对数据进⾏分组
2. group_concat函数作⽤就是将每个分组中的每个成员的指定个字段拼接在⼀⾏中显示
3. 聚合函数在和 group by 结合使⽤时, 统计的对象是每个分组
4. having 是对分组结果进⾏条件过滤
5. with rollup在分组结果最后新增⼀⾏完成汇总显示。


## 限制 limit
可以使⽤limit限制取出记录的数量，但limit要写在sql语句的最后。
> limit 起始记录,记录数

- 起始记录是指从第⼏条记录开始取，第⼀条记录的下标是0。  
- 记录数是指从起始记录开始向后依次取的记录数。

```sql
-- 从第下标为0的记录开始取，取3条
select * from students limit 0,3;
```

## 连接 join on

- 内连接查询：查询的结果为两个表匹配到的数据
- 右(外)连接查询：查询的结果为两个表匹配到的数据和右表特有的数据
- 左(外)连接查询：查询的结果为两个表匹配到的数据和左表特有的数据

```sql
-- 内连接查询班级表与学⽣表
select * from students inner join classes on students.cls_id = classes.id;
-- 左连接查询班级表与学⽣表
select * from students as s left join classes as c on s.cls_id = c.id;
-- 右连接查询班级表与学⽣表
select * from students as s right join classes as c on s.cls_id = c.id;
-- 查询学⽣姓名及班级名称
select s.name,c.name from students as s inner join classes as c on s.cls_id = c.id;
```
**总结**
1. 连接的⽬的主要将多张的相关数据汇总⼀个结果集中
2. 连接分为内连接, 左连接, 右连接
3. on 关键字⽤于 设置多表连接操作的条件
4. 内连接 inner join 的结果是表与表之间满⾜连接条件的数据
5. 外连接 outer join 是在内连接的基础上添加了外部数据，外部数据来⾃于左表(右表数据位置对应填充NULL) 则是左连接；外部数据来⾃于右表（左表数据位置对应填充NULL）则是右连接。
6. 注意: 能够使⽤连接的前提是 多表之间有字段上的关联。

## 子查询
主查询和⼦查询的关系
- ⼦查询是嵌⼊到主查询中
- ⼦查询是辅助主查询的,要么充当条件,要么充当数据源
- ⼦查询是可以独⽴存在的语句,是⼀条完整的 select 语句

```sql
-- 查询⼤于平均年龄的学⽣
select * from students where age > (select avg(age) from students);
-- 查找班级年龄最⼤,身⾼最⾼的学⽣
select name from classes where id in (select cls_id from students);
-- 查找班级年龄最⼤,身⾼最⾼的学⽣
select * from students where (height,age) = (select max(height),max(age) from students);
```
**总结:**
1. ⼦查询是⼀个完整的SQL语句
2. ⼦查询分为三种 标量、⾏、列⼦查询
3. 标量⼦查询返回的结果⼀⾏⼀列
4. 列⼦查询使⽤格式: 主查询 where 条件 in (列⼦查询)
5. ⾏⼦查询使⽤格式: 主查询 where (字段1,2,...) = (⾏⼦查询)

## 分页
> select * from 表名 limit start=0,count

1. 从start开始，获取count条数据
2. start默认值为0
3. 也就是当⽤户需要获取数据的前n条的时候可以直接写上 xxx limit n;

```sql
-- 查询前3⾏男⽣信息
select * from students where gender=1 limit 0,3;
```
总要是下标的地推
 (n-1)*m m
1. 使⽤limit限制数据显示数量
2. limit后 参数⼀ 数据查询的起始下标，参数⼆ 显示查询数据的数量

## 总结

```sql
SELECT select_expr [,select_expr,...] [
FROM tb_name
[JOIN 表名]
[ON 连接条件]
[WHERE 条件判断]
[GROUP BY {col_name | postion} [ASC | DESC], ...]
[HAVING WHERE 条件判断]
[ORDER BY {col_name|expr|postion} [ASC | DESC], ...]
[ LIMIT {[offset,]rowcount | row_count OFFSET offset}]
]
```

## 数据库三范式

1.第⼀范式（1NF）：强调的是列的原⼦性，即列不能够再分成其他⼏列。

2.第⼆范式（2NF）：满⾜ 1NF，另外包含两部分内容，
  - 表必须有⼀个主键；
  - ⾮主键字段 必须完全依赖于主键，⽽不能只依赖于主键的⼀部分。

3.第三范式（3NF）：满⾜ 2NF，另外⾮主键列必须直接依赖于主键，不能存在传递依赖。即不能存在：⾮主键列 A 依赖于⾮主键列 B，⾮主键列 B 依赖于主键的情况。

**总结：**
- 1NF强调字段是最⼩单元，不可再分
- 2NF强调在1NF基础上必须要有主键和⾮主键字段必须完全依赖于主键，也就是说 不能部分依赖
- 3MF强调在2NF基础上 ⾮主键字段必须直接依赖于主键，也就是说不能传递依赖(间接依赖)。



## 索引

1.索引的使⽤

```sql
-- 查看表中已有索引
show index from 表名;
-- 创建索引
create index 索引名称 on 表名(字段名称(⻓度))
-- 删除索引
drop index 索引名称 on 表名;
```
2. 查询

```sql
-- 开启运⾏时间监测：
set profiling=1;
-- 查找第1万条数据ha-99999
select * from test_index where title='ha-99999';
-- 查看执⾏的时间：
show profiles;
-- 为表title_index的title列创建索引：
create index title_index on test_index(title(10));
-- 执⾏查询语句：
select * from test_index where title='ha-99999';
-- 再次查看执⾏的时间
show profiles;
```

**注意**
建⽴太多的索引将会影响更新和插⼊的速度，因为它需要同样更新每个索引⽂件。对于⼀个经常需要更新和插⼊的表格，就没有必要为⼀个很少使⽤的where字句单独建⽴
索引了，对于⽐较⼩的表，排序的开销不会很⼤，也没有必要建⽴另外的索引。建⽴索引会占⽤磁盘空间




## ⽤户管理

MySQL账户体系：根据账户所具有的权限的不同，MySQL的账户可以分为以下⼏种
- 服务实例级账号：，启动了⼀个mysqld，即为⼀个数据库实例；如果某⽤户如root,拥有服务
- 实例级分配的权限，那么该账号就可以删除所有的数据库、连同这些库中的表
- 数据库级别账号：对特定数据库执⾏增删改查的所有操作
- 数据表级别账号：对特定表执⾏增删改查等所有操作
- 字段级别的权限：对某些表的特定字段进⾏操作
- 存储程序级别的账号：对存储程序进⾏增删改查的操作

**注意：**
进⾏账户操作时，需要使⽤root账户登录，这个账户拥有最⾼的实例级权限。账户的操作主要包括创建账户、删除账户、修改密码、授权权限等。


1.查看所有⽤户

```sql
use mysql;
desc user;
select host,user,authentication_string from user;
```
authentication_string表示密码，为加密后的值

2.创建账户、授权

```sql
grant 权限列表 on 数据库 to '⽤户名'@'访问主机' identified by '密码';

grant select on jing_dong.* to 'laowang'@'localhost' identified by '123456';
```
- 可以操作python数据库的所有表，⽅式为: jing_dong.*
- 访问主机通常使⽤ 百分号% 表示此账户可以使⽤任何ip的主机登录访问此数据库
- 访问主机可以设置成 localhost或具体的ip，表示只允许本机或特定主机访问

3.查看⽤户有哪些权限

```sql
show grants for laowang@localhost;
```

4.修改权限

```sql
grant 权限名称 on 数据库 to 账户@主机 with grant option;
```

5.修改密码

```sql
update user set authentication_string=password('新密码') where user='⽤户名';

update user set authentication_string=password('123') where user='laowang';
```

6.刷新权限

改完成后需要刷新权限
```sql
flush privileges
```

7.删除账户

使⽤root登录
```sql
drop user '⽤户名'@'主机';
drop user 'laowang'@'%';

delete from user where user='⽤户名';
delete from user where user='laowang';

-- 操作结束之后需要刷新权限
flush privileges
```
推荐使⽤语法1删除⽤户, 如果使⽤语法1删除失败，采⽤语法2⽅式
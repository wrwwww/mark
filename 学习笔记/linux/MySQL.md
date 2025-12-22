# 1. 安装与配置

## 1.1. 下载链接

[官网](http://dev.mysql.com/downloads/mysql/)

## 1.2. Windows

### 1.2.1. 配置文件

在下载的mysql压缩包解压后的根目录创建文件`my.ini`,并添加以下配置

```
[mysqld]
#设置3306端口
port=3306
#设置mysql的安装目录
basedir=D:\\software\\mysql-8.0.34-winx64
#设置mysql数据库的数据的存放目录
datadir=D:\\software\\mysql-8.0.34-winx64\\data
#允许最大连接数
max_connections=200
#允许连接失败的次数。
max_connect_errors=10
#服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
#创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
#默认使用“mysql_native_password”插件认证
#mysql_native_password
authentication_policy=*
[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8mb4
# set 是字符集 collation是默认排序规则（比较大小、排序规则）
[client]
#设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

### 1.2.2. 添加环境变量

将解压后的bin文件夹添加到环境变量

### 1.2.3. 生成临时密码

```
mysqld --initialize --console
// 初始化
 ./mysqld.exe --initialize-insecure --defaults-file="D:\software\mysql-5.1.73-winx64\my.ini" --install mysql
```

该命令会在命令行生成临时随机密码

### 1.2.4. mysql服务

#### 1.2.4.1. 创建服务

```
# 第一种
mysqld --install mysql
# 第二种创建Windows服务
sc create mysql binPath= mysqld_bin_path(注意：等号与值之间有空格)
```

#### 1.2.4.2. 删除服务

```
mysqld --remove mysql
```

#### 1.2.4.3. 打开服务

```
net start mysql
```

#### 1.2.4.4. 关闭服务

```
net stop mysql 
```

### 1.2.5. 登录

```
# 回车后输入临时密码
mysql -uroot -p
mysql -h 地址 -P 端口 -u 用户名 -p 密码
```

### 1.2.6. 修改密码

```
# 修改密码
alter user 'root'@'localhost' identified by '1234';
-- 修改当前用户的密码
PASSWORD() 函数在 MySQL 8.0 中被 移除了。

-- MySQL 8.0 使用 ALTER USER ... IDENTIFIED BY 'new_password' 来修改密码。
SET PASSWORD = PASSWORD('新密码');
```

然后刷新权限：

```
FLUSH PRIVILEGES;
```

## 1.3. Linux在 **AlmaLinux / CentOS / RHEL 8+** 上，使用 `dnf` 安装 MySQL 的步骤可以分成 **官方仓库安装** 和 **MySQL 官方 Yum 仓库安装** 两种方式。下面我给你整理一份完整、可直接执行的步骤。

---

## 1.4. 一、使用系统默认仓库安装（MariaDB 兼容 MySQL）

AlmaLinux 8 及 RHEL/CentOS 8 默认仓库中，`mysql` 通常指 MariaDB（MySQL 的开源分支）。如果你只需要兼容 MySQL 的数据库，可以直接用：

```
sudo dnf install mysql-server -y
```

安装完成后：

```
# 启动 MySQL 服务
sudo systemctl enable --now mysqld

# 查看状态
sudo systemctl status mysqld
```

初始 root 密码在日志中：

```
sudo grep 'temporary password' /var/log/mysqld.log
```

然后使用 `mysql_secure_installation` 初始化：

```
sudo mysql_secure_installation
```

---

## 1.5. 二、使用官方 MySQL Yum 仓库安装（安装 Oracle MySQL）

如果你想安装**Oracle 官方 MySQL（比如 8.0）**，推荐步骤如下：

### 1.5.1. 1️⃣ 下载官方仓库 RPM

```
sudo dnf install https://repo.mysql.com/mysql80-community-release-el8-7.noarch.rpm
```

注意：根据你的 AlmaLinux/RHEL 版本选择对应 RPM，例如 EL8、EL9。

### 1.5.2. 2️⃣ 查看可用版本

```
dnf module list mysql
dnf repolist enabled | grep mysql
```

### 1.5.3. 3️⃣ 安装 MySQL 服务端

```
sudo dnf install mysql-community-server -y
```

### 1.5.4. 4️⃣ 启动 MySQL 并设置开机自启

```
sudo systemctl enable --now mysqld
sudo systemctl status mysqld
```

### 1.5.5. 5️⃣ 获取初始 root 密码

官方 MySQL 安装会在 `/var/log/mysqld.log` 中生成临时密码：

```
sudo grep 'temporary password' /var/log/mysqld.log
```

然后执行：

```
sudo mysql_secure_installation
```

完成安全初始化，例如：

- 修改 root 密码
- 移除匿名用户
- 禁止远程 root 登录
- 删除测试数据库

---

## 1.6. 三、常用管理命令

```
# 启动 MySQL
sudo systemctl start mysqld

# 停止 MySQL
sudo systemctl stop mysqld

# 重启 MySQL
sudo systemctl restart mysqld

# 查看状态
sudo systemctl status mysqld

# 登录 MySQL
mysql -u root -p
```

---

## 1.7. 四、推荐注意事项

1. 如果你之前安装过 MariaDB，需要先卸载，否则可能冲突：

```
sudo dnf remove mariadb-libs mariadb-server
```

2. 官方 MySQL 安装完毕后，建议设置开机自启：

```
sudo systemctl enable mysqld
```

3. 配置防火墙开放 3306 端口（如果需要远程访问）：

```
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload
```

---

我可以帮你整理一份 **AlmaLinux 8 上安装 MySQL 8 的最简一步到位命令脚本**，直接复制执行就能完成安装和初始化。

你希望我帮你写吗？

## 1.8. 一、临时跳过公钥验证

如果只是临时安装某个包，可以在命令中加参数：

```
sudo dnf install mysql-community-server --nogpgcheck -y
```

解释：

- `--nogpgcheck` → 不验证 GPG 公钥
- `-y` → 自动确认安装

⚠️ 注意：跳过 GPG 校验存在安全风险，只建议在可信环境下使用。

# 2. 备份与恢复

## 2.1. 逻辑备份

### 2.1.1. 概念

逻辑备份其实就是利用MySQL数据库自带的mysqldump命令，或者使用第三方的工具，然后把数据库里的数据以SQL语句的方式导出成文件的形式。在需要恢复数据时，通过使用相关的命令（如：source ）将备份文件里的SQL语句提取出来重新在数据库中执行一遍，从而达到恢复数据的目的。

逻辑备份的优点与使用场景

优点：简单，易操作，自带工具方便、可靠。

使用场景：数据库数据量不大的情况可以使用，数据量比较大（超过20G左右）时备份速度比较慢，一定程度上还会影响数据库本身的性能。

### 2.1.2. 备份

#### 2.1.2.1. 导出到sql文件

导出sql文件

```
mysqldump -A -B --single-transaction >/server/backup/mysql_$(date +%F).sql
```

导出压缩文件

```
mysqldump -A -B --single-transaction ｜gzip>/server/backup/mysql_$(date +%F).sql.gz
```

#### 2.1.2.2. 单个表的备份

```
# 第一种直接备份
create table A_backup select * from A

# 第二种
# 复制表结构
create table a_backup like A;
INSERT INTO A_BACKUP SELECT * FROM A; -- 导入数据

# SELECT ...... INTO OUTFILE filename [OPTIONS] 文件不能已存在
SELECT * FROM test.person INTO OUTFILE "C:\person0.txt";
```

[https://www.cnblogs.com/yourblog/p/10381962.html](https://www.cnblogs.com/yourblog/p/10381962.html)

  

### 2.1.3. 恢复

### 2.1.4. 恢复导出的sql文件

如果导出的sql经过压缩需要解压

```
# linux
gzip -o mysql_$(date +%F).sql.gz
```

在终端

```
mysql.exe --user=root --host=127.0.0.1 --port=3306 < D:\Desktop\_localhost-2023_08_13_08_47_5-dump.sql
```

在mysql环境中

```
source /server/backup/mysql_$(date +%F).sql
```

## 2.2. 物理备份

### 2.2.1. 概念

物理备份就是利用命令（如cp、tar、scp等）直接将数据库的存储数据文件复制一份或多份，分别存放在其它目录，以达到备份的效果。

这种备份方式，由于在备份时数据库还会存在数据写入的情况，一定程度上会造成数据丢失的可能性。在进行数据恢复时，需要注意新安装的数据的目录路径、版本、配置等与原数据要保持高度一致，否则同样也会有问题。

所以，这种物理备份方式，常常需要在停机状态下进行，一般对实际生产中的数据库不太可取。因此，此方式比较适用于数据库物理迁移，这种场景下这种方式比较高效率。

物理备份的优点及使用场景

优点：速度快，效率高。

场景：可用于停机维护及数据库物理迁移场景中。

实际生产环境中，具体使用哪种方式，就需要看需求与应用场景所定。

## 2.3. 全量与增量备份

### 2.3.1. 相关链接

1. [高逼格企业级MySQL数据库备份方案，原来是这样....](https://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247489241&idx=1&sn=a2584937badf3977a8da064509f2b8a9&chksm=e91b73c5de6cfad32f3a86535e069e420e98a712d9e2199e007177c3535986d03cb098b07e45&scene=21#wechat_redirect)

# 3. 函数

## 3.1. 常用函数

`ifnull()`函数

`concat()`函数

`sum()`函数

`avg()`函数

`is null`关键字

`limit [3,5]`限制查询

`date(表中date字段)`提取日期

`year(表中date字段)`提取年

`current_time()`当前时间

`COUNT()` 计算表中的记录数（行数）

`MAX()` 求出表中任意列中数据的最大值

`MIN()` 求出表中任意列中数据的最小值

`group_concat()`将group by 后的显示出来

`union all`关键字和 `union`

`DATEDIFF` 两个日期间的差值

## 3.2. 窗口函数

  

## 3.3. limit

limit 2, 1 等于 limit 1 offset 2

跳过两条取一条,下标从1开始

## 3.4. 分区函数 Partition By

partition by 对连续的相同的内容分区

rank() over(partition by team order by age)

对连续的相同的团队划分区,然后排名

![](学习笔记/Attachments/1691886081160-8206c83e-17da-4d40-9f42-a6274871341b.png)

## 3.5. rank()与dense_rank()的区别

由以上的例子得出，rank()和dense_rank()都可以将并列第一名的都查找出来；但rank()是跳跃排序，有两个第一名时接下来是第三名；而dense_rank()是非跳跃排序，有两个第一名时接下来是第二名。

## 3.6. OVER

语法结构 OVER( [ PARTITION BY … ] [ ORDER BY … ] )

1 、partition by 字段名字A：子句进行分组，partition by是固定的分组语法；

2、order by 字段名字B：子句进行排序，order by 是固定的排序语法。

比如我们上面的例子就是用到了partition by classid 和 order by score这样的用法了，注意：如果联合使用指的意思是：先分组然后再排序

OVER()函数不能单独使用，必须跟在 排名函数（ ROW_NUMBER、DENSE_RANK、RANK、NTILE） 或 5种聚合函数（SUM、MAX、MIN、AVG、COUNT）后边。

## 3.7. 开窗函数

### 3.7.1. 排名开窗函数

语法结构：排名函数() OVER ( [ <partition_by字段> ] <order_by字段> )  
注意：在排名开窗函数中必须使用ORDER BY语句

|id|salary|rn_row|rn_rank|rn_dense_rank|rn_ntile|
|---|---|---|---|---|---|
|3|300|1|1|1|1|
|2|200|2|2|2|1|
|5|200|3|2|2|2|
|1|100|4|4|3|3|
|4|100|5|4|3|4|

#### 3.7.1.1. ROW_NUMBER()

`ROW_NUMBER()` 为结果集中的每一行分配一个唯一的序号，序号是根据 `OVER` 子句中的 `ORDER BY` 子句指定的顺序来分配的。如果两行或多行在 `ORDER BY` 子句中指定的列上有相同的值，那么这些行将收到不同的序号，因为 `ROW_NUMBER()` 保证序号的唯一性。

#### 3.7.1.2. RANK()

`RANK()` 也为结果集中的行分配一个序号，但这个序号是基于 `ORDER BY` 子句指定的列上的值来分配的。如果两行或多行在 `ORDER BY` 子句指定的列上有相同的值，那么这些行将收到相同的序号，并且下一个序号会跳过，以反映这些并列的排名。例如，如果有两行并列第一，则下一行的序号是3，而不是2。

#### 3.7.1.3. DENSE_RANK()

`DENSE_RANK()` 与 `RANK()` 类似，也是基于 `ORDER BY` 子句中的值来分配序号。不同之处在于，如果两行或多行并列，`DENSE_RANK()` 不会跳过任何序号。即，如果有两行并列第一，则下一行的序号是2，而不是像 `RANK()` 那样是3。

#### 3.7.1.4. NTILE()

### 3.7.2. 聚合开窗函数

语法结构：聚合函数( ) OVER ( [ partition by 字段] [order by 字段]) ，其中【partition by字段】和【order by 字段】是可选择的

#### 3.7.2.1. MAX()

#### 3.7.2.2. COUNT()

#### 3.7.2.3. SUM()

  

### 3.7.3. leetcode 相关题目

1. [分数排名](https://leetcode.cn/problems/rank-scores/description/)
2. [连续出现的数字](https://leetcode.cn/problems/consecutive-numbers/)
3. [查找重复的电子邮箱](https://leetcode.cn/problems/duplicate-emails/description/)
4. [部门工资最高的员工](https://leetcode.cn/problems/department-highest-salary/description/)

#### 3.7.3.1. 连续出现的数字

  

|num|rank1|rank2|rank1-rank2|
|---|---|---|---|
|1|1|1|0|
|1|2|2|0|
|1|3|3|0|
|1|5|4|1|
|1|6|5|1|
|1|7|6|1|

rank1-rank2 相等超过三个说明该数字有三个连续

## 3.8. 相关链接

1. [MySQL 8.0 的 5 个新特性，太实用了！(窗口函数)](https://zhuanlan.zhihu.com/p/373752457)
2. [SQL中OVER（PARTITION BY）详解](https://blog.csdn.net/weixin_45003816/article/details/103721121)

# 4. 数据库设计范式

### 4.1.1. 第一范式

实体类中的某一个属性不能有多个值或者不能有重复的属性

### 4.1.2. 第二范式

实体的属性必须完全依赖主键

### 4.1.3. 第三范式

不传递依赖于主键

# 5. 事务

## 5.1. 事务的特性

ACID

原子性 持久性 隔离性 一致性

## 5.2. 事务的隔离级别

|**隔离级别**|**解决问题**|**产生的问题**|
|---|---|---|
|read uncommitted||脏读|
|read committed|脏读|不可重复读|
|repeatable read|不可重复读|幻读|
|serializable|幻读|(串行) 效率低|

事务自动提交 set autocommit=0/1

开启事务

start transaction

insert 数据

提交 commit

回滚 rollback

savepoint 点名 事务保存点

rollback to savepoint 点名

删除保存点 release savepoint 点名

### 5.2.1. 查看数据库当前隔离级别

```
select @@transaction_isolation;
```

# 6. 用户管理

终端登录mysql`mysql -u"用户名" -p"密码" "数据库(可选)";`

root 用户创建用户`create "用户名"&"localhost" identified by "123";`

单前用户修改密码`set password =password("*******");`

删除用户 `drop user "用户名"@"localhost"`或者`delete from user where user="用户名"`

```
mysql -uroot -p
mysql -h 127.0.0.1 -p8080 -uroot -p123 database_name
```

```
set password = password("1234");
```

```
use mysql;
select user from user;
```

```
delete from user where user="要删除的用户";
-- 或者
drop user "要删除的用户" @"localhost" ;
```

# 7. SQL语言

## 7.1. 数据定义语言(DDL)

### 7.1.1. 概述

DDL全称是Data Definition Language，即数据定义语言，定义语言就是定义关系模式、删除关系、修改关系模式以及创建数据库中的各种对象，比如表、聚簇、索引、视图、函数、存储过程和触发器等等。

数据定义语言是由SQL语言集中负责数据结构定义与数据库对象定义的语言，并且由CREATE、ALTER、DROP和TRUNCATE四个语法组成。

`net start mysql` 开启sql服务

`net stop mysql` 关闭sql服务

```
-- 创建数据库
create database 数据库名;
-- 修改数据库的字符集
alter database 数据库的名称 character set utf8;
-- 删除数据库或者表
drop database(table) 数据库名;
```

`flush privileges;` 刷新数据库

`show create database/(table) name;` 展示创建数据库的语句

## 7.2. 数据操作语言(DML)

### 7.2.1. 概述

数据操纵语言全程是Data Manipulation Language，主要是进行插入元组、删除元组、修改元组的操作。主要有insert、update、delete语法组成。

## 7.3. 数据查询语言(DQL)

### 7.3.1. 概述

数据查询语言全称是Data Query Language，所以是用来进行数据库中数据的查询的，即最常用的select语句

### 7.3.2. 基本语句

表的创建与删除

```
-- 创建表,并且添加主键约束,和外键约束
create table 表名(
	`字段名` 类型名(大小) 约束,
	primary key(`字段名`),
	foreign key(`字段名`) references 表名(`字段名`)
)engine = innodb default charset = utf8
-- 删除表
drop table 表名;
-- 展示表的结构
desc(describe) 表名;
```

表结构的修改

```
-- 重命名表(数据库不重命名)
alter table `旧名字` rename `新名字`;
-- 字段类型的修改
alter table `表名` modify `字段名` 字段类型(int(10));
-- 字段名的修改(change) 
alter table  `表名` change  `字段名` `新名字` 字段类型(int(10));

-- 添加字段
alter table `表名` add `字段名` `字段类型` [first/last/after `字段名`];
-- 添加主键约束
alter table `表名` add primary key(字段);
```

`rename`: 可以用来修改表的名称

`**change**`:关键字可以修改表的字段的名称,能修改表的类型

`**modify**`:关键字可以修改表的字段类型,但不能修改字段的名称

表数据的增删查改

```
-- 插入语句
insert into `表名` values ("","",0),("","",0);
insert into `表名`(字段,字段,字段) values ("","",0),("","",0);

-- 查询语句
select * from `表名` where 条件表达式 group by 字段 having 条件表达式 order by 字段 [asc/desc];
-- 查询全部信息并去重
select distinct * from `表名`;

-- 更新语句
update `表名` set 字段=数值 where 条件表达式;

-- 删除语句
delete from `表名` where 条件表达式; -- delete 使用后自增索引可能会出问题
truncate table `表名`;  -- 删除表重新创建,清空全部数据
```

### 7.3.3. 基本语句的实战

表的创建

### 7.3.4. 1.创建学生表名为student，包含5个属性：

```
CREATE TABLE student(
sno char(5),
sname char(8),
sdept char(2) not null,
sclass char(2) not NULL,
sage numeric(2),
PRIMARY KEY(sno)
)engine=INNODB DEFAULT CHARSET=utf8
```

#### 7.3.4.1. 2.创建课程表：

```
create table course(
 cno char(3),
 cname char(16) UNIQUE,
 ccredit numeric(2),
 PRIMARY KEY(cno)
)engine = innodb default charset = utf8
```

#### 7.3.4.2. 3.创建分数表：

```
CREATE table score (
sno char(5),
cno char(3),
score NUMERIC(5,2),
PRIMARY KEY(sno,cno),
FOREIGN KEY(sno) REFERENCES student(sno),
FOREIGN KEY(cno) REFERENCES course(cno)
)engine = innodb default charset = utf8
```

#### 7.3.4.3. 4.给学生表的sdept添加索引：

```
alter table student add index(sdept);
```

#### 7.3.4.4. 5.给表student添加字段ssex性别字段并且默认为男：

```
alter table student add ssex char(1) default '男';
```

### 7.3.5. 插入数据

#### 7.3.5.1. 1.给表student插入数据：

```
insert into student VALUES("96001","马小燕","CS","01",21,"女"),
("96002","黎明","CS","01",18,"男"),
("96003","刘东明","MA","01",18,"男"),
("97001","马蓉","MA","02",19,"女"),
("97002","李成功","CS","01",20,"男"),
("97003","黎明","IS","03",19,"女"),
("97004","李丽","CS","02",19,"女"),
("96005","司马志明","CS","02",18,"男");
```

#### 7.3.5.2. 2.给表course插入数据：

```
INSERT INTO `course` VALUES ('001', '数学分析', 8),
 ('002', '普通物理', 8),
 ('003', '微机原理', 4),
 ('004', '数据结构', 4),
 ('005', '操作系统', 4),
 ('006', '数据库原理', 4),
 ('007', '编译原理', 3),
 ('008', '程序设计', 2);
```

#### 7.3.5.3. 3.给表score插入数据：

```
INSERT INTO `score` VALUES ('96001', '001', 77.00);
('96001', '003', 89.00),
('96001', '004', 86.00),
('96001', '005', 82.00),
('96002', '001', 88.00),
('96002', '003', 92.00),
('96002', '006', 90.00),
('96005', '004', 92.00),
('96005', '005', 90.00),
('96005', '006', 89.00);
```

### 7.3.6. 数据的查询

```
-- 求全体学生的学号、姓名、性别和年龄。
select sno,sname,ssex,sage from student;
-- 求选修了课程的学生学号。(应该是有分数的人才是有选修课的人)
select distinct sno from score;
-- 求年龄在19岁与22岁(含20岁和22岁)之间的学生的学号和年龄。
select sno,sage from student where sage >=19 and sage<=22;
select sno,sage from student where sage between 19 and 22;
-- 求各门课程的平均成绩与总成绩。(应该是有人选没人选都求出分数来)
select course.cname,ifnull(avg(score),0),ifnull(sum(score),0) sum from course left join score on course.cno=score.cno group by course.cno;
-- 求姓名是以“李”打头的学生。(模糊查询/正则表达式)
select sname from student where sname like '李%';
select sname from student where sname rlike '^李';
-- 求选修了课程名为 ’数据结构’ 的学生的学号和姓名。（分别用连接和子查询做）
select sno,student from student where sno in (select sno from score where score.con in (select cno from course where course.cname='数据结构'));
select student.sno,student.sname from student,score,course where student.sno=score.sno and score.cno=course.cno and course.cname='数据结构';
-- 求学生人数不足3人的系及其相应的学生数。
select sdept,count(*) as people from  student group by sdept having people<3;
```

#### 7.3.6.1. 查询数据库中表的列表

```
# 查询课程表
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'test' and TABLE_NAME regexp '^c[0-9]{6}';
```

### 7.3.7. 数据的更新

```
update 表名 set where 条件表达式;
```

### 7.3.8. 数据的删除

```
drop table 表名;
```

### 7.3.9. 创建视图并操作

创建视图`create view 视图名称 as (select * from 表名);`

```
--  (建立视图)  建立计算机系的学生的视图STUDENT_CS。
create view STUDENT_CS as select * from student;
-- (建立视图)  建立由学号和平均成绩两个字段的视图STUDENT_GR。
select student.sno,ifnull(avg(score),0) from student left join score on student.sno=score.sno group by student.sno;
--(视图查询)利用视图STUDENT_GR求平均成绩为88分以上的学生学号和平均成绩。
select * from STUDENT_GR where `avg(s.score)`>88;
--(视图更新)利视图STUDENT_CS，增加学生( ‘96006’, ‘张然’, ‘CS’, ‘02’, ‘男’, 19 )。
insert into STUDENT_GB(sno, sname, sdept, sclass, sage, ssex) values ("96006","张然", "CS", "02", 19, "男");
--（视图更新)利用视图STUDENT_CS，将学生年龄增加1岁。观察其结果。
update student_gb set sage=sage+1;
```

  

## 7.4. 数据控制语言(DCL)

### 7.4.1. 概述

数据控制语言：Data Control Language。用来授权或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，能够对数据库进行监视。

比如常见的授权、取消授权、回滚、提交等等操作。

## 7.5. 相关链接

1. [SQL语言：DDL、DML、DQL、DCL详解](https://zhuanlan.zhihu.com/p/391552199)

# 8. mysql相关链接

1. [最新、最全、最详细的 MySQL 数据库学习笔记总结（2021最新版）](https://zhuanlan.zhihu.com/p/365792812)

# 9. mysql数据类型后面的长度

你问得非常好！这个问题很多刚接触 MySQL 的人都会迷惑。我们分开来说：

---

### 9.1.1. `INT(M)` 或 `BIGINT(M)` 里括号的数字是什么意思

在 **MySQL 8.0 之前**，`INT(M)` 中的 `M` 叫做 **显示宽度（display width）**。

- 它并 **不影响存储大小，也不影响可存储的数值范围**。
- 它只是告诉客户端：当启用 `ZEROFILL` 时，显示数字时要用多少位来填充。

例子：

```
CREATE TABLE t (
  id INT(5) ZEROFILL
);
INSERT INTO t VALUES (42);
SELECT id FROM t;
```

输出会是：

```
00042
```

因为显示宽度是 5，就会在左边补零。

❗ 但是从 **MySQL 8.0.17 开始，整数类型的显示宽度已废弃**（除非有 `ZEROFILL`），所以 `INT(5)` 和 `INT` 没区别。

✅ **结论**：

- `INT(M)` 的 `M` 不影响能存多大数，也不影响性能。
- 如果没有 `ZEROFILL`，几乎可以忽略这个数字。

---

### 9.1.2. 2. 整数类型的大小到底由谁决定

是由数据类型本身决定的，而不是括号里的数字：

|类型|存储大小|可存储范围 (有符号)|
|---|---|---|
|TINYINT|1 字节|-128 ~ 127|
|SMALLINT|2 字节|-32768 ~ 32767|
|MEDIUMINT|3 字节|-8388608 ~ 8388607|
|INT/INTEGER|4 字节|-2147483648 ~ 2147483647|
|BIGINT|8 字节|-2^63 ~ 2^63-1|

---

### 9.1.3. 3. `VARCHAR(n)` 里的 `n` 是什么意思

`VARCHAR(n)` 中的 `n` 表示 **最大能存多少字符**，而不是字节数（⚠️ 但要考虑字符集）。

- 如果表使用 `utf8mb4` 字符集，一个字符最多占 4 个字节。
- 所以 `VARCHAR(1)` 允许你存 **一个字符**，可能占 1~4 个字节。

例子：

```
CREATE TABLE t (
  c1 VARCHAR(1) CHARACTER SET utf8mb4
);
INSERT INTO t VALUES ('a');   -- OK
INSERT INTO t VALUES ('😊'); -- OK，因为是 1 个字符
INSERT INTO t VALUES ('ab'); -- ERROR，因为超过 1 个字符
```

---

### 9.1.4. 4. 常见误区

- **误区 1：**`INT(1)` 范围只有 0-9。 ❌ 错误！范围还是 -2147483648 ~ 2147483647。
- **误区 2：**`VARCHAR(1)` 就只能占 1 个字节。 ❌ 错误！它是 1 个字符，不是 1 字节。

---

要不要我给你做一个对比表，把 `CHAR(n)`、`VARCHAR(n)`、`TEXT` 这些类型的存储差别和性能特点也列出来？这样你以后设计表会更有底气。

非常值得整理！👍 这块很多人设计表的时候容易乱用，导致性能不佳或者浪费空间。我给你做一份「速查表 + 设计建议」：

---

## 9.1. 📌 CHAR vs VARCHAR vs TEXT 对比

|特性|**CHAR(n)**|**VARCHAR(n)**|**TEXT**|
|---|---|---|---|
|**存储长度**|固定长度，总是占 `n`<br><br>个字符（不足右侧填充空格）|变长，实际存多少占多少，额外 +1或2字节记录长度|变长，额外 2字节记录长度|
|**最大长度**|最多 255 字符|0 ~ 65535 字符（受行大小限制）|4 种：TINYTEXT(255)、TEXT(65k)、MEDIUMTEXT(16M)、LONGTEXT(4GB)|
|**存储位置**|直接存在行内|直接存在行内|可能溢出行外（特别是 InnoDB），只在行里存一个指针|
|**性能特点**|查询速度快（定长字段便于对齐）|空间利用率高|适合存很大文本，但排序/索引效率低|
|**自动填充空格**|是（取出会自动去掉右侧空格）|否|否|
|**索引支持**|全字段可索引|全字段可索引|必须指定前缀长度索引，例如 `INDEX(content(100))`|

---

## 9.2. 📖 设计建议

✅ **推荐用法**：

1. **用** `**CHAR(n)**`

- 存储长度完全一致、固定的值，例如：

- 国家代码（`CHAR(2)`，如 "US", "CN"）
- MD5 哈希值（`CHAR(32)`）
- 性别字段（`CHAR(1)`，'M' 或 'F'）

- 优点：查询快，占用空间可预测，行对齐效率高。

2. **用** `**VARCHAR(n)**`

- 绝大多数普通文本字段都适合用 `VARCHAR`，例如：

- 用户名、邮箱、地址、备注
- 文章标题、分类名

- 优点：节省空间，灵活，支持索引。

3. **用** `**TEXT**`

- 当你可能存特别大的文本时：

- 博客文章正文
- 评论、描述
- 富文本、JSON

- 注意：

- `TEXT` 不能有默认值（MySQL 限制）
- 大字段可能导致行溢出，影响性能
- 排序和索引可能需要指定前缀长度

---

## 9.3. 🏆 实战建议

- **能用** `**VARCHAR**` **就不要用** `**TEXT**`，因为 `TEXT` 有行外存储和索引限制。
- **能用定长就用** `**CHAR**`，特别是存哈希、编码等，能提升查询性能。
- **不要滥用大长度** `**VARCHAR(65535)**`，这会占用额外字节去存长度，而且会浪费空间。

---

要不要我给你画一张小图，直观展示 CHAR/VARCHAR/TEXT 在存储引擎里占用的空间和结构？这样你对行内 vs 行外存储的区别会更有感知。

### 9.3.1. 修改表中的外键约束

1. 删除外键
2. 添加新的外键

删除外键需要知道外键名称，可以通过show create table xx;来查看建表语句中默认设置的约束名称。

```
-- 通过 show create 查看建表语句，查看默认的外键的约束名称
show create table sys_code_value;
-- 删除约束
alter table sys_code_value drop foreign key `sys_code_value_ibfk_1`;
-- 创建外键约束，前提表中没有孤儿数据，可以通过select * from cur where code_type not in (select code_type from target);
alter table sys_code_value add constraint `code_type_fk_1` FOREIGN KEY (code_type) REFERENCES sys_code_type (code_type);
-- 要修改外键的字段类型以及字段名称，先删除外键约束
alter table sys_code_value drop foreign key `code_type_fk_1`;
-- 更改字段名称
alter table sys_code_value change type_id code_type char(36);
-- 更改数据类型
alter table sys_code_value modify code_type varchar(50) not null comment "code_type";

```

是的，数据库授权可以设置时间限制。不同的数据库系统有不同的实现方式：

## 9.4. MySQL

### 9.4.1. 1. 创建有时限的用户

```
-- 创建用户，密码30天后过期
CREATE USER 'username'@'localhost' 
IDENTIFIED BY 'password' 
PASSWORD EXPIRE INTERVAL 30 DAY;

-- 授予权限并设置有效期
GRANT SELECT, INSERT ON database.table 
TO 'username'@'localhost' 
WITH GRANT OPTION 
VALID UNTIL '2025-12-31 23:59:59';
```

### 9.4.2. 2. 设置密码过期策略

```
-- 设置密码90天后过期
ALTER USER 'username'@'localhost' 
PASSWORD EXPIRE INTERVAL 90 DAY;
```

### 9.4.3. 3. 使用 MySQL 8.0 的密码策略

```
-- 设置密码策略
SET GLOBAL default_password_lifetime = 90; -- 全局默认90天

-- 或者针对特定用户
ALTER USER 'username'@'localhost' 
PASSWORD EXPIRE INTERVAL 90 DAY;
```

## 9.5. PostgreSQL

### 9.5.1. 1. 创建有时限的用户

```
-- 创建用户，设置有效期到指定日期
CREATE USER temp_user 
VALID UNTIL '2025-12-31 23:59:59';

-- 授予权限
GRANT SELECT ON table_name TO temp_user;
```

## 9.6. SQL Server

### 9.6.1. 1. 创建有期限的登录

```
-- 创建登录，设置过期时间
CREATE LOGIN temp_user 
WITH PASSWORD = 'password',
CHECK_EXPIRATION = ON,
CHECK_POLICY = ON;

-- 设置密码过期策略
ALTER LOGIN temp_user 
WITH PASSWORD = 'new_password' 
MUST_CHANGE, 
CHECK_EXPIRATION = ON;
```

## 9.7. Oracle

### 9.7.1. 1. 创建有期限的用户

```
-- 创建用户并设置密码有效期
CREATE USER temp_user IDENTIFIED BY password
PASSWORD EXPIRE;

-- 或者设置具体的过期时间
ALTER PROFILE user_profile 
LIMIT PASSWORD_LIFE_TIME 60; -- 60天过期
```

## 9.8. Redis

### 9.8.1. 1. 设置键的过期时间

```
-- 设置键值对，30分钟后过期
SET user:permission:123 "read_only" EX 1800
```

## 9.9. 通用最佳实践

### 9.9.1. 1. 定期审计脚本

```
-- 检查即将过期的用户
SELECT user, host, password_last_changed, 
       password_lifetime 
FROM mysql.user 
WHERE password_lifetime IS NOT NULL;
```

### 9.9.2. 2. 自动化过期处理

# 10. 隔离级别

## 10.1. 隔离级别 & 事务可见性

|隔离级别|主要特性|可能读现象|典型查询示例|
|---|---|---|---|
|`READ UNCOMMITTED`|最低隔离，**不做任何加锁**|_脏读_、_不可重复读_、_幻读_|`SELECT …`|
|`READ COMMITTED`|每条语句只看已提交的事务|_不可重复读_、_幻读_|`SELECT …` (单条语句)|
|`REPEATABLE READ`|默认级别，使用 **MVCC + next‑key lock** 防止幻读|_不发生幻读_|`SELECT … FOR UPDATE` / `SELECT …` (WITH SNAPSHOT)|
|`SERIALIZABLE`|最高级别，使用 **行锁 + gap lock** 等保证完全串行|无幻读、无脏读|`SELECT … LOCK IN SHARE MODE` / `SELECT … FOR UPDATE`|

**MVCC**（多版本并发控制）保持历史版本，读不加锁；写会加行锁。  
幻读是 _新行被插入导致相同范围查询返回更多行_ 的现象。

---

## 10.2. 什么叫脏读、不可重复读、幻读？

这三种“读异常”（_read anomalies_）是 **事务隔离级别** 的核心概念。  
在并发环境下，事务看到的数据如果不是自己 **提交前** 的版本，就会产生这些异常。

|异常|简单定义|触发条件|典型示例（MySQL）|
|---|---|---|---|
|**脏读 (Dirty Read)**|读取了**未提交**事务写入的数据。|事务 A 写数据 → 事务 B 读取 → 事务 A 回滚|A: `INSERT …`  <br>B: `SELECT …`（把 A 的插入值读到）|
|**不可重复读 (Non‑Repeatable Read)**|同一事务中多次读取同一行，**读到不同值**（因为另一个事务修改并提交）。|事务 A 读 → 事务 B 更新相同行并提交 → 事务 A 再读|A: `SELECT …`  <br>B: `UPDATE …`  <br>A: `SELECT …`|
|**幻读 (Phantom Read)**|同一事务多次查询同一范围，**出现/消失了行**（因为另一个事务插入或删除了满足条件的行）。|事务 A 读 range → 事务 B 插入/删除行并提交 → 事务 A 再读|A: `SELECT * FROM t WHERE age>30`  <br>B: `INSERT…` or `DELETE …`  <br>A: `SELECT * FROM t WHERE age>30`|

---

### 10.2.1. 脏读 (Dirty Read)

|事务 A|事务 B|
|---|---|
|`START TRANSACTION;`  <br>`INSERT INTO t (id, val) VALUES (1, 'dirty');`|`START TRANSACTION;`  <br>`SELECT val FROM t WHERE id=1;`|
|(没有 `COMMIT`)|① 看到 `'dirty'`  <br>② 若 A `ROLLBACK`，B 的读取数据就是 **不存在** 的 **脏** 记录。|

**隔离级别**  
只有 `READ UNCOMMITTED`;  
其余级别（`READ COMMITTED`、`REPEATABLE READ`、`SERIALIZABLE`）**不会**出现脏读。

---

### 10.2.2. 不可重复读 (Non‑Repeatable Read)

|事务 A|事务 B|
|---|---|
|`START TRANSACTION;`  <br>`SELECT val FROM t WHERE id=1;`  <br>(假设值为 `'old'`)|`START TRANSACTION;`  <br>`UPDATE t SET val='new' WHERE id=1;`  <br>`COMMIT;`|
|再次 `SELECT val FROM t WHERE id=1;`  <br>结果为 `'new'` —— **第一次** 和 **第二次** 读取结果不同。||

**隔离级别**  
① `READ UNCOMMITTED` & `READ COMMITTED` 支持不可重复读。  
② `REPEATABLE READ` 和 `SERIALIZABLE` 能保证 **同事务内的查询结果不变**（使用 **快照** 或 **gap lock**）。

---

### 10.2.3. 幻读 (Phantom Read)

|事务 A|事务 B|
|---|---|
|`START TRANSACTION;`  <br>`SELECT COUNT(*) FROM t WHERE age > 30;`  <br>(假设为 5)|`START TRANSACTION;`  <br>`INSERT INTO t (id, age, val) VALUES (100, 35, 'new');`  <br>`COMMIT;`|
|再次 `SELECT COUNT(*) FROM t WHERE age > 30;`  <br>结果变为 6 —— 除了现有行，“**幻**”出现了新行。||

**隔离级别**  
① `READ UNCOMMITTED`、`READ COMMITTED`、`REPEATABLE READ` 都可能触发幻读。  
② 仅 `SERIALIZABLE` 使用 **gap lock** 防止新行插入，从而消除幻读。

---

### 10.2.4. 如何预防

|目标|方案|
|---|---|
|防止脏读|使用 `READ COMMITTED` 或更高级别|
|防止不可重复读|`REPEATABLE READ` 或 **使用快照读**（`SELECT … LOCK IN SHARE MODE`）|
|防止幻读|`SERIALIZABLE`，或在 `REPEATABLE READ` 下 **加显式行锁**（`SELECT … FOR UPDATE`）|

**注意**：

- `REPEATABLE READ` 默认已对**主键**等覆盖范围使用 **gap lock**，可以阻止 _幻读_，但如果你只用非聚集索引查询，还是可能出现幻读。
- 写入时尽可能使用 **最小范围的锁**：避免全表或长行锁。

---

### 10.2.5. **具体代码演示**

```
-- 假设存在表 t(id INT PK, val VARCHAR(20))
```

**脏读（READ UNCOMMITTED）**

```
-- 事务 A
START TRANSACTION;
INSERT INTO t VALUES (1,'bad');
-- 事务 B
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
START TRANSACTION;
SELECT val FROM t WHERE id=1;   -- 返回 'bad'
ROLLBACK;  -- A 回滚，B 读到的值不应该存在
```

**不可重复读（READ COMMITTED）**

```
-- 事务 A
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT val FROM t WHERE id=1;   -- 'old'
COMMIT;

-- 事务 B
START TRANSACTION;
UPDATE t SET val='new' WHERE id=1;
COMMIT;

-- 事务 A 再次查询
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT val FROM t WHERE id=1;   -- 'new' → 不可重复读
COMMIT;
```

**幻读（SERIALIZABLE）**

```
-- 事务 A
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
SELECT COUNT(*) FROM t WHERE age > 30;  -- 5
COMMIT;

-- 事务 B 插入新行
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  -- 也是为了演示
START TRANSACTION;
INSERT INTO t (id, age, val) VALUES (999, 35, 'new');
COMMIT;

-- 事务 A 再次查询
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
SELECT COUNT(*) FROM t WHERE age > 30;  -- 仍为 5，幻读被防止
COMMIT;
```

---

### 10.2.6. 小结

|异常|需要避免的隔离级别|典型防护手段|
|---|---|---|
|脏读|任何级别都可能|`READ COMMITTED` 起步|
|不可重复读|`READ UNCOMMITTED`、`READ COMMITTED`|`REPEATABLE READ` 或显式锁|
|幻读|所有级别 ≠ `SERIALIZABLE`|`SERIALIZABLE`，或使用显式行/范围锁|

**核心思路**：  
读异常的产生与 **数据修改** 的并发、**事务的隔离级别**、以及 **锁机制** 三者密切相关。  
根据业务需求权衡事务隔离（一致性 vs 并发），并在代码层面加锁或使用更高级别，才能获得预期的事务语义。

## 10.3. InnoDB 锁的种类

|锁类型|触发语句|保护范围|作用|典型场景|
|---|---|---|---|---|
|**共享锁 (S)**|`SELECT … LOCK IN SHARE MODE`|行|允许读，但不允许写|读并发|
|**排他锁 (X)**|`SELECT … FOR UPDATE`|行|阻止其他事务读写|更新前锁行|
|**记录锁 (Record Lock)**|写、行锁|行|只锁住记录|插入、更新|
|**Gap Lock**|等值查询（`=`）或排序|索引键值 _gap_|防止幻读|`SELECT … WHERE id = 5` (id 不存在)|
|**Next‑Key Lock**|**Gap Lock + Record Lock**|键区间起止点|防止插入冲突 & 幻读|任何范围查询|
|**Auto‑inc Lock**|插入自增主键|自增列|防止自增冲突|`INSERT … AUTO_INCREMENT`|
|**Table‑level Lock**|`LOCK TABLES …`|表|适用于DDL或极端并发|`CREATE TABLE`|

**Gap Lock + Record Lock** 组合得到 _Next‑Key Lock_，是 InnoDB 防止幻读的核心手段。

### 10.3.1. Gap vs Next‑Key

- **Gap Lock** 只锁住两键之间的 _空隙_，不锁实际记录。  
    示例：查询 `WHERE id = 5` 且 `id` 不存在 → 锁住 `(4,6)` 区间的 gap。
- **Next‑Key Lock** 其实是 _Gap Lock + Record Lock_ 的合体。  
    如果记录存在，锁住该记录 + 前一个键到该键的 gap，形成 _锁定区间_。

---

## 10.4. 索引的类型与对锁的影响

|索引类型|说明|对锁的影响|常见用法示例|
|---|---|---|---|
|**主键 (PRIMARY KEY)**|唯一、聚集索引|写操作会产生 **Record Lock** + **Gap Lock**（区间）|`PRIMARY KEY (id)`|
|**唯一索引 (UNIQUE)**|唯一、聚集或非聚集|写时会锁对应记录 + gap|`UNIQUE (email)`|
|**非聚集索引 (INDEX)**|仅存索引键|写时只锁 **记录+gap**，不锁聚集键|`INDEX (name)`|
|**全文索引 (FULLTEXT)**|适用于文本搜索|通过 _全文搜索引擎_ 进行，在 InnoDB 4.0 以后可使用|`FULLTEXT (content)`|
|**空间索引 (SPATIAL)**|用作地理位置|只影响空间字段|`SPATIAL INDEX (loc)`|
|**位图索引 (BITMAP)**|低占用空间，读多写少|适用于稀疏字段|—|
|**列式存储索引 (HASH, BRIN)**|仅在 PostgreSQL 或 MariaDB|对 InnoDB 无直接影响|—|

**聚集索引**：数据行存储在 B‑Tree 叶子节点。  
**非聚集索引**：仅存索引键加原表行键的指针。  
**锁粒度**：写主键索引会加 _锁到 key 值_，写非聚集索引只锁 _index entry_。

---

## 10.5. 4. 锁 + 索引 + 隔离级别的协作实例

假设有表 `orders`：

```
CREATE TABLE orders (
    order_id      BINARY(16) PRIMARY KEY      -- UUID 主键
  , customer_id   INT
  , status        VARCHAR(20)
  , updated_at    DATETIME(6) DEFAULT CURRENT_TIMESTAMP(6)
      ON UPDATE CURRENT_TIMESTAMP(6)
) ENGINE=InnoDB;
```

### 10.5.1. 4.1 幻读：`REPEATABLE READ` → **Next‑Key Lock**

```
-- 事务 A (在 REPEATABLE READ 下)
START TRANSACTION;
SELECT * FROM orders WHERE order_id = UNHEX('0000000000000000') /* 不存在 */
-- 产生：Gap Lock on (order_id < gap < next_key_start) + X lock

-- 事务 B (尝试插入相同主键)
INSERT INTO orders (order_id, customer_id, status) VALUES (UNHEX('0000000000000000'), 10, 'NEW');
-- B 会等待 A（因为 A 的 Next‑Key Lock 在同一 gap）

COMMIT;  -- A 提交后 B 可以继续
```

### 10.5.2. 4.2 自增主键的 **Auto‑inc Lock**

```
CREATE TABLE seq(
  id INT AUTO_INCREMENT PRIMARY KEY
) ENGINE=InnoDB;

INSERT INTO seq VALUES ();   -- 把锁加到 auto_inc lock
```

### 10.5.3. 4.3 Gap Lock 的“无痕”锁定

假设主键是 `INT` 并且 `id` 只插入到 1000 之后：

```
SELECT * FROM seq WHERE id = 5000;   -- id 不存在
-- 产生: Gap Lock on (5000-? )   (大范围未被覆盖的空隙)
```

此锁会阻止任何插入在 5000 之前出现新行。

---

## 10.6. 5. 性能贴士

|场景|建议|说明|
|---|---|---|
|**高并发插入**|1. 使用 **自增主键** 或基于时间的 UUID `<` （减少 gap 大小）  <br>2. 设定 `innodb_autoinc_lock_mode=2`|避免全局 auto‑inc 锁|
|**大批量更新**|1. 先 `UPDATE … WHERE … LIMIT …` 并加 `ROW_NUMBER()` 分批|避免一次性全表锁|
|**查询频繁出现幻读**|1. 把查询放在 `SERIALIZABLE` 或加 `FOR UPDATE`|需要额外性能成本|
|**复杂索引查询**|1. 只建立必要的索引|索引多了写操作要加更多锁|
|**UUID 主键**|1. 采用 `UUID()` 或 `UUID_TO_BIN()`|更好支持 B‑Tree 排序，减少冲突|
|**长事务**|1. 逻辑拆分为小事务|缩短锁持有时间|

---

## 10.7. 6. 小结

|模块|关键点|
|---|---|
|**隔离级别**|控制读的**可见性**，从脏读到完全串行。|
|**锁**|InnoDB 通过 **共享/排他行锁**、**Gap**、**Next‑Key**、**Auto‑inc**、**表锁** 等多层级保护数据一致性。|
|**索引**|聚集/非聚集决定锁的粒度；正确使用索引可让查询快且锁冲突少。|
|**UUID 主键**|随机会扩大 gap，导致 _gap lock_ 触发的并发瓶颈。|
|**性能调优**|适时分区、自增锁模式、分批操作、最小化索引，都是减少锁争用的常用手段。|

**操作提示**  
① 在查询前加 `EXPLAIN EXTENDED` 或 `SHOW WARNINGS` 可查看索引使用。  
② 可通过 `SHOW ENGINE INNODB STATUS\G` 检查当前锁等待情况。  
③ 生产环境建议在 `REPEATABLE READ` 下，必要时仅开启 `READ COMMITTED` 或 `SERIALIZABLE`。

祝你编码🚀，并发顺畅🌐！
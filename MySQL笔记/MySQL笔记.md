[toc]

# 这是我的MySQL笔记

## 安装各个版本的MySQL

- 在linux下

- 在windows下

## MySQL数据库的简单使用用和基础介绍

使用命令行窗口链接MySQL数据库
远程服务器访问

- mysql -u 用户名 -p密码 -h 主机IP -P 端口号
注意:这里的用户需要有远程连接的权限,后面会讲

> 参数解释:
1.-p后面接密码,中间没有空格
2.如果没有写-h 主机IP,则默认本机
3.如果没有写-p 端口,则默认访问3306端口
4.实际工作中,3306端口一般会修改

下面是如何使用MySQL的**示例**

```sql
create database db02;
use db02;
create table users(id int,name varchar(255),address varchar(255));
show tables;
insert into user values(1,"tom","北京")
select * from users;
```

---

数据库三层结构

1. 安装MySQL数据库就是一个在主机安装一个数据库管理系统DMBS(database manage system),这个系统可以管理多个数据库
2. 一个数据库可以创建多个表,以保存数据
3. 数据库管理系统(DBMS),数据库和表的关系如图所示

```mermaid
 graph LR
 A[客户端命令行终端-Dos或者--java中的JDBC]-->B[3306]
 B-->C[(DBMS)]
 C-->D[数据库DB01]
 D-->表1
 D-->表2
 C-->E[数据库DB02]
 E-->表3
 E-->表4
 ```

==数据中表的文件依旧是文件==

---

数据在数据库中的存储格式为:
列(column),行(row)
表的一行称为一个记录

## SQL语句分类

- DDL:数据定义语句;[create]
- DML:数据操作语句;[增删查改],[insert,update,delete等]
- DQL:数据查询语句;[select]
- [DCL](#id) :数据控制语句;[管理数据库],比如说grant,revoke等等

## 对数据库操作

- 删除数据库:

```sql
drop database 数据库名;
```

- 创建数据库

```sql
create database 数据库名;
//创建一个使用utf8指令集的数据库hsp_db02
create database hsp_db02 character set utf8;
//创建一个使用utf8字符集的并且带有校对规则(区分不区分大小写)的hsp_db03
create database hsp_db03 character set utf8 collate  utf8_general_ci;
```

查看mysql中支持的字符集和校对规则
mysql中每个字符集都会对应多个校对规则，是一对多的关系。比如utf8对应的collation有utf8_general_ci，utf8_bin，utf8_unicode_ci等。而且每个character set会有个默认的collation与之对应，当我们在创建数据库或者创建表时如果只指定character set，不指定collation，就会使用character set默认的collation。collation的命名是以对应的character set为开头，比如collation为utf8_general_ci，我们就知道这个collation对应的字符集是utf8。

如果想要知道电脑里支持哪些字符集

```sql


```

如果想知道支持哪些字符集和校对规则

```sql
show character set;//字符集
//返回结果第三列就是默认的校对规则
show collation;//校对规则
```

查看当前数据库的字符集和校对规则

```sql
show variables  like  '%character%';
show variables  like  '%collation%';
```

查看表和列的校对规则

```sql
show table status from db_name like '%table_name%' ;//表
show full columns from table_name;//列
```

[还不清楚点这里](https://www.cnblogs.com/cy0628/p/15026818.html)

- 展示出所有数据库

```sql
show databases;
```

- 显示数据库创建语句

```sql
show create database db_name;
```

- 展示表详细信息

```sql
desc table_name;
```

数据库删除语句

```sql
drop database [if exists] db_name;//if exists意思是如果存在
```

在给字段,表或者数据库取名时,最好加上反引号,否则在某中情况下会有错误

### 备份

1.0 备份数据库,这些需要在命令行下执行

```shell
mysqldump -u root -p123456 -d 数据库1,数据库2,数据库n > 文件名.sql
```

2.1 恢复备份的数据,在mysql中执行

```sql
source 保存路径 文件名.sql
```

2.2 第二中恢复方法,直接在MySQL可视化工具里面操作

1.1 备份数据表

```shell
mysqldump -u root -p123456 -d 数据库名 -t 表1,表2 > 文件名.sql
```

2.1恢复方法跟上面一样

这一段纸没了,所以缺少了一点

### DCL语句操作

- 查询数据库中有哪些用户

```sql
use mysql;
select user,host from user;
```

> 注:用户的添加修改删除等操作都是在root权限下操作的!

- 创建用户

```sql
create user 'user_name'@'host' identified by password;
```

- 删除用户
  
```sql
drop user 'user_name'@'host' ;
```

- 修改用户密码

```sql
alter user 'user_name'@'host' identified with mysql_native_password by 'new password';
```

- 修改用户名

```sql
rename user 'old_user_name'@'host' to 'new_user_name'@'host';
```

权限控制
|权限类型 |权限说明|
|:-:|:-:|
|All/All Privileges|代表全局或者全数据库对象级别的所有权限|
|Alter查询权限|
|insert|代表允许修改表结构的权限，但必须要求有create和insert权限配合,如果是rename表名，则要求有alter和drop原表， create和insert新表的权限|
|Alter routine |代表允许修改或者删除存储过程、函数的权限|
|Create |代表允许创建新的数据库和表的权限|
|Create routine|代表允许创建存储过程、函数的权限|
|Create tablespace |代表允许创建、修改、删除表空间和日志组的权限|
|Create temporary tables|代表允许创建临时表的权限|
|Create user|代表允许创建、修改、删除、重命名user的权限|
|Create view|代表允许创建视图的权限|
|Delete|允许执行delete操作|
|Drop |代表允许删除数据库、表、视图的权限，包括truncate table命令|
|Event|代表允许查询，创建，修改，删除MySQL事件|
|Execute|代表允许执行存储过程和函数的权限|
|File|代表允许在MySQL可以访问的目录进行读写磁盘文件操作，可使用的命令包括load data infile,select … into outfile,load file()函数|
|Grant option|代表是否允许此用户授权或者收回给其他用户你给予的权限,重新付给管理员的时候需要加上这个权限|
|Index|代表是否允许创建和删除索引|
|Insert|代表是否允许在表里插入数据，同时在执行analyze table,optimize table,repair table语句的时候也需要insert权限|
|Lock tables| 代表允许对拥有select权限的表进行锁定，以防止其他链接对此表的读或写Process 代表允许查看MySQL中的进程信息，比如执行show processlist, mysqladmin processlist, show engine等命令|
|Reference| 是在5.7.6版本之后引入，代表是否允许创建外键|
|Reload|代表允许执行flush命令，指明重新加载权限表到系统内存中，refresh命令代表关闭和重新开启日志文件并刷新所有的表|
|lication client|代表允许执行show master status,show slave status,show binary logs命令|
|Replication slave|代表允许slave主机通过此用户连接master以便建立主从复制关系|
|Select |允许执行select操作|
|Show databases|代表允许执行show databases命令查看所有的数据库名|
|Show view|代表允许执行show create view命令查看视图创建的语句|
|Shutdown|代表允许关闭数据库实例，执行语句包括mysqladmin shutdown|
|Super|代表允许执行一系列数据库管理命令，包括kill强制关闭某个连接命令， change master to创建复制关系命令，以及create/alter/drop server等命令|
|Trigger| 代表允许创建，删除，执行，显示触发器的权限|
|Update|允许执行update操作|
|Usage|是创建一个用户之后的默认权限，其本身代表连接登录权限。使用create user语句创建的用户，默认就拥有这个usage权限，但是除了能登录之外|

- 展示用户所拥有的权限

```sql
show grants for 'user_name'@'localhost';
```

- 给用户授予权限

```sql
grant 权限[,权限2] on 数据库名.表名 to 'user_name'@'host';
flush privileges;//刷新一下权限,否则不会生效
```

- 给用户撤销权限

```sql
revoke 权限1[,权限2] on 数据库名.表名 from 'user_name'@'host';
flush privileges;//刷新一下权限,否则不会生效
```

在授权时,可以用通配符`*`代替数据库名和表名.表示所有数据库和表
例如:

```sql
grant all on *.* to 'hxl'@'localhost';//表示给root用户提供所有数据库和所有表的所有权限
```

### 函数

- 字符串函数

|函数名|功能|
|:-:|:-:|
|concat(s1,s2,s3)|将s1,s2,s3拼接成一个字符串|
|lower(str)|将str变成全小写|
|upper(str)|讲str变成全大写|
|lpad(str,n,pad)|左填充,用字符串pad对str左填充,达到n个字符长度|
|rpad(str,n,pad)|右填充,用字符串pad对str右填充,达到n个字符长度|
|trim(str)|去掉头尾的空格|
|substring(str,start,len)|返回从str字符start位置起,len个长度的字符串|

- 数值函数

|函数名|功能|
|:-:|:-:|
|ceil(x)|向下取整|
|floor(x)|向下取整|
|mod(x,y)|返回x/y的模|
|rand()|返回0-1的随机数|
|round(x,y)|求参数x的四舍五入值,保留y位小数|

- 日期函数

|函数名|功能|
|:-:|:-:|
|curdate()|返回当前日期|
|curtime()|返回当前时间|
|now()|当前日期和时间|
|year(date)|返回指定date年份|
|month(date)|获取指定的date月份|
|day(date)|获取当前日期|
|||
|||
|||
|||
|||

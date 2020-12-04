## 第一章、了解SQL

### 1.1 数据库基础

***



#### 1.1.1 什么是数据库

有组织的数据存储容器（一个文件或者是一组文件）

***注意区别数据库软件***

#### 1.1.2 表

#### 1.1.3 列和数据类型

列：一个字段

数据类型：

#### 1.1.4 行

#### 1.1.5 主键

行中唯一表示自己的一列，用来表示一个特定的行，例如删除更新的时候寻找制定的行。

*能够唯一区分表中的一行*

*总是设定主键，方便管理操作*

```sql
主键：
任意相同的两行不具有相同的主键值
每个个行都必须有一个主键值，不能为NULL
主键可以绑定在多条列上，但是必须保证多条列的组合是唯一的
```

==主键的好习惯==

==不更新主键列==

==不重用主键值==

***



### 1.2 什么是SQL

***



1、s q l是一种结构化查询语言。

基本通用于各个数据库查询软件



***

## 第二章、MYSQL简介

### 2.1什么是MYSQL

一种数据库软件

### 2.2.1 客户机-服务机软件

客户机发送操作请求、服务机操作后返回操作结果

### 2.1.2 MYSQL版本

v4:增加Innodb 引擎，增加事物处理 ，增加全文搜索

v4.1函数库 子查询 增加

V5.  存储过程、触发器、游标、视图

## 第三章、使用MSQL

### 3.1 连接

主机名+端口+用户名+密码

### 3.2 选择数据库

```sql
use Database;
```



### 3.3了解数据库和表

```sql
show database;
show tables;
show columns from tables; 
describe tables; 显示列
show status;
show create database create tables;
show grants;
show errors show warnings;
```



## 第四章、检索数据

### 4.1 SELECT

必要条件决定语句：想要检索什么，从哪里检索

### 4.2检索单列

```sql
select name from user;
```

数据未排序

多条时。结束sql 用 ==分号== 单条不需要 最好加上

Sql 不分大小写

sql 忽略空格 所以可以换行

### 4.3检索多列

```sql
select age,name,sex from user;
```

逗号

### 4.4检索所有列

```sql
select * from user;
```

一般在确切知道需要的数据情况下，不使用通配符 降低性能

### 4.5检索不同行

```sql
select distinct age from user;
```



### 4.6限制结果

```sql
select name from user limit 5;
select name from user  limit 2,5;
select name from user limit 4 offset 3;
```



### 4.7使用完全限定的表名

```sql
select user.name from hahuabase.user;
```





## 第五章、排序检索数据

### 5.1排序数据

```sql
select name from user order by age; 
```

### 5.2多列排序

```sql
select name from user order by name,age,height;
```

### 5.3指定方向排序

```sql
select name from user order by age desc;
select name from user order by age desc,name;
```

==多个列排序时必须在每个列后面加上 desc==

```sql
select name from user order by age desc limit 1;//查询年龄最大值
```

==子句顺序位置 from->order by -> limit==



## 第六章、过滤数据

### 6.1 使用where子句

```sql
select name,age from user where age =2;
```

### 6.2 where 操作符

```
= > < != <= >= between
```

#### 6.2.1 检索单个值

```sql
select * from user where name="dyq"; //注意不区分大小写
```

#### 6.2.2 不匹配检查

```sql
select * from user where age <> 13;
```

#### 6.2.3 范围值检查

```sql
select * from user where age between 5 and 10;
```

#### 6.2.4 NULL 值检查

```sql
select * from user where height is null;
```



## 第七章、数据过滤

### 7.1 组合where子句

为了能够进行更强的过滤

#### 7.1.1  and 操作符

```sql
select * from user where name="xiaoming" and age =12;
```

#### 7.1.2 or 操作符

```sql
select * from user where name ="xiaoming" or name = "xiaoqiang";
```

#### 7.1.3 计算次序

And > or 

解决方案是加括号

### 7.2 IN 操作符

```sql
select * from user where name in("xiaoming","xiaoqiang");
```

### 7.3 NOT 操作符

```sql
select * from user where name not in("xiaoming");
```



## 第八章、用通配符进行过滤
































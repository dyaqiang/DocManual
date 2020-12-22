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

### 8.1 like 操作符

通配符：匹配值的一部分的特殊字符

搜索模式：

#### 8.1.1  %

```sql
select id, name from user where name like 'fish%';
```

~~~~sql
select id , name from user where name like '%fish%'
~~~~

````sql
select id , name from user where name like 'F%y';
````

#### 8.1.2  _

```sql
select id , name from user where name like '_fish';
```

#### 8.1.3 []

```sql
select id , name from user where name like '[JM]%'
```

```sql
select id ,name from user where name like '[^JM]%';
```

==能用其他方案代替的，尽量不使用通配符==

## 第九章、创建计算字段

#### 9.1 计算字段

#### 9.2 拼接字段

````sql
select name + '(' + age + ')' from user;  
````

````sql
select name || '(' || age || ')' from user;
````

==存疑==

##### 9.2.1使用别名

```sql
select name + '(' + age + ')' as nameage from user;  
```

#### 9.3 执行算术计算



## 第十章、数据处理函数

#### 10.1 函数

##### 10.1.1 文本处理函数

````sql
select name, age UPPER(name) as name from user 
````

##### 10.1.2 时间日期处理函数

```sql
select name, ctime where DATEPART(yy,ctime) = 2004;
```

##### 10.1.3 数值处理函数

```sql
select name,age where ABS(age) = 14;
```



## 第十一章、汇总数据

#### 11.1 聚集函数

行数、最大、最小、平均、和

```sql
select AVG(age) as aveage from user;
```

````sql
select count(*) as count_num from user;
````

```sql
select max(age) as max_age from user;
```

~~~sql
select min(age) as min_age from user;
~~~

```sql
select sum(age) as sum_age from user;
```

#### 11.2 聚集不同值

````sql
select distinct age from user;
````

#### 11.3 聚集函数的组合使用

````sql
select count(*) as num , min(age) as min_age ,max(age) as max_age from user 
````



## 第十二章、分组数据

#### 12.1 分组

````sql
select  sex ,count(*) as num_count from user group by sex ;
````

#### 12.2 过滤分组

```sql
select name ,count(*) as num_count from user group by age having count(*)>10;
```

#### 12.3 分组 排序

```sql
select age ,count(*) as num_count from user group by age having count(*)>10  order by num_count;
```



## 第十三章 、子查询

#### 13.1 子查询

#### 13.2 子查询-过滤

```sql
//查询买了某种物品的用户
select name, age from user where uid in( select uid from order where order.ordertype = 1)
```

#### 13.3 子查询-计算

```sql
//查询每个用户买了那些东西
select name ,age, (select count(*) from order where order.uid = user.uid ) as orders from user;
```



## 第十四章、联结表

#### 14.1 不同类型的联结

##### 14.1.1 自联结

```sql
select name ,age from user where name= (select name from user where age =12);
```

##### 14.1.2 自然联结

```sql
select A.* ,B.* from user as A,order as B where A.uid =B.uid;
```

##### 14.1.3 外部联结

````sql
select A.* ,B.* from user as A inner join order as B on A.uid = B.uid;//内联
````

```sql
select A.* ,B.* from user as A left outer join order as B on A.uid = B.uid;//外联
```



## 第十五章、组合查询

#### 15.1 union

````sql
select name ,age  from user where name in('xiaodong','xiaohong') union select name ,age from user where age =19;
````

````sql
select name ,age  from user where name in('xiaodong','xiaohong') union all select name ,age from user where age =19;   // 不去除去重复行
````

#### 15.2 排序

````sql
select name ,age  from user where name in('xiaodong','xiaohong') union select name ,age from user where age =19; order by sex;
````

## 第十六章、插入数据

#### 16.1 数据插入

##### 16.1.1 插入完整的行

常用：略

##### 16.1.2 插入部分行

常用：略

定义的时候容许为null、给出默认值

##### 16.1.3 插入检索到的行

````sql
insert into user2(age,name,sex,uid) select age ,name ,sex ,uid from user
````

#### 16.2 复制表

```sql
select * into user from user2;
```

## 第十七章、更新、删除

#### 17.1 更新数据

```sql
update user set age = null where uid = 123456;
```

#### 17.2  删除数据

```sql
delete from user where uid =123456;
```

==切记删除的时候带where==

## 第十八章、创建操作表

#### 18.1 创建表

两点：使用null 、指定默认值default

#### 18.2 更新表

1、可增加列

2、一般不容许删除更改列

3、一边容许重命名列

4、一般对有数据的列更改有限制

#### 18.3 删除表

#### 18.4 重命名表



## 第十九章、视图

#### 19.1 why 

1、虚拟的表，查询出来的结果进行包装。

2、重用sql、复杂sql 操作 、使用表的一部分、保护数据的权限、

3、大部分应用是简化了操作、见下面的分析

#### 19.2 创建视图

```sql
create view user_v1   as  select name  from user ; //创建后下面再用，把user_v1当表来用
```

##### 19.2.1 简化复杂联结

##### 19.2.2 重新格式化检索出来的数据

##### 19.2.3 用视图过滤不要的数据

##### 19.2.4 用视图计算字段

## 第二十章、存储过程

#### 20.1 what

为以后的使用而保存的一条或者多条sql 语句的集合

#### 20.2 why 

把处理封装到容易使用的单元中，

不需要反复的建立一些列的操作，更好的保证了数据的一致性。

简化变动的管理

存储过程通常编译存储，性能更高

即：简单、高效、安全

#### 20.3 执行存储过程

#### 20.4 创建存储过程

## 第二十一章、事物

#### 21.1 事物处理

#### 21.2 控制事物的处理

## 第二十三章 、使用游标

#### 23.1 游标

#### 23.2 使用游标

## 第二十四章、高级SQL

####  24.1约束

##### 24.1.1主建

##### 24.1.2 外建

##### 24.1.3 唯一约束

##### 24.1.4 检查约束

#### 24.2 索引

#### 24.3 触发器

#### 24.4 数据库安全














































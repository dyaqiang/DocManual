##### 1、安装mysql 

1、安装客户端程序

2、安装服务员端程序

##### 2、连接

```mysql
#建立连接
mysql -uroot -p123456; #本机省略host
mysql -hlocalhost -uroot -p123456;

#断开连接
exit;
quit;
\q;
```

##### 3、数据类型

###### 3.1 数值类型

| 类型                     | 占用的存储空间（单位：字节） | 无符号数取值范围 | 有符号数取值范围 | 含义           |
| ------------------------ | ---------------------------- | ---------------- | ---------------- | -------------- |
| `TINYINT`                | 1                            | 0 ~ 2⁸-1         | -2⁷ ~ 2⁷-1       | 非常小的整数   |
| `SMALLINT`               | 2                            | 0 ~ 2¹⁶-1        | -2¹⁵ ~ 2¹⁵-1     | 小的整数       |
| `MEDIUMINT`              | 3                            | 0 ~ 2²⁴-1        | -2²³ ~ 2²³-1     | 中等大小的整数 |
| `INT`（别名：`INTEGER`） | 4                            | 0 ~ 2³²-1        | -2³¹ ~ 2³¹-1     | 标准的整数     |
| `BIGINT`                 | 8                            | 0 ~ 2⁶⁴-1        | -2⁶³ ~ 2⁶³-1     | 大整数         |



###### 3.2浮点类型

| 类型     | 占用的存储空间（单位：字节） | 绝对值最小非0值          | 绝对值最大非0值          | 含义         |
| -------- | ---------------------------- | ------------------------ | ------------------------ | ------------ |
| `FLOAT`  | 4                            | ±1.175494351E-38         | ±3.402823466E+38         | 单精度浮点数 |
| `DOUBLE` | 8                            | ±2.2250738585072014E-308 | ±1.7976931348623157E+308 | 双精度浮点数 |

| 类型          | 取值范围         |
| ------------- | ---------------- |
| `FLOAT(4, 1)` | -999.9~999.9     |
| `FLOAT(5, 1)` | -9999.9~9999.9   |
| `FLOAT(6, 1)` | -99999.9~99999.9 |
| `FLOAT(4, 0)` | -9999~9999       |
| `FLOAT(4, 1)` | -999.9~999.9     |
| `FLOAT(4, 2)` | -99.99~99.99     |

更加精确的数据类型

| 类型            | 占用的存储空间（单位：字节） | 取值范围   |
| --------------- | ---------------------------- | ---------- |
| `DECIMAL(M, D)` | 取决于M和D                   | 取决于M和D |

```mysql
无符号数值类型 UNSIGNED
```



###### 3.3 日期时间

| 类型        | 存储空间要求 | 取值范围                                       | 含义         |
| ----------- | ------------ | ---------------------------------------------- | ------------ |
| `YEAR`      | 1字节        | 1901~2155                                      | 年份值       |
| `DATE`      | 3字节        | '1000-01-01' ~ '9999-12-31'                    | 日期值       |
| `TIME`      | 3字节        | '-838:59:59' ~ '838:59:59'                     | 时间值       |
| `DATETIME`  | 8字节        | '1000-01-01 00:00:00' ～ '9999-12-31 23:59:59' | 日期加时间值 |
| `TIMESTAMP` | 4字节        | '1970-01-01 00:00:01' ～ '2038-01-19 03:14:07' | 时间戳       |



##### 4、数据库基本操作

```sql
show databases;
create database duanyaqiang;
create database if not exists duanyaqiang;
drop database duanyaqiang;
drop database if exists duanyaqiang;
```

##### 5、表的基本操作

```mysql
show tables;
create table first_table(name varchar(100),age int(10))COMMENT"第一张表";
desc first_table;
create table if not exists first_table(name varchar(100),age int(10))COMMENT"第一张表";
drop table;

#修改
alter table first_table rename to fisrt_list;
#转移库
ALTER TABLE first_table1 RENAME TO dahaizi.first_table1;
#添加列
alter table fisrt_list add column sex varchar(25);
#删除列
alter table fisrt_list drop sex;
#修改列
ALTER TABLE first_table MODIFY second_column1 VARCHAR(2) FIRST;
```

##### 6、列的属性

```mysql
#没有显式指定的列的值将被设置为NULL，NULL的意思就是此列的值尚不确定
NULL 
#NULL的含义是这个列的值还没有被设置。如果我们不想让默认值为NULL，而是设置成某个有意义的值，可以在定义列的时候给该列增加一个DEFAULT属性
DEFAULT 
#需要要求表中的某些列中必须有值，不能存放NULL，那么可以用这样的语法来定义这个列
NOT NULL
```

###### 主键

```mysql
#单个
CREATE TABLE student_info (
    number INT PRIMARY KEY,
    name VARCHAR(5),
    sex ENUM('男', '女'),
    id_number CHAR(18),
    department VARCHAR(30),
    major VARCHAR(30),
    enrollment_time DATE
);

#组合的形式单独声明
CREATE TABLE student_score (
    number INT,
    subject VARCHAR(30),
    score TINYINT,
    PRIMARY KEY (number, subject)
);
```

###### UNIQUE 唯一

````mysql
CREATE TABLE student_info (
    number INT PRIMARY KEY,
    name VARCHAR(5),
    sex ENUM('男', '女'),
    id_number CHAR(18) UNIQUE,
    department VARCHAR(30),
    major VARCHAR(30),
    enrollment_time DATE
);
#组合
CREATE TABLE student_info (
    number INT PRIMARY KEY,
    name VARCHAR(5),
    sex ENUM('男', '女'),
    id_number CHAR(18),
    department VARCHAR(30),
    major VARCHAR(30),
    enrollment_time DATE,
    UNIQUE KEY uk_id_number (id_number)
);
````

###### 主键和`UNIQUE`约束的区别

>   主键和`UNIQUE`约束都能保证某个列或者列组合的唯一性，但是：
>
> 1. 一张表中只能定义一个主键，却可以定义多个`UNIQUE`约束！
> 2. 规定：主键列不允许存放NULL，而声明了`UNIQUE`属性的列可以存放`NULL`，而且`NULL`可以重复地出现在多条记录中！
>
> 小贴士： 一个表的某个列声明了UNIQUE属性，那这个列的值不就不可以重复了么，为啥NULL这么特殊？哈哈，NULL就是这么特殊。NULL其实并不是一个值，它代表不确定，我们平常说某个列的值为NULL，意味着这一列的值尚未被填入。



###### 外键

```mysql
CREATE TABLE student_score (
    number INT,
    subject VARCHAR(30),
    score TINYINT,
    PRIMARY KEY (number, subject),
    CONSTRAINT FOREIGN KEY(number) REFERENCES student_info(number)
);
```

###### AUTO_INCREMENT

> 1. 一个表中最多有一个具有AUTO_INCREMENT属性的列。
> 2. 具有AUTO_INCREMENT属性的列必须建立索引。主键和具有`UNIQUE`属性的列会自动建立索引。不过至于什么是索引，在学习MySQL进阶的时候才会介绍。
> 3. 拥有AUTO_INCREMENT属性的列就不能再通过指定DEFAULT属性来指定默认值。
> 4. 一般拥有AUTO_INCREMENT属性的列都是作为主键的属性，来自动生成唯一标识一条记录的主键值。

###### 列的注释

```sql
CREATE TABLE first_table (
    id int UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '自增主键',
    first_column INT COMMENT '第一列',
    second_column VARCHAR(100) DEFAULT 'abc' COMMENT '第二列'
) COMMENT '第一个表';
```

###### ZEROFILL

一般不用

###### 列的属性

```html
mysql> DESC student_info;
+-----------------+-------------------+------+-----+---------+-------+
| Field           | Type              | Null | Key | Default | Extra |
+-----------------+-------------------+------+-----+---------+-------+
| number          | int(11)           | NO   | PRI | NULL    |       |
| name            | varchar(5)        | YES  |     | NULL    |       |
| sex             | enum('男','女')   | YES  |     | NULL    |       |
| id_number       | char(18)          | YES  | UNI | NULL    |       |
| department      | varchar(30)       | YES  |     | NULL    |       |
| major           | varchar(30)       | YES  |     | NULL    |       |
| enrollment_time | date              | YES  |     | NULL    |       |
+-----------------+-------------------+------+-----+---------+-------+
7 rows in set (0.00 sec)
```



##### 7、简单查询

###### As 别名

 ###### distinct 排重

```mysql
select distinct student_id from student;
select distinct student_id ,name from student;
```

###### limit 限制条数

````mysql
select * from student limit 1，5;
````

###### DESC ASC

按照某个列进行排序

```sql
 select * from student order by student_id desc;
```

##### 8、带搜索条件的查询

```mysql
=
between and
in
not in
# 匹配NULL值
IS NULL
IS NOT NULL

#多条件
and
or

#通配符
like 
not like
```

##### 9、表达式和函数

```

```



##### 10、分组查询

```mysql
#分组
select  oaid ,AVG(oaid) from shebei  group by cdid;
#where分组
select AVG(oaid) from shebei where oaid>1 group by cdid;
#having
select AVG(oaid) from shebei  group by cdid having AVG(oaid)>2;
#分组+排序
select AVG(oaid) as avgs from shebei  group by cdid having AVG(oaid)>2 order by avgs desc;
#顺序
SELECT [DISTINCT] 查询列表
[FROM 表名]
[WHERE 布尔表达式]
[GROUP BY 分组列表 ]
[HAVING 分组过滤条件]
[ORDER BY 排序列表]
[LIMIT 开始行, 限制条数]
```



##### 11、子查询

```mysql
多表查询，一般能不搞就不搞 否则效率不高。
```

##### 12、组合查询

##### 13、数据插入更新

##### 14、视图

简单理解是查询语句的别名

##### 15、自定义变量

##### 16、存储过程 

简单理解就是对一系列函数的操作的集合

##### 17、游标

使用游标查询我们所在记录的行

##### 18、触发器

我们在对表中的记录做增、删、改操作前和后都可能需要让`MySQL`服务器自动执行一些额外的语句，这个就是所谓的`触发器`的应用场景。


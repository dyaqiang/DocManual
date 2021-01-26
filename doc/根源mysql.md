#### 1、重新理解mysql

##### 1.1 客户端、服务端

>```
>MySQL`服务器进程的默认名称为`mysqld`， 而我们常用的`MySQL`客户端进程的默认名称为`mysql
>```

##### 1.2 启动客户端、服务端

##### 1.3 客户端和服务端建立连接

> 通信方式一：TCP|IP

* TCP|IP
* 端口号：mysqld -P3307
* 连接：mysql -h127.0.0.1 -uroot -P3307 -p

> 通信方式二：Unix域套接字文件

* 连接：mysql -hlocalhost -uroot --socket=/tmp/a.txt -p

##### 1.4 服务器处理客户端请求流程

> 流程图：客户端->处理连接->查询缓存->语法解析->查询优化->存储引擎->文件系统

###### 1.4.1 连接管理

连接包含三种类型：TCP/IP 、命名管道共享内存、Unix套接字

> *认证：携带主机信息、用户名、密码 到服务端进行认证*

> *交互：客户端选择一种连接方式建立连接，客户端进程连接到服务端进程的时候，服务端的进程会建立一个线程，来处理该连接的交互行为，当客户端断开的时候并不会理解销毁这个线程，而是把他缓存起来等待其他的客户端的连接，一旦建立了连接服务端会一直监听客户端的请求，获取到的消息是一个文本，进入下一步的解析*

> *安全：s s l*

###### 1.4.2 解析和优化

包含三部分：查询缓存、语法解析、查询优化

* 查询缓存

  > *缓存：之前查询过就不再进行查询，直接命中缓存返回结果*
  >
  > *不缓存：查询语句的符号一点不一样也不会走缓存、系统函数一般不走缓存、有修改表结构的不走缓存（update 、insert）*
  >
  > *删除：从 MySQL 5.7.20开始，不推荐使用查询缓存，并在MySQL 8.0中删除*

* 语法解析

  > *对客户端发送的文本进行解析，满足服务端可识别的程序，可以简单理解为编译*

* 查询优化

  > 我们写的sql 并不是最好的查询效果，用那些索引，怎么连接等、这个时候会进行优化生成一个==执行计划==，我们可以用==explain==查看这个执行计划

###### 1.4.3 存储引擎

 *前面的操作还咩有操作到真实的数据呢，如何把数据存储到物理层，和从物理层提取数据， 这个事情就是存储引擎负责的，他会提供一个底层的API供上层调用 （行数据还是我们看到的表现层的逻辑）*

##### 1.5 存储引擎

###### 常见的存储引擎

1. InnoDB
2. MyISAM
3. MEMORY

###### 常用操作

```mysql
SHOW ENGINES;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)

#suport 是否支持
#comment 注释
#Transactions 是否支持事物
#XA 是否支持分布式事务
#Savepoints 是否支持事物回滚
```

###### 创建表时创建不同的存储引擎

```mysql
#创建存储引擎
CREATE TABLE 表名(
    建表语句;
) ENGINE = 存储引擎名称;

#修改存储引擎
ALTER TABLE 表名 ENGINE = 存储引擎名称;
```



#### 2、启动选项和配置文件

*阐述：mysql 的客户端和服务端程序有很多的配置项，服务端如：和客户端的连接数、和客户端的通信方式、表的存储引擎。 客户端如：host 、端口号、用户名、密码*

> 配置中总会有些默认值、连接数默认151、存储引擎默认innodb

##### 2.1启动项的命令行的常见命令配置：

| 长形式       | 短形式 | 含义     |
| ------------ | ------ | -------- |
| `--host`     | `-h`   | 主机名   |
| `--user`     | `-u`   | 用户名   |
| `--password` | `-p`   | 密码     |
| `--port`     | `-P`   | 端口     |
| `--version`  | `-V`   | 版本信息 |

##### 2.2配置文件的配置

###### 2.21 配置文件的路径

###### 2.22 配置文件的内容

###### 2.22 配置文件的配置优先级

##### 2.3 系统变量

*会影响程序运行的一些变量，系统变量==VARIABLES==*

```mysql
#查看默认存储引擎值
SHOW VARIABLES LIKE 'default_storage_engine';
#查询容许客户端最大连接数值
SHOW VARIABLES like 'max_connections';
#查询defalut开头的系统变量值
SHOW VARIABLES LIKE 'default%';
```

###### 2.3.1 设置系统变量

*1、命令行启动选项中 2、配置文件中  3、服务器运行过程中也可以配置  4、系统变量也可以设置作用范围*

##### 2.4 状态变量

*为了了解服务的运行状态，服务器会自己设置一些状态变量==STATUS==，*

```mysql
SHOW STATUS LIKE 'thread%';
# Threads_connected :当前有多少客户端和服务端连接
# Handler_update 更新了多少条记录了
```



#### 3、字符集、比较规则

> ***字符集简介****：计算机只能识别二进制、那么我们的数据怎么存储到计算机，需要解决两个事情：1、那些数据映射成二进制，就是一个字典查看，2、怎么映射：当然是编码和解码*。
>
> ***比较规则****：怎么比较大小？全部转换为二进制进行比较，大小写怎么办？单独处理呗，怎么处理就会有什么样的比较效果*
>
> ***常见的编码方式：*** *ASCII、ISO 8859-1、GB2312、GBK、utf8*

##### 3.1 mysql中的字符集和比较规则

> *utf8mb3: 阉割过的utf8字符集，只使用1～3个字节表示字符（在MySQL中utf8是utf8mb3的别名）*
>
> *utf8mb4：正宗的utf8字符集，使用1～4个字节表示字符*
>
> *所以在使用emoji 表情的时候 我们必须选择utf8mb4，不能直接使用utf8 🐴* 

###### 3.1.1 字符集比较规则

```mysql
#查看支持的字符集--CHARSET
SHOW CHARSET;
#查看比较规则--COLLATION
SHOW COLLATION LIKE 'utf8\_%';
#utf8 默认的比较规则：utf8_general_ci
```

##### 3.2 字符集比较规则的应用

*有级别的概念：服务器、数据库、表级别、列级别*

###### 3.2.1 服务器

```mysql
#字符集
SHOW VARIABLES LIKE 'character_set_server';
#比较规则
SHOW VARIABLES LIKE 'collation_server';
#服务器级别默认的字符集是utf8，默认的比较规则是utf8_general_ci。

#启动创建时配置
[server]
character_set_server=gbk
collation_server=gbk_chinese_ci
```

###### 3.2.1 数据库

```mysql
#数据库级别的设置
mysql> CREATE DATABASE charset_demo_db
    -> CHARACTER SET gb2312
    -> COLLATE gb2312_chinese_ci;
#字符集    
SHOW VARIABLES LIKE 'character_set_database';    
#比较规则
SHOW VARIABLES LIKE 'collation_database';
```

###### 3.2.3 表级别

```mysql
#表级别设置设置
mysql> CREATE TABLE t(
    ->     col VARCHAR(10)
    -> ) CHARACTER SET utf8 COLLATE utf8_general_ci;
```

###### 3.2.3 列级别

```mysql
CREATE TABLE 表名(
    列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称],
    其他列...
);
ALTER TABLE 表名 MODIFY 列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称];
#一般是默认的
```

##### 3.3 客户端服务端通信时的字符集

*当客户端的编码字符集和服务端的解码字符集不一致的时候怎么办呢？*

###### 3.3.1 字符集转换

> 客户端：发送请求使用的字符集.   ( linux: utf8、 win：gbk. 软件navicat、phpstrom 会自定义字符集)
>
> 服务端：接受解码，认为来的是character_set_client的字符 ： 会进行编码为character_set_connection的字符
>
> 数据库查找：character_set_connection这个编码的字符进行查找这种编码的列。
>
> 返回：character_set_results这个列编码的方式进行返回。
>
> 客户端使用的是： character_set_results编码的方式，就能读懂了

*通常都把 ***character_set_client*** 、***character_set_connection***、***character_set_results*** 这三个系统变量设置成和客户端使用的字符集一致的情况，这样减少了很多无谓的字符集转换

###### 3.3.2 比较规则



#### 4、Innodb   记录行结构

*疑惑：客户端发送请求服务端返回结果，表的数据存在哪里？什么格式存放？怎么访问这些数据？*

##### 4.1 Innodb 页

>  *1、我们把数据存储在磁盘上的时候是用存储引擎，断电后还存在*。  *2、处理数据是在内存中进行的，所以处理的时候需要先加载数据进内存*。  *3、我们知道内存处理比加载进内存要快几个数量级，每次操作数据都加载所有数据进入内存的话，那么十分影响效率*。 *4、所以进行以页为单位进行内存加载*。 *5、Innodb 每次内存加载是16kb,内存数据刷到磁盘也是16kb*。

##### 4.2 Innodb 行

*记录在磁盘上的存储方式就是行记录-行格式*

###### COMPACT行格式

* 额外信息

  1. 变长字段长度列表

     记录那些varchar的字段具体占用的长度是多少，即真正的长度和占用的字节数

  2. NULL值列表

  3. 记录头信息

* 真实数据

  1. 我们自己定义的列
  2. 隐藏列 row_id、transaction_id、roll_pointer

  

  

  

#### 5 、Innodb  数据页结构

*记录在磁盘上的存储方式就是行记录-行格式*










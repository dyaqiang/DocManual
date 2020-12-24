#### 1、初识

##### 认识下Redis

1. Redis 是一种基于键值对的NOSQL数据库，
2. 区别于其他数据库的是他的value 可以是 string 、hash 、list、set、zset、bitmaps、hyperloglog、CEO、多种数据结构和算法，应用场景很多
3. 操作全在内存中，性能极高
4. 快照日志存储到硬盘，防止丢失
5. 附加过期处理、发布订阅、事物、流水线、LUA脚本的功能

##### 特性

* 读写速度：

  1. 基于内存操作
  2. C语言实现
  3. 单线程+IO多路复用，避免多线程环境下，并发竞争

* 键值对：

  value可以是字符串，也可以是其他数据结构

* 多种功能：

  1. 键过期，用于缓存，分布式锁
  2. 简单的发布订阅能力，一般不用
  3. Lua 脚本
  4. 持久化支持，AOF、RDB
  5. 高可用：集群主从复制、哨兵模式、cluster模式，主从+哨兵配合使用
  6. 慢慢来...

##### 使用场景

* 缓存：缓存热数据，分摊数据库的压力
* 排行榜：
* 计数器：
* 赞、踩、粉丝、共同好友、推送、下拉刷新
* 消息队列：一般不用

##### 原理

* 单线程模型

  ==来张图比较好理解==

  1. Bs客户端发送http请求到服务器 ，bs 会阻塞等待服务器返回结果，我们的redis比较牛，他先把命令发到一个队列中依次执行。
  2. 单线程了还快？
     1. 内存操作
     2. 非阻塞IO，epoll 多路IO复用  这个知识点很大，后面慢慢解释
     3. 单线程避免竞态

#### 2、安装&配置

#### 3、数据结构

###### 全局命令

```shell
keys
dbsize
exists key
del a b c
expire key 20
type key 
```

###### 数据结构

* string

  ~~~powershell
   ## 参数解析
   ## ex seconds：为键设置秒级过期时间。px milliseconds：为键设置毫秒级过期时间。
   ## nx：键必须不存在，才可以设置成功，用于添加。xx：与nx相反，键必须存在，才可以设置成功，用于更新。
   set key value [ex seconds] [px milliseconds] [nx|xx]
   ## 例子
   set test hello
  ~~~

  ````powershell
   ## 格式
   setex key seconds value
   setnx key value
   ## 因为键test已存在，所以setnx会失败，返回结果为0
   setnx test redis
   ## 因为键test已存在，所以setex会成功，并修改对应的值
   setex test 60 redis
  
  ````

  ```powershell
  ## 格式
  get key
  ## 获取testkey，返回的就是你设置的那个值了，如果key不存在则返回(nil)
  get test
  
  ```

  ```powershell
  ## 格式
  mset key value [key value ...]
  ## 例子
  mset a 1 b 2 c 3 d 4
  ```

  ```powershell
  ## 格式
  mget key [key ...]
  ## 例子，返回的结果就是按顺序对应key的值了，如果不存在这是(nil)
  mget a b c
  ```

  ```powershell
  ## incr 的值是一个自增操作，如果值不是整数是会报错的，如果key不存在，执行命令返回的就是1，否则则是自增加一。
  incr key
  ## 第一次计数,返回1
  incr auto
  ## 第二次计数,返回2
  incr auto     
  ```

  ````powershell
  ## decr（自减）、incrby（自增指定数字）、 decrby（自减指定数字）、incrbyfloat（自增浮点数）
  decr key
  incrby key increment
  decrby key decrement
  incrbyfloat key increment
  ````

  ```powershell
   ## 格式
   append key value
   ## 给test追加，返回的是值的长度
   append test yyds
  ```

  ```powershell
   ## 格式
   strlen key
   ## 获取test值长度，需要注意的是一个中文占3个字节。
   strlen test
  ```

  ```powershell
   ## 格式
   getset key value
   ## 如果key不存在则返回(nil)，否则就返回设值前的值
    getset test hello1
  ```

  ````powershell
   ## 格式，从哪个位置到哪个位置，从0开始算
   getrange key start end
   ## 获取0-1的字符，返回he
   getrange test 0 1
  ````

* set

  ~~~powershell
   ## 添加元素
   sadd key element [element ...]
   ## 向myset中添加 abc,返回的是是成功添加元素的个数
   sadd myset a b c
  ~~~

  ````powershell
   ## 格式 可以指定多个
   srem key element [element ...]
   ## 删除myset中的ab，返回的是成功删除的个数
   srem myset a b
  
  ````

  ```powershell
   ## 格式，scard的时间复杂度为O（1），它不会遍历集合所有元素，而是直接用Redis内部的变量
   scard key
   ## 返回myset的元素个数
   scard myset
  
  ```

  ```powershell
   ## 格式
   sismember key element
   ## 在就返回1否则返回0
   sismember myset c
  ```

  ```powershell
   ## 格式，[count]是可选参数，如果不写默认为1
   srandmember key [count]
   ## 随机返回myset中的两个元素
   srandmember myset 2
  
  ```

  ```powershell
   ## 格式，srandmember和spop都是随机从集合选出元素，两者不同的是spop命令执行后，元素会从集合中删除，而srandmember不会。
   spop key
   ## 从myset随机删除一个
   spop myset
  ```

   ```powershell
   ## 格式
   smembers key
   ## 返回myset中的所有元素，需要注意的是结果是无序的
   smembers myset
  
   ```

  ````powershell
   ## 格式
   sinter key [key ...]
   ## 取user:1:tag user:2:tag的交集，结果返回sunny和cute
   sinter user:1:tag user:2:tag
  
  ````

  ```powershell
   ## 格式
   sdiff key [key ...]
   ## 取user:1:tag user:2:tag的差集，结果返回handsome beefcake
   sdiff user:1:tag user:2:tag
  
  ```

  使用场景：用户添加标签，一人多个标签
  好友/关注/粉丝/感兴趣的人
  随机展示
  黑名单/白名单

* zset

  ~~~powershell
   ## 添加成员
   zadd key score member [score member ...]
   ## 向有序集合user:rankings添加用户test和他的分数100
   zadd user:rankings 100 test
  ~~~

  ````powershell
   ## 格式
   zcard key
   ## 统计user:rankings成员个数
   zadd user:rankings 100 test
  ````

  ```powershell
   ## 格式
   zscore key member
   ## 获取test分数
   zscore user:rankings test
  ```

  ```powershell
   ## zrank是从分数从低到高返回排名（排名从0开始计算）
   zrank key member
   ## zrevrank是从分数从高到低返回排名（排名从0开始计算）
   zrevrank key member
   ## 从高到低获取返回1
   zrank user:rankings test
   ## 从低到高获取返回0
   zrevrank user:rankings test
  
  ```

  ```powershell
   ## 格式，支持多个成员
   zrem key member [member ...]
   ## 删除user:ranking中的test
   zrem user:rankings test
  
  
  ```

  ```powershell
   ## 格式,increment 需要增加的分数
   zincrby key increment member
   ## 给test添加10
   zincrby user:rankings 10 test
  
  ```

  ```powershell
   ## 格式,按照分值排名的，zrange是从低到高返回
   zrange key start end [withscores]
   ## 格式,按照分值排名的，zrevrange是从高到低返回
   zrevrange key start end [withscores]
   ## 返回整个有序集成员
   zrange user:rankings 0 -1 withscores
   ## 返回有序集下标区间 1 至 2 的成员和分数
   zrange user:rankings 1 2 withscores
  
  ```

  ```powershell
   ## 格式,zrangebyscore按照分数从低到高返回
   zrangebyscore key min max [withscores] [limit offset count]
   ## 格式,zrevrangebyscore 按照分数从高到低返回
   zrevrangebyscore key max min [withscores] [limit offset count]
   ## 从低到高返回100到200的成员
   zrangebyscore user:rankings 100 200 withscores
  ```

  ```powershell
   ## 格式
   zcount key min max
   ## 格式,zrevrangebyscore 按照分数从高到低返回
   zcount user:rankings 100 200
  ```

  ````powershell
   ## 格式
   zremrangebyrank key start end
   ## 删除第0到第2名的元素
   zremrangebyrank user:rankings 0 2
  ````

  ```powershell
   ## 格式
   zremrangebyscore key min max
   ## 删除100到200分的成员
   zremrangebyscore user:rankings 100 200
  ```

  ```powershell
   ## 格式，交集
   ## destination: 交集计算结果保存到这个键。
   ## numkeys: 需要做交集计算键的个数。
   ## key[key...]: 需要做交集计算的键。
   ## weights weight[weight...]: 每个键的权重，在做交集计算时，每个键中的每个member会将自己分数乘以这个权重，每个键的权重默认是1。
   ## aggregate sum|min|max: 计算成员交集后，分值可以按照sum（和）、
   ## min（最小值）、max（最大值）做汇总，默认值是sum
   zinterstore destination numkeys key [key ...] [weights weight [weight ...]] [aggregate sum|min|max]  
   
   
    ## 格式，参数于交集的一致
   zunionstore destination numkeys key [key ...] [weights weight [weight ...]] [aggregate sum|min|max]
  ```

​      使用场景：
​      1、标签   2、共同好友。3、网站独立IP  4、统计用户点赞、取消点赞 5、排行榜功能

* list

  ~~~powershell
   ## 从右边插入元素(right)
   rpush key value [value ...]
   ## 下面代码从右向左插入元素
   rpush city:guangdong guangzhou chaoshan shenzhen
   ## 从左边插入元素(left)
   lpush key value [value ...]
   ## 下面代码从左向右插入元素
   rpush city:guangdong shenzhen chaoshan guangzhou
   ## 向某个元素前或者后插入元素, linsert命令会从列表中找到等于(pivot)的元素，在其前(before)或者后(after)插入一个新的元素(value)
   linsert key before|after pivot value
   ## 在潮汕前插入一个元素zhongshan，返回的是列表长度
   linsert city:guangdong before chaoshan zhongshan
  
  ~~~

  ````powershell
   ## 获取指定范围内的元素列表。索引下标从左到右分别是0到N-1，但是从右到左分别是-1到-N。lrange中的end选项包含了自身，这个和很多编程语言不包含end不太相同。
   lrange key start end
   ## 返回的就是从1到包含3的列表，zhongshan chaoshan guangzhou
   lrange city:guangdong 1 3
   ## 获取列表指定索引下标的元素
   lindex key index
   ## 获取city:guangdong索引为1的元素 guangzhou
   lindex city:guangdong 1
   ## 获取列表长度
   llen key
   ## 获取city:guangdong的长度，返回4
   llen city:guangdong
  
  ````

  ```powershell
   ## 从列表左侧弹出元素
   lpop key
   ## 从左侧删除一个元素
   lpop city:guangdong
   ## 从列表右侧弹出
   rpop key
    ## 从右侧删除一个元素
   rpop city:guangdong
   ## 删除指定元素。从列表中找到等于value的元素进行删除。
   ## 如果count>0，从左到右，删除最多count个元素。如果count=0，删除所有。
   ## 如果count<0，从右到左，删除最多count绝对值个元素。
   lrem key count value
   ## 比如我们的列表为a a a guangzhou zhongshan chaoshan guangzhou,执行下面命令结果为guangzhou zhongshan chaoshan guangzhou
   lrem city:guangdong 3 a
   ## 按照索引范围修剪列表,让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。
   ltrim key start end
   ## 下面操作会只保留列表city:guangdong第2个到第4个元素
   ltrim city:guangdong 1 3
  
  
  ```

  ```powershell
   ## 修改指定索引下标的元素
   lset key index newValue
   ## 将第一个元素修改为beijing
   lset city:guangdong 0 beijing
  
  ```

  ```powershell
   ## 阻塞式弹出，blpop和brpop是lpop和rpop的阻塞版本
   ## key[key...]：多个列表的键，timeout：阻塞时间（单位：秒）
   ## 注意点一、如果是多个键，那么brpop会从左至右遍历键，一旦有一个键能弹出元素，客户端立即返回
   ## 注意点二、如果多个客户端对同一个键执行brpop，那么最先执行brpop命令的客户端可以获取到弹出的值。
   blpop key [key ...] timeout
   brpop key [key ...] timeout
   ## 如果里里列表中没有元素，阻塞三秒后返回，如果设置为0，则会一直阻塞，如果此期间添加了数据element1，客户端立即返回。
   ## 列表不为空：客户端会立即返回
   brpop city:guangdong 3
  ```

  使用场景：
  消息队列
  朋友圈点赞列表，评论列表

  定时排行榜

* hash

  ~~~powershell
    ## 格式
   hset key field value
   ## 为用户1添加一对field-value，成功返回1，反之就是0
   hset user:1 name test
  ~~~

  ````powershell
   ## 格式，这个命令跟String的set、setnx逻辑是一样的，但是其作用域由键变为field，不懂的可以看下上一篇
   hsetnx key_name field value
   ## 再次执行下面这个命令会不成功，因为name 已经存在了
   hsetnx user:1 name test
  ````

  ```powershell
   ## 格式，需要同时制定key和field
   hget key field
   ## 获取user:1key下面的name
   hget user:1 name
  ```

  ```powershell
   ## 格式，需要同时制定key和field
   hget key field
   ## 获取user:1key下面的name
   hget user:1 name
  ```

  ```powershell
   ## 格式，可以删除多个field
   hdel key field [field ...]
   ## 删除user:1下面name和age
   hdel user:1 name age
  
  ```

  ```powershell
   ## 格式
   hlen key
   ## 统计user:1field的个数
   hlen user:1
  
  ```

  ```powershell
   ## 格式
   hmget key field [field ...]
   hmset key field value [field value ...]
   ## 获取user:1下的name和age
   hmget user:1 name age
   ## 批量设置user:1的field
   hmset user:1 name test2 age 12 city guangzhou
  
  ```

  ```powershell
   ## 格式
   hexists key field
   ## 判断user:1下的name是否存在，存在返回1，否则0
   hexists user:1 name
  
  ```

  ```powershell
   ## 格式，怎么不叫hfields,Antirez 你误导我
   hkeys key
   ## 返回user:1 下所有的field
   hkeys user:1
  ```

  ````powershell
   ## 格式
   hvals key
   ## 返回user:1 下所有的value
   hvals user:1
  
  ````

  应用场景：购物车，用户id key、商品ID field、商品数量 value
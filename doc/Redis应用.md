##### 排行榜

###### 1、常规实现

//数据准备

![image-20201225112708671](/Users/duanyaqiang/github/DocManual/doc/image-20201225112708671.png)

//获取高分TOP 10

![image-20201225113012068](/Users/duanyaqiang/github/DocManual/doc/image-20201225113012068.png)

//获取低分TOP 10

![image-20201225113120487](/Users/duanyaqiang/github/DocManual/doc/image-20201225113120487.png)



//查询用户的排名

![image-20201225113525497](/Users/duanyaqiang/github/DocManual/doc/image-20201225113525497.png)



//查询用户分数

![image-20201225113356836](/Users/duanyaqiang/github/DocManual/doc/image-20201225113356836.png)



###### 问题

* 24 小时内怎么实现呢

* 相同分数怎么实现呢

  1. 首先默认是按照字典排序的

  2. 加入时间戳解决方案    
     带时间戳的分数 = 实际分数*10000000000 + (9999999999 – timestamp)，分数优先+时间倒序

     读取分数时  = 去掉后10位即可

  3. 相同时间加入呢，（可以忽略）。

  4. 有个问题 排行榜过大的时候，目前只有6位排行 （可以忽略）


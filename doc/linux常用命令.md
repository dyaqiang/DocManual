##### 1、进程相关

```shell
 # 查找被占用的端口
 netstat -tln | grep 8080
 
 # 查看端口 被那个进程占用
 lsof -i:8080
 
 # 杀死进程
 kill -9 8080
 
 #查看进程
ps -ef | grep nginx
pkill -9 nginx
```



##### 2、网络连接

```shell
#ssh 连接远程服务器
ssh -p 22 root@81.70.42.98

#文件传输
scp local_file remote_username@remote_ip:remote_file 
```



##### 3、文件

```shell
#解压
tar -zxvf java.tar.gz

#复制目录
cp -r /root/movie/ /tmp/
```



##### 4、用户

```shell
#新建用户
adduser es 
passwd es

#切换用户
su root 

#添加文件权限
chown -R es:es /root/elasticsearch-7.0/
```



##### 5、yum 常用

```shell
#查看yum源
yum -y list java*
#查看yum源码
yum search java|grep jdk 

#安装
yum install java-1.8.0-openjdk.x86_64
#卸载
yum -y remove java-1.8.0-open* 
```



##### 6、vim

~~~shell
#删除一行
dd
~~~








#####  基本操作

```
git clone 
git status
git branch
git add 
git pull 
git push 
git checkout 
git checkout -b 
```



##### 场景一

```gitlab
最重要的场景： 

从远程克隆下来一个主分支，切换到自己的分支上开发，自己的分支开发完成后 要合并到远程的主分支上。 这个时候，有其他人对相同的文件也进行了修改，那么就会出现冲突。


两种解决方案

方案一：
1、克隆master
2、创建新的分支dev
3、开发dev
4、本地分支dev.  push到远程对应的分支上dev，
5、中间有人修改了相同文件的master。
6、这时候合并远程dev到远程master   必定会出现冲突
7、先切换到本地dev分支到本地主分支master上， 拉取远程master下来到本地master 
然后 再切换回去dev 再合并到当前分支上
8、再pu sh 到远程的 分支上 再合并 就OK了

方案二：
1、2、3、4、5、6 同一
7、直接拉取 远程的主分支上到当前的分支 然后 直接push  再合并 这种感觉是有风险的
8、风险在哪里？？？？？？？？？
操作中.....
```


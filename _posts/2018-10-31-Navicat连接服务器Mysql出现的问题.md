---
*layout*: post
tile: 2018-10-31Navicat连接服务器Mysql出现的问题
date: 2018-10-30
description: 今天在用navicat连接腾讯云服务器时，出现了一系列的问题，也根据网上的解答成功连接上了服务器，故来做个总结。
tag: 博客
---

### 引言

今天在用navicat连接腾讯云服务器时，出现了一系列的问题，也根据网上的解答成功连接上了服务器，故来做个总结。



### 安全组未设置

何为安全组，用我自己的话来理解就是为了安全，把服务器允许被操作的一系列操作归为一个组，这个就是安全组

最容易出现的，也是最容易解决的问题就是安全组的权限问题。我们应该手动设服务器的`3306`端口已经被打开，可以被外界连接，解决方法也很简单，打开服务器，找到安全组选项，添加一个规则。

![TencentCloudSafe](/images/mysql/TencentCloudSafe.png)



### MySQL配置文件

出现`Password authentication failed`可能是因为其中一个配置项没有开启。

将`/etc/ssh/sshd_config`中的`#Passwordauthentication yes`的注释`#`去掉。2、重启sshd。

```shell
$ systemctl restart sshd 
```

重启完，应该还没好，emmm，最关键的一步来了



### 外部IP访问授权

你会看到这样的一个错误`1130-Host XXX.XXX.XXX.XXX is not allowed to connect to this MySQL server`,对，你没有权限去访问一个root用户的数据库，所以我们需要开放权限给我们本地使用Navicat的主机

我们不建议你开放所有ip，这太过于危险。所以，下面的命令可以使你只添加一个`Host`。



1.首先远程连接进入服务器，在shell中输入`mysql -u root -p`，然后回车，输入密码后回车进入mysql命令行。 

```shell
 $ mysql -u root -p
```



2.输入`use mysql;`注意加上分号，不加的话默认语句未执行完毕。

```mysql
 mysql> use mysql;
```



3.输入`select Host from user; `你会看到只有`localhost`。我们需要将xxx.xxx.xxx.xxx也添加到这里。

```mysql
 mysql> select Host from user; 
```

+-----------------------------------------------------+
| 			Host                       		    |
+-----------------------------------------------------+
|		 localhost               		    |
|		 localhost               		    |
|		 localhost                     		    |
|		 localhost                                   |
+-----------------------------------------------------+



4.添加方法如下：

输入 `grant all privileges on *.* to root@"xxx.xxx.xxx.xxx" identified by "password";`

```mysql
 mysql> grant all privileges on *.* to root@"xxx.xxx.xxx.xxx" identified by "password";
```

上面也提到了，不推荐授权所有IP， `GRANT ALL PRIVILEGES ON *.* TO ‘root’@’xxx.xxx.xxx.xxx’ IDENTIFIED BY ‘123456’ WITH GRANT OPTION;`这种操作最好不要做。这相当于是给IP-xxx.xxx.xxx.xxx赋予了所有的权限，当然也包括远程访问权限。



然后再输入 `flush privileges;` 重载mysql权限。

```mysql
mysql> flush privileges;
```



### 总结

上面这些操作应该就能解决问题了，如果不能，emmmm，我也没见过。再去[google](https://www.google.com)或者[baidu](https://www.baidu.com/)喽。
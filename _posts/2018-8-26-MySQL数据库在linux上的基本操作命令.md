---
layout: post
tile: 2018-8-26-MySQL数据库在linux上的基本操作命令
date: 2018-8-26
description: 在linux上进入msql的命令，以及mysql的基本操作命令。
tag: 博客
---

## 在进入msql之前我们需要先在本地注册账户并登陆，这些都是需要在命令行中完成的,其命令如下。

### 创建账户并且设置登录密码

~~~shell
    $mysql -u <usrName> -p 
~~~

### 退出mysql
~~~shell
    $exit                    
~~~

### 重置密码
~~~shell
    $mysqladmin -u <usrName> -p password "********"   
~~~
--------------------
## 现在我们可以正式进入mysql数据库里了，我们需要先学习一下最基本的对数据库层面进行的操作命令，比如说，查看数据库，创建数据库，切换数据库等等操作。
### 查看所有数据库
~~~sql
    show database                      
~~~

### 切换数据库
~~~sql
    use <databaseName> UTF-8   
~~~

## 接下来就是对表进行操作。

### 查看所有表
~~~sql
    show tables                   
~~~

### 查看表字段
~~~sql
    desc <tableName>                   
~~~

### 显示create database 语句是否能够创建指定的数据库
~~~sql
    show create database  
~~~

### 显示create database 语句是否能够创建指定的数据库            
~~~sql
    show create table <tableName>
~~~
     
### 查看当前用户
~~~sql
    select user()     
~~~
    
### 查看当前数据库     
~~~sql
    select database()                
~~~

------------

### 创建数据库
~~~sql
    create database <databaseName>
~~~ 
### 创建表 
~~~sql
    create table                   
~~~

--------------------

## 进行基础的增删改查操作

### 向表中插入
~~~sql
    insert into <table_name> values()
~~~

### 删除操作
~~~sql
    delete from <table_name> where <条件>
~~~  
  
### 表的更新
~~~sql
update <table_name> set <column_name> = <表达式> where <条件>
~~~

### 表的查询
~~~sql
    --where表示限定条件
    select <column_name> from <table_name> where <条件>

    --*为通配符，意为所有
    selec * from <table_name>
~~~

## 未完待续...
---
layout: post
tile: 2019-2-27-2019-2-27-解决putty长时间无操作自动断开问题putty
date: 2019-2-27
description: 使用putty连接远程服务器，经常会出现长时间无操作后就自动断开，或者无响应，无法再通过键盘输入，再过一会就自动断开。我们就要重复的登录，这样是很麻烦的。所以有了这样一篇博文
tag: 博客
---



### 简述：

使用putty连接远程服务器，经常会出现长时间无操作后就自动断开，或者无响应，无法再通过键盘输入，再过一会就自动断开。我们就要重复的登录，这样是很麻烦的。所以可以这样做。

### 特别说明：

本文参考了 Bingo的相关博客，链接如下： <https://bingozb.github.io/61.html>



### 解决方法：

在Connection里面有个Seconds between keepaliaves。这里就是每间隔指定的秒数，就给服务器发送一个空的数据包，来保持连接。以免登录的主机那边在长时间没接到数据后，会自动断开SSH的连接。默认是0，就是不打开这个功能。 

![putty](/images/putty.png)

这就是最简单的方法

### 心跳验证

#### 客户端验证

如果客户端为Linux或Unix

```shell
vim /etc/ssh/ssh_config
```

添加如下选项

```diff
+ ServerAliveInterval 20
+ ServerAliveCountMax 999
```

- `ServerAliveInterval`表示每隔多少秒，从客户端向服务器端发送一次心跳（alive 检测）。
- `ServerAliveCountMax`表示服务端多少次心跳无响应之后，客户端才会认为与服务器的 SSH 连接已经断开，然后断开连接。

#### 服务端验证

```shell
sudo vim /etc/ssh/sshd_config
```



```diff
+ ClientAliveInterval 60
+ ClientAliveCountMax 3
```

- `ClientAliveInterval`表示每隔多少秒，从服务器端向客户端发送一次心跳。
- `ClientAliveInterval`表示客户端多少次心跳无响应之后，服务端才会认为客户端已经断开连接，然后断开连接。

上述配置则表示：每隔60秒，服务器向客户端发出一次心跳。若客户端超过3次请求未响应，则会从服务器端断开与客户端的连接。所以，总共允许无响应的时间是 60 * 3 = 180 秒以内。
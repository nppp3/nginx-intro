# 安装nginx

## 一、安装环境

操作系统：

CentOS Linux release 7.5.1804

nginx版本：

nginx-1.20.2

用途：

http服务器

## 二、准备工作

### 1. 创建用户

首先我们先创建一个普通用户，用于安装及运行nginx。（如果已经创建过普通用户，此步可跳过）

`# useradd nginx`

### 2. 创建要部署到的目录

在根下创建servers文件夹

`# mkdir /servers`

修改文件夹属主为我们刚刚创建的nginx用户

`# chown -R nginx:nginx /servers`

### 3. 安装PCRE库和zlib库（rewrite模块和gzip模块ssl模块需要）

`# yum install pcre pcre-devel zlib zlib-devel  openssl openssl-devel openssl-libs`

接着我们切换到nginx用户，开始下一步的操作

`# su - nginx`

## 三、从源码编译安装

下载nginx源码包并解压

```shell
$ wget http://nginx.org/download/nginx-1.20.2.tar.gz
$ tar zxvf nginx-1.20.2.tar.gz
```

配置nginx：

```shell
$ cd nginx-1.20.2
$ ./configure --prefix=/servers/nginx --with-http_ssl_module
```

参数

--prefix                            用于指定nginx安装到的目录

--with-http_ssl_module   启用ssl模块

编译安装：

$ make && make install

## 四、启动nginx

如果此时启动nginx，会出现一条报错

`$ sudo /servers/nginx/sbin/nginx`

> nginx: [emerg] bind() to 0.0.0.0:80 failed (13: Permission denied)

绑定到0.0.0.0:80失败（拒绝权限）

解释：因为nginx要用到80端口，必须要用到root权限

我们再次切换到root用户，执行visudo

```shell
$ su
# visudo
```

在最后一行追加以下内容。保存退出

`nginx   ALL=NOPASSWD:   /servers/nginx/sbin/nginx`

再次启动

`$ sudo /servers/nginx/sbin/nginx`

最后，我们打开浏览器输入http://+ip地址。如果出现以下网页，安装成功啦。

nginx欢迎页面

在下一篇将讲解nginx配置文件。 
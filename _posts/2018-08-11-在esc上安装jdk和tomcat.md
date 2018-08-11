---
layout: post
title: 安装jdk和tomcat
keywords: 
category: linux
author: 晴天
description: 
---

在CentOS上安装jdk和tomcat。

下载安装包到相应目录，我放在了/home/java以及/home/tomcat下。

1.解压jdk-8u181-linux-x64.tar.gz

```
tar -zxvf jdk-8u144-linux-x64.tar.gz
```

2.配置环境变量，在/etc/profile中加入一下内容

```
export JAVA_HOME=/home/java/jdk1.8.0_181
export PATH=$PATH:${JAVA_HOME}/bin
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools/jar
```

source /etc/profile，运行java -version验证

3.解压apache-tomcat-9.0.10.tar.gz

```
tar -zxvf apache-tomcat-9.0.10.tar.gz
```

4.tomcat没有配置环境变量，成功启动了

5.再装一个nginx，前期准备GCC  PCRE  zlib  OpenSSL

```
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```

检查是否已安装pcre

```
rpm -qa pcre
```

查看pcre版本

```
pcre-config --version
```

6.下载nginx

```
wget http://nginx.org/download/nginx-1.15.2.tar.gz
```

7.解压安装，默认安装目录是/usr/local/nginx

```
tar zxvf nginx-1.15.2.tar.gz
cd nginx-1.15.2.tar.gz
./configure
make
make install
```

8.启动nginx

```
/usr/local/nginx/sbin/nginx
```

9.卸载，只需删除/usr/local/nginx即可

10.安装iptables防火墙

```
yum install -y iptables-services
```


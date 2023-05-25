<h1 style="color:skyblue;text-align:center">Nginx学习笔记</h1>







# 概述

Nginx（“engine x”）一个具有高性能的**HTTP**和**反向代理**的**WEB服务器**，同时也是一个**POP3/SMTP/IMAP代理服务器**，是由伊戈尔·赛索耶夫(俄罗斯人)使用C语言编写的，Nginx的第一个版本是2004年10月4号发布的0.1.0版本。另外值得一提的是伊戈尔·赛索耶夫将Nginx的源码进行了开源，这也为Nginx的发展提供了良好的保障。



## 名词解释

### POP3

POP3(Post Offic Protocol 3)邮局协议的第三个版本



### SMTP

SMTP(Simple Mail Transfer Protocol)简单邮件传输协议



### IMAP

IMAP(Internet Mail Access Protocol)交互式邮件存取协议



### 正向代理

![image-20230427212015741](img/Nginx学习笔记/image-20230427212015741.png)

正向代理是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理





### 反向代理

![image-20230427212030716](img/Nginx学习笔记/image-20230427212030716.png)



反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源。同时，用户不需要知道目标服务器的地址，也无须在用户端作任何设定。反向代理服务器通常可用来作为Web加速，即使用反向代理作为Web服务器的前置机来降低网络和服务器的负载，提高访问效率







## Nginx的优点

### 速度更快、并发更高

单次请求或者高并发请求的环境下，Nginx都会比其他Web服务器响应的速度更快。一方面在正常情况下，**单次请求会得到更快的响应**，另一方面，**在高峰期(如有数以万计的并发请求)，Nginx比其他Web服务器更快的响应请求**。Nginx之所以有这么高的并发处理能力和这么好的性能原因在于**Nginx采用了多进程和I/O多路复用(epoll)的底层实现**



### 配置简单，扩展性强

Nginx的设计**极具扩展性**，它本身就是由很多模块组成，这些模块的使用可以通过配置文件的配置来添加。这些模块有官方提供的也有第三方提供的模块，如果需要完全可以开发服务自己业务特性的定制模块



### 高可靠性

Nginx采用的是**多进程模式运行**，其中有**一个master主进程和N多个worker进程**，worker进程的数量我们可以手动设置，每个worker进程之间都是相互独立提供服务，并且master主进程可以在某一个worker进程出错时，快速去"拉起"新的worker进程提供服务



### 热部署

现在互联网项目都要求以7*24小时进行服务的提供，针对于这一要求，Nginx也提供了热部署功能，即**可以在Nginx不停止的情况下，对Nginx进行文件升级、更新配置和更换日志文件**等功能



### 成本低、BSD许可证

BSD是一个开源的许可证

Nginx本身是开源的，我们不仅可以**免费的将Nginx应用在商业领域**，而且**还可以在项目中直接修改Nginx的源码来定制自己的特殊要求**

![image-20230427213937784](img/Nginx学习笔记/image-20230427213937784.png)







## Nginx常用功能

### 基本HTTP服务

Nginx可以提供基本HTTP服务，可以作为HTTP代理服务器和反向代理服务器，支持通过缓存加速访问，可以完成简单的负载均衡和容错，支持包过滤功能，支持SSL等。

- 处理静态文件、处理索引文件以及支持自动索引；
- 提供反向代理服务器，并可以使用缓存加上反向代理，同时完成负载均衡和容错；
- 提供对FastCGI、memcached等服务的缓存机制，，同时完成负载均衡和容错；
- 使用Nginx的模块化特性提供过滤器功能。Nginx基本过滤器包括gzip压缩、ranges支持、chunked响应、XSLT、SSI以及图像缩放等。其中针对包含多个SSI的页面，经由FastCGI或反向代理，SSI过滤器可以并行处理。
- 支持HTTP下的安全套接层安全协议SSL.
- 支持基于加权和依赖的优先权的HTTP/2



### 高级HTTP服务

- 支持基于名字和IP的虚拟主机设置
- 支持HTTP/1.0中的KEEP-Alive模式和管线(PipeLined)模型连接
- 自定义访问日志格式、带缓存的日志写操作以及快速日志轮转。
- 提供3xx~5xx错误代码重定向功能
- 支持重写（Rewrite)模块扩展
- 支持重新加载配置以及在线升级时无需中断正在处理的请求
- 支持网络监控
- 支持FLV和MP4流媒体传输



### 邮件服务

Nginx提供邮件代理服务也是其基本开发需求之一，主要包含以下特性：

- 支持IMPA/POP3代理服务功能
- 支持内部SMTP代理服务功能







## Nginx官网

Nginx的官方网站为: http://nginx.org

![image-20230428134411492](img/Nginx学习笔记/image-20230428134411492.png)



Nginx的官方下载网站为http://nginx.org/en/download.html

![image-20230428134440063](img/Nginx学习笔记/image-20230428134440063.png)



* **Mainline version**：主线版本，开发Nginx的最新版本
* **Stable version**：稳定版本
* **Legacy versions**：旧版本









# 常见服务器

## IIS

全称(**Internet Information Services**)即互联网信息服务，是由微软公司提供的基于windows系统的互联网基本服务。windows作为服务器在稳定性与其他一些性能上都不如类UNIX操作系统，因此在需要高性能Web服务器的场合下，IIS可能就会被"冷落"



## Tomcat

Tomcat是一个运行Servlet和JSP的Web应用软件，Tomcat技术先进、性能稳定而且开放源代码，因此深受Java爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web应用服务器。但是Tomcat天生是一个**重量级**的Web服务器，**对静态文件和高并发的处理比较弱**



## Apache

Apache的发展时期很长，同时也有过一段辉煌的业绩。在2014年以前都是市场份额第一的服务器。Apache有很多优点，如**稳定、开源、跨平台**等。但是它出现的时间太久了，在它兴起的年代，互联网的产业规模远远不如今天，所以它被设计成一个**重量级的、不支持高并发**的Web服务器。在Apache服务器上，**如果有数以万计的并发HTTP请求同时访问，就会导致服务器上消耗大量能存，操作系统内核对成百上千的Apache进程做进程间切换也会消耗大量的CUP资源，并导致HTTP请求的平均响应速度降低**，这些都决定了Apache不可能成为高性能的Web服务器。这也促使了Lighttpd和Nginx的出现



## Lighttpd

Lighttpd是德国的一个开源的Web服务器软件，它和Nginx一样，都是**轻量级、高性能**的Web服务器，欧美的业界开发者比较钟爱Lighttpd，而国内的公司更多的青睐Nginx，同时**网上Nginx的资源要更丰富些**





## 其他的服务器

Google Servers，Weblogic, Webshpere(IBM)...









# Nginx安装与运行

## Docker安装

### 第一步：搜索镜像

命令：

```sh
docker search nginx
```

```sh
PS C:\Users\mao\Desktop> docker search nginx
NAME                                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                                             Official build of Nginx.                        18433     [OK]
linuxserver/nginx                                 An Nginx container, brought to you by LinuxS…   193
bitnami/nginx                                     Bitnami nginx Docker Image                      159                  [OK]
ubuntu/nginx                                      Nginx, a high-performance reverse proxy & we…   86
privatebin/nginx-fpm-alpine                       PrivateBin running on an Nginx, php-fpm & Al…   72                   [OK]
bitnami/nginx-ingress-controller                  Bitnami Docker Image for NGINX Ingress Contr…   25                   [OK]
rancher/nginx-ingress-controller                                                                  11
kasmweb/nginx                                     An Nginx image based off nginx:alpine and in…   6
bitnami/nginx-ldap-auth-daemon                                                                    3
bitnami/nginx-exporter                                                                            3
rapidfort/nginx                                   RapidFort optimized, hardened image for NGINX   3
circleci/nginx                                    This image is for internal use                  2
redash/nginx                                      Pre-configured nginx to proxy linked contain…   2
rancher/nginx-ingress-controller-defaultbackend                                                   2
vmware/nginx                                                                                      2
rancher/nginx                                                                                     2
rapidfort/nginx-official                          RapidFort optimized, hardened image for NGIN…   1
bitnami/nginx-intel                                                                               1
vmware/nginx-photon                                                                               1
rancher/nginx-conf                                                                                0
rancher/nginx-ssl                                                                                 0
rapidfort/nginx-ib                                RapidFort optimized, hardened image for NGIN…   0
unit                                              Official build of NGINX Unit: a polyglot app…   0         [OK]
continuumio/nginx-ingress-ws                                                                      0
rancher/nginx-ingress-controller-amd64                                                            0
PS C:\Users\mao\Desktop>
```





### 第二步：拉取镜像

命令：

```sh
docker pull nginx
```

如果不指定版本，默认使用最新版本

```sh
PS C:\Users\mao\Desktop> docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
26c5c85e47da: Downloading
4f3256bdf66b: Downloading
2019c71d5655: Downloading
8c767bdbc9ae: Downloading
78e14bb05fd3: Downloading
75576236abf5: Downloading
latest: Pulling from library/nginx
26c5c85e47da: Pull complete
4f3256bdf66b: Pull complete
2019c71d5655: Pull complete
8c767bdbc9ae: Pull complete
78e14bb05fd3: Pull complete
75576236abf5: Pull complete
Digest: sha256:63b44e8ddb83d5dd8020327c1f40436e37a6fffd3ef2498a6204df23be6e7e94
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
PS C:\Users\mao\Desktop>
```





### 第三步：查看镜像是否拉取成功

命令：

```sh
docker images
```

```sh
PS C:\Users\mao\Desktop> docker images
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
nginx               latest    6efc10a0510f   2 weeks ago     142MB
redislabs/rebloom   latest    66d626dc1387   17 months ago   147MB
PS C:\Users\mao\Desktop>
```





### 第四步：运行

```sh
docker run  -v D:/Docker/nginx/logs:/var/log/nginx -p 80:80 -d --name nginx nginx
```

```sh
PS C:\Users\mao\Desktop> docker run  -v D:/Docker/nginx/logs:/var/log/nginx -v D:/Docker/nginx/html:/usr/share/nginx/html -v D:/Docker/nginx/conf:/etc/nginx/conf.d -p 80:80 -d --name nginx nginx
8354588d9e6fdc57932f913236e556c8971523f62b65c998ec26bb259f98f146
PS C:\Users\mao\Desktop>
```

```sh
PS C:\Users\mao\Desktop> docker run  -v D:/Docker/nginx/logs:/var/log/nginx -p 80:80 -d --name nginx nginx
4f3ddbf99a0e392fffaf99af51bf055e04d3a300b28bc44d6c551d5487e47d47
PS C:\Users\mao\Desktop>
```







### 第五步：检查运行状态

命令：

```sh
docker ps
```

或者：

```sh
docker ps -a
```



```sh
PS C:\Users\mao\Desktop> docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                NAMES
8354588d9e6f   nginx     "/docker-entrypoint.…"   13 seconds ago   Up 11 seconds   0.0.0.0:80->80/tcp   nginx
PS C:\Users\mao\Desktop>
```

```sh
PS C:\Users\mao\Desktop> docker ps -a
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS                     PORTS                     NAMES
4f3ddbf99a0e   nginx               "/docker-entrypoint.…"   20 seconds ago   Up 19 seconds              0.0.0.0:80->80/tcp        nginx
0a3197e86f21   redislabs/rebloom   "docker-entrypoint.s…"   8 weeks ago      Exited (255) 2 weeks ago   0.0.0.0:16379->6379/tcp   redis-redisbloom
PS C:\Users\mao\Desktop>
```





### 第六步：访问Nginx

http://localhost/

![image-20230428142728767](img/Nginx学习笔记/image-20230428142728767.png)



### 第六步：进入容器查看日志

```sh
docker exec -it nginx /bin/bash
```

```sh
PS C:\Users\mao\Desktop> docker exec -it nginx /bin/bash
root@4f3ddbf99a0e:/# pwd
/
root@4f3ddbf99a0e:/# type nginx
nginx is /usr/sbin/nginx
root@4f3ddbf99a0e:/# cd /var/log/nginx
root@4f3ddbf99a0e:/var/log/nginx# ls -l
total 8
-rw-r--r-- 1 root root  859 Apr 28 06:26 access.log
-rw-r--r-- 1 root root 2507 Apr 28 06:26 error.log
root@4f3ddbf99a0e:/var/log/nginx# cat access.log
172.17.0.1 - - [28/Apr/2023:06:26:12 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:06:26:12 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:06:26:19 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:06:26:19 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
root@4f3ddbf99a0e:/var/log/nginx# cat error.log
2023/04/28 06:25:46 [notice] 1#1: using the "epoll" event method
2023/04/28 06:25:46 [notice] 1#1: nginx/1.23.4
2023/04/28 06:25:46 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6)
2023/04/28 06:25:46 [notice] 1#1: OS: Linux 5.10.16.3-microsoft-standard-WSL2
2023/04/28 06:25:46 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/04/28 06:25:46 [notice] 1#1: start worker processes
2023/04/28 06:25:46 [notice] 1#1: start worker process 29
2023/04/28 06:25:46 [notice] 1#1: start worker process 30
2023/04/28 06:25:46 [notice] 1#1: start worker process 31
2023/04/28 06:25:46 [notice] 1#1: start worker process 32
2023/04/28 06:25:46 [notice] 1#1: start worker process 33
2023/04/28 06:25:46 [notice] 1#1: start worker process 34
2023/04/28 06:25:46 [notice] 1#1: start worker process 35
2023/04/28 06:25:46 [notice] 1#1: start worker process 36
2023/04/28 06:25:46 [notice] 1#1: start worker process 37
2023/04/28 06:25:46 [notice] 1#1: start worker process 38
2023/04/28 06:25:46 [notice] 1#1: start worker process 39
2023/04/28 06:25:46 [notice] 1#1: start worker process 40
2023/04/28 06:25:46 [notice] 1#1: start worker process 41
2023/04/28 06:25:46 [notice] 1#1: start worker process 42
2023/04/28 06:25:46 [notice] 1#1: start worker process 43
2023/04/28 06:25:46 [notice] 1#1: start worker process 44
2023/04/28 06:25:46 [notice] 1#1: start worker process 45
2023/04/28 06:25:46 [notice] 1#1: start worker process 46
2023/04/28 06:25:46 [notice] 1#1: start worker process 47
2023/04/28 06:25:46 [notice] 1#1: start worker process 48
2023/04/28 06:25:46 [notice] 1#1: start worker process 49
2023/04/28 06:25:46 [notice] 1#1: start worker process 50
2023/04/28 06:25:46 [notice] 1#1: start worker process 51
2023/04/28 06:25:46 [notice] 1#1: start worker process 52
2023/04/28 06:25:46 [notice] 1#1: start worker process 53
2023/04/28 06:25:46 [notice] 1#1: start worker process 54
2023/04/28 06:25:46 [notice] 1#1: start worker process 55
2023/04/28 06:25:46 [notice] 1#1: start worker process 56
2023/04/28 06:25:46 [notice] 1#1: start worker process 57
2023/04/28 06:25:46 [notice] 1#1: start worker process 58
2023/04/28 06:25:46 [notice] 1#1: start worker process 59
2023/04/28 06:25:46 [notice] 1#1: start worker process 60
2023/04/28 06:26:12 [error] 30#30: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost", referrer: "http://localhost/"
root@4f3ddbf99a0e:/var/log/nginx#
```









## Linux安装



Linux：centOS



### 第一步：准备Linux系统

准备一个内核为2.6及以上版本的操作系统，因为linux2.6及以上内核才支持epoll,而Nginx需要解决高并发压力问题是需要用到epoll，所以我们需要有这样的版本要求

确保centos能联网

确认关闭防火墙



这里为了方便，使用docker的centOS来演示

```sh
PS C:\Users\mao\Desktop> docker search centos
NAME                                         DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                                       DEPRECATED; The official build of CentOS.       7569      [OK]
kasmweb/centos-7-desktop                     CentOS 7 desktop for Kasm Workspaces            36
couchbase/centos7-systemd                    centos7-systemd images with additional debug…   7                    [OK]
dokken/centos-7                              CentOS 7 image for kitchen-dokken               6
dokken/centos-stream-9                                                                       4
dokken/centos-stream-8                                                                       4
continuumio/centos5_gcc5_base                                                                3
eclipse/centos_jdk8                          CentOS, JDK8, Maven 3, git, curl, nmap, mc, …   3                    [OK]
dokken/centos-8                              CentOS 8 image for kitchen-dokken               3
adoptopenjdk/centos7_build_image                                                             1
spack/centos7                                CentOS 7 with Spack preinstalled                1
spack/centos6                                CentOS 6 with Spack preinstalled                1
atlas/centos7-atlasos                        ATLAS CentOS 7 Software Development OS          0
couchbase/centos-72-java-sdk                                                                 0
ustclug/centos                               Official CentOS Image with USTC Mirror          0
couchbase/centos-72-jenkins-core                                                             0
dokken/centos-6                              CentOS 6 image for kitchen-dokken               0
datadog/centos-i386                                                                          0
couchbase/centos-70-sdk-build                                                                0
couchbase/centos-69-sdk-build                                                                0
couchbase/centos-69-sdk-nodevtoolset-build                                                   0
bitnami/centos-extras-base                                                                   0
corpusops/centos-bare                        https://github.com/corpusops/docker-images/     0
bitnami/centos-base-buildpack                Centos base compilation image                   0                    [OK]
corpusops/centos                             centos corpusops baseimage                      0
PS C:\Users\mao\Desktop> docker pull centos
Using default tag: latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
PS C:\Users\mao\Desktop> docker images
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
nginx               latest    6efc10a0510f   2 weeks ago     142MB
redislabs/rebloom   latest    66d626dc1387   17 months ago   147MB
centos              latest    5d0da3dc9764   19 months ago   231MB
PS C:\Users\mao\Desktop>
```



```sh
docker run -it --name centos -p 8090:80 centos
```





### 第二步：更换yum源

```sh
cd /etc/yum.repos.d/
```

```sh
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
```

```sh
yum makecache
```

```sh
yum update -y
```





### 第三步：安装yum-utils

```sh
yum install -y yum-utils
```

```sh
[root@b402c92dee3f /]# yum install -y yum-utils
Failed to set locale, defaulting to C.UTF-8
Last metadata expiration check: 0:06:27 ago on Fri Apr 28 06:56:11 2023.
Dependencies resolved.
================================================================================================================================================
 Package                                       Architecture                Version                            Repository                   Size
================================================================================================================================================
Installing:
 yum-utils                                     noarch                      4.0.21-3.el8                       baseos                       73 k
Installing dependencies:
 dbus-glib                                     x86_64                      0.110-2.el8                        baseos                      127 k
 dnf-plugins-core                              noarch                      4.0.21-3.el8                       baseos                       70 k
 python3-dateutil                              noarch                      1:2.6.1-6.el8                      baseos                      251 k
 python3-dbus                                  x86_64                      1.2.4-15.el8                       baseos                      134 k
 python3-dnf-plugins-core                      noarch                      4.0.21-3.el8                       baseos                      234 k
 python3-six                                   noarch                      1.11.0-8.el8                       baseos                       38 k

Transaction Summary
================================================================================================================================================
Install  7 Packages

Total download size: 927 k
Installed size: 2.3 M
Downloading Packages:
(1/7): dnf-plugins-core-4.0.21-3.el8.noarch.rpm                                                                  51 kB/s |  70 kB     00:01
(2/7): dbus-glib-0.110-2.el8.x86_64.rpm                                                                          78 kB/s | 127 kB     00:01
(3/7): python3-dateutil-2.6.1-6.el8.noarch.rpm                                                                  135 kB/s | 251 kB     00:01
(4/7): python3-dbus-1.2.4-15.el8.x86_64.rpm                                                                     218 kB/s | 134 kB     00:00
(5/7): python3-six-1.11.0-8.el8.noarch.rpm                                                                       87 kB/s |  38 kB     00:00
(6/7): python3-dnf-plugins-core-4.0.21-3.el8.noarch.rpm                                                         316 kB/s | 234 kB     00:00
(7/7): yum-utils-4.0.21-3.el8.noarch.rpm                                                                        170 kB/s |  73 kB     00:00
------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                           385 kB/s | 927 kB     00:02
warning: /var/cache/dnf/baseos-398269605bdca3dc/packages/dbus-glib-0.110-2.el8.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID 8483c65d: NOKEY
CentOS Linux 8 - BaseOS                                                                                         1.6 MB/s | 1.6 kB     00:00
Importing GPG key 0x8483C65D:
 Userid     : "CentOS (CentOS Official Signing Key) <security@centos.org>"
 Fingerprint: 99DB 70FA E1D7 CE22 7FB6 4882 05B5 55B3 8483 C65D
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                        1/1
  Installing       : python3-six-1.11.0-8.el8.noarch                                                                                        1/7
  Installing       : python3-dateutil-1:2.6.1-6.el8.noarch                                                                                  2/7
  Installing       : dbus-glib-0.110-2.el8.x86_64                                                                                           3/7
  Running scriptlet: dbus-glib-0.110-2.el8.x86_64                                                                                           3/7
  Installing       : python3-dbus-1.2.4-15.el8.x86_64                                                                                       4/7
  Installing       : python3-dnf-plugins-core-4.0.21-3.el8.noarch                                                                           5/7
  Installing       : dnf-plugins-core-4.0.21-3.el8.noarch                                                                                   6/7
  Installing       : yum-utils-4.0.21-3.el8.noarch                                                                                          7/7
  Running scriptlet: yum-utils-4.0.21-3.el8.noarch                                                                                          7/7
  Verifying        : dbus-glib-0.110-2.el8.x86_64                                                                                           1/7
  Verifying        : dnf-plugins-core-4.0.21-3.el8.noarch                                                                                   2/7
  Verifying        : python3-dateutil-1:2.6.1-6.el8.noarch                                                                                  3/7
  Verifying        : python3-dbus-1.2.4-15.el8.x86_64                                                                                       4/7
  Verifying        : python3-dnf-plugins-core-4.0.21-3.el8.noarch                                                                           5/7
  Verifying        : python3-six-1.11.0-8.el8.noarch                                                                                        6/7
  Verifying        : yum-utils-4.0.21-3.el8.noarch                                                                                          7/7

Installed:
  dbus-glib-0.110-2.el8.x86_64              dnf-plugins-core-4.0.21-3.el8.noarch                  python3-dateutil-1:2.6.1-6.el8.noarch
  python3-dbus-1.2.4-15.el8.x86_64          python3-dnf-plugins-core-4.0.21-3.el8.noarch          python3-six-1.11.0-8.el8.noarch
  yum-utils-4.0.21-3.el8.noarch

Complete!
[root@b402c92dee3f /]#
```





### 第四步：yum进行安装

```sh
yum install -y nginx
```

```sh
[root@b402c92dee3f /]# yum install -y nginx
Failed to set locale, defaulting to C.UTF-8
Last metadata expiration check: 0:08:16 ago on Fri Apr 28 06:56:11 2023.
Dependencies resolved.
================================================================================================================================================
 Package                                  Architecture        Version                                              Repository              Size
================================================================================================================================================
Installing:
 nginx                                    x86_64              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream              570 k
Upgrading:
 ncurses-base                             noarch              6.1-9.20180224.el8                                   baseos                  81 k
 ncurses-libs                             x86_64              6.1-9.20180224.el8                                   baseos                 334 k
 openssl-libs                             x86_64              1:1.1.1k-5.el8_5                                     baseos                 1.5 M
Installing dependencies:
 dejavu-fonts-common                      noarch              2.35-7.el8                                           baseos                  74 k
 dejavu-sans-fonts                        noarch              2.35-7.el8                                           baseos                 1.6 M
 fontconfig                               x86_64              2.13.1-4.el8                                         baseos                 274 k
 fontpackages-filesystem                  noarch              1.44-22.el8                                          baseos                  16 k
 freetype                                 x86_64              2.9.1-4.el8_3.1                                      baseos                 394 k
 gd                                       x86_64              2.2.5-7.el8                                          appstream              144 k
 groff-base                               x86_64              1.22.3-18.el8                                        baseos                 1.0 M
 jbigkit-libs                             x86_64              2.1-14.el8                                           appstream               55 k
 libX11                                   x86_64              1.6.8-5.el8                                          appstream              611 k
 libX11-common                            noarch              1.6.8-5.el8                                          appstream              158 k
 libXau                                   x86_64              1.0.9-3.el8                                          appstream               37 k
 libXpm                                   x86_64              3.5.12-8.el8                                         appstream               58 k
 libjpeg-turbo                            x86_64              1.5.3-12.el8                                         appstream              157 k
 libpng                                   x86_64              2:1.6.34-5.el8                                       baseos                 126 k
 libtiff                                  x86_64              4.0.9-20.el8                                         appstream              188 k
 libwebp                                  x86_64              1.0.0-5.el8                                          appstream              272 k
 libxcb                                   x86_64              1.13.1-1.el8                                         appstream              229 k
 libxslt                                  x86_64              1.1.32-6.el8                                         baseos                 250 k
 ncurses                                  x86_64              6.1-9.20180224.el8                                   baseos                 387 k
 nginx-all-modules                        noarch              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               23 k
 nginx-filesystem                         noarch              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               24 k
 nginx-mod-http-image-filter              x86_64              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               35 k
 nginx-mod-http-perl                      x86_64              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               45 k
 nginx-mod-http-xslt-filter               x86_64              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               33 k
 nginx-mod-mail                           x86_64              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               64 k
 nginx-mod-stream                         x86_64              1:1.14.1-9.module_el8.0.0+184+e34fea82               appstream               85 k
 openssl                                  x86_64              1:1.1.1k-5.el8_5                                     baseos                 709 k
 perl-Carp                                noarch              1.42-396.el8                                         baseos                  30 k
 perl-Data-Dumper                         x86_64              2.167-399.el8                                        baseos                  58 k
 perl-Digest                              noarch              1.17-395.el8                                         appstream               27 k
 perl-Digest-MD5                          x86_64              2.55-396.el8                                         appstream               37 k
 perl-Encode                              x86_64              4:2.97-3.el8                                         baseos                 1.5 M
 perl-Errno                               x86_64              1.28-420.el8                                         baseos                  76 k
 perl-Exporter                            noarch              5.72-396.el8                                         baseos                  34 k
 perl-File-Path                           noarch              2.15-2.el8                                           baseos                  38 k
 perl-File-Temp                           noarch              0.230.600-1.el8                                      baseos                  63 k
 perl-Getopt-Long                         noarch              1:2.50-4.el8                                         baseos                  63 k
 perl-HTTP-Tiny                           noarch              0.074-1.el8                                          baseos                  58 k
 perl-IO                                  x86_64              1.38-420.el8                                         baseos                 142 k
 perl-MIME-Base64                         x86_64              3.15-396.el8                                         baseos                  31 k
 perl-Net-SSLeay                          x86_64              1.88-1.module_el8.3.0+410+ff426aa3                   appstream              379 k
 perl-PathTools                           x86_64              3.74-1.el8                                           baseos                  90 k
 perl-Pod-Escapes                         noarch              1:1.07-395.el8                                       baseos                  20 k
 perl-Pod-Perldoc                         noarch              3.28-396.el8                                         baseos                  86 k
 perl-Pod-Simple                          noarch              1:3.35-395.el8                                       baseos                 213 k
 perl-Pod-Usage                           noarch              4:1.69-395.el8                                       baseos                  34 k
 perl-Scalar-List-Utils                   x86_64              3:1.49-2.el8                                         baseos                  68 k
 perl-Socket                              x86_64              4:2.027-3.el8                                        baseos                  59 k
 perl-Storable                            x86_64              1:3.11-3.el8                                         baseos                  98 k
 perl-Term-ANSIColor                      noarch              4.06-396.el8                                         baseos                  46 k
 perl-Term-Cap                            noarch              1.17-395.el8                                         baseos                  23 k
 perl-Text-ParseWords                     noarch              3.30-395.el8                                         baseos                  18 k
 perl-Text-Tabs+Wrap                      noarch              2013.0523-395.el8                                    baseos                  24 k
 perl-Time-Local                          noarch              1:1.280-1.el8                                        baseos                  34 k
 perl-URI                                 noarch              1.73-3.el8                                           appstream              116 k
 perl-Unicode-Normalize                   x86_64              1.25-396.el8                                         baseos                  82 k
 perl-constant                            noarch              1.33-396.el8                                         baseos                  25 k
 perl-interpreter                         x86_64              4:5.26.3-420.el8                                     baseos                 6.3 M
 perl-libnet                              noarch              3.11-3.el8                                           appstream              121 k
 perl-libs                                x86_64              4:5.26.3-420.el8                                     baseos                 1.6 M
 perl-macros                              x86_64              4:5.26.3-420.el8                                     baseos                  72 k
 perl-parent                              noarch              1:0.237-1.el8                                        baseos                  20 k
 perl-podlators                           noarch              4.11-1.el8                                           baseos                 118 k
 perl-threads                             x86_64              1:2.21-2.el8                                         baseos                  61 k
 perl-threads-shared                      x86_64              1.58-2.el8                                           baseos                  48 k
Installing weak dependencies:
 openssl-pkcs11                           x86_64              0.4.10-2.el8                                         baseos                  66 k
 perl-IO-Socket-IP                        noarch              0.39-5.el8                                           appstream               47 k
 perl-IO-Socket-SSL                       noarch              2.066-4.module_el8.3.0+410+ff426aa3                  appstream              298 k
 perl-Mozilla-CA                          noarch              20160104-7.module_el8.3.0+416+dee7bcef               appstream               15 k
Enabling module streams:
 nginx                                                        1.14
 perl                                                         5.26
 perl-IO-Socket-SSL                                           2.066
 perl-libwww-perl                                             6.34

Transaction Summary
================================================================================================================================================
Install  70 Packages
Upgrade   3 Packages

Total download size: 22 M
Downloading Packages:
(1/73): jbigkit-libs-2.1-14.el8.x86_64.rpm                                                                       39 kB/s |  55 kB     00:01
(2/73): gd-2.2.5-7.el8.x86_64.rpm                                                                                86 kB/s | 144 kB     00:01
(3/73): libXau-1.0.9-3.el8.x86_64.rpm                                                                            81 kB/s |  37 kB     00:00
(4/73): libX11-common-1.6.8-5.el8.noarch.rpm                                                                    138 kB/s | 158 kB     00:01
(5/73): libXpm-3.5.12-8.el8.x86_64.rpm                                                                          120 kB/s |  58 kB     00:00
(6/73): libtiff-4.0.9-20.el8.x86_64.rpm                                                                         279 kB/s | 188 kB     00:00
(7/73): libjpeg-turbo-1.5.3-12.el8.x86_64.rpm                                                                   192 kB/s | 157 kB     00:00
(8/73): libxcb-1.13.1-1.el8.x86_64.rpm                                                                          456 kB/s | 229 kB     00:00
......
(70/73): perl-libs-5.26.3-420.el8.x86_64.rpm                                                                    361 kB/s | 1.6 MB     00:04
(71/73): ncurses-libs-6.1-9.20180224.el8.x86_64.rpm                                                             657 kB/s | 334 kB     00:00
(72/73): perl-threads-shared-1.58-2.el8.x86_64.rpm                                                               20 kB/s |  48 kB     00:02
(73/73): openssl-libs-1.1.1k-5.el8_5.x86_64.rpm                                                                 331 kB/s | 1.5 MB     00:04
------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                           713 kB/s |  22 MB     00:30
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                        1/1
  Upgrading        : openssl-libs-1:1.1.1k-5.el8_5.x86_64                                                                                  1/76
  Running scriptlet: openssl-libs-1:1.1.1k-5.el8_5.x86_64                                                                                  1/76
  Installing       : openssl-1:1.1.1k-5.el8_5.x86_64                                                                                       2/76
  Installing       : openssl-pkcs11-0.4.10-2.el8.x86_64                                                                                    3/76
  Installing       : libpng-2:1.6.34-5.el8.x86_64                                                                           ......                                                                           74/76
  Cleanup          : ncurses-base-6.1-7.20180224.el8.noarch                                                                               75/76
  Cleanup          : openssl-libs-1:1.1.1g-15.el8_3.x86_64                                                                                76/76
  Running scriptlet: openssl-libs-1:1.1.1g-15.el8_3.x86_64                                                                                76/76
  Running scriptlet: fontconfig-2.13.1-4.el8.x86_64                                                                                       76/76
  Verifying        : gd-2.2.5-7.el8.x86_64                                                                                                 1/76
  Verifying        : jbigkit-libs-2.1-14.el8.x86_64                                                                                        2/76
  Verifying        : libX11-1.6.8-5.el8.x86_64                                                                                             3/76
  Verifying        : libX11-common-1.6.8-5.el8.noarch                                                                                      4/76
......                                                                            74/76
  Verifying        : openssl-libs-1:1.1.1k-5.el8_5.x86_64                                                                                 75/76
  Verifying        : openssl-libs-1:1.1.1g-15.el8_3.x86_64                                                                                76/76

Upgraded:
  ncurses-base-6.1-9.20180224.el8.noarch          ncurses-libs-6.1-9.20180224.el8.x86_64          openssl-libs-1:1.1.1k-5.el8_5.x86_64
Installed:
  dejavu-fonts-common-2.35-7.el8.noarch                                      dejavu-sans-fonts-2.35-7.el8.noarch
  fontconfig-2.13.1-4.el8.x86_64                                             fontpackages-filesystem-1.44-22.el8.noarch
  freetype-2.9.1-4.el8_3.1.x86_64                                            gd-2.2.5-7.el8.x86_64
  groff-base-1.22.3-18.el8.x86_64                                            jbigkit-libs-2.1-14.el8.x86_64
  libX11-1.6.8-5.el8.x86_64                                                  libX11-common-1.6.8-5.el8.noarch
  libXau-1.0.9-3.el8.x86_64                                                  libXpm-3.5.12-8.el8.x86_64
  libjpeg-turbo-1.5.3-12.el8.x86_64                                          libpng-2:1.6.34-5.el8.x86_64
  libtiff-4.0.9-20.el8.x86_64                                                libwebp-1.0.0-5.el8.x86_64
  libxcb-1.13.1-1.el8.x86_64                                                 libxslt-1.1.32-6.el8.x86_64
  ncurses-6.1-9.20180224.el8.x86_64                                          nginx-1:1.14.1-9.module_el8.0.0+184+e34fea82.x86_64
  nginx-all-modules-1:1.14.1-9.module_el8.0.0+184+e34fea82.noarch            nginx-filesystem-1:1.14.1-9.module_el8.0.0+184+e34fea82.noarch
  nginx-mod-http-image-filter-1:1.14.1-9.module_el8.0.0+184+e34fea82.x86_64  nginx-mod-http-perl-1:1.14.1-9.module_el8.0.0+184+e34fea82.x86_64
  nginx-mod-http-xslt-filter-1:1.14.1-9.module_el8.0.0+184+e34fea82.x86_64   nginx-mod-mail-1:1.14.1-9.module_el8.0.0+184+e34fea82.x86_64
  nginx-mod-stream-1:1.14.1-9.module_el8.0.0+184+e34fea82.x86_64             openssl-1:1.1.1k-5.el8_5.x86_64
  openssl-pkcs11-0.4.10-2.el8.x86_64                                         perl-Carp-1.42-396.el8.noarch
  perl-Data-Dumper-2.167-399.el8.x86_64                                      perl-Digest-1.17-395.el8.noarch
  perl-Digest-MD5-2.55-396.el8.x86_64                                        perl-Encode-4:2.97-3.el8.x86_64
  perl-Errno-1.28-420.el8.x86_64                                             perl-Exporter-5.72-396.el8.noarch
  perl-File-Path-2.15-2.el8.noarch                                           perl-File-Temp-0.230.600-1.el8.noarch
  perl-Getopt-Long-1:2.50-4.el8.noarch                                       perl-HTTP-Tiny-0.074-1.el8.noarch
  perl-IO-1.38-420.el8.x86_64                                                perl-IO-Socket-IP-0.39-5.el8.noarch
  perl-IO-Socket-SSL-2.066-4.module_el8.3.0+410+ff426aa3.noarch              perl-MIME-Base64-3.15-396.el8.x86_64
  perl-Mozilla-CA-20160104-7.module_el8.3.0+416+dee7bcef.noarch              perl-Net-SSLeay-1.88-1.module_el8.3.0+410+ff426aa3.x86_64
  perl-PathTools-3.74-1.el8.x86_64                                           perl-Pod-Escapes-1:1.07-395.el8.noarch
  perl-Pod-Perldoc-3.28-396.el8.noarch                                       perl-Pod-Simple-1:3.35-395.el8.noarch
  perl-Pod-Usage-4:1.69-395.el8.noarch                                       perl-Scalar-List-Utils-3:1.49-2.el8.x86_64
  perl-Socket-4:2.027-3.el8.x86_64                                           perl-Storable-1:3.11-3.el8.x86_64
  perl-Term-ANSIColor-4.06-396.el8.noarch                                    perl-Term-Cap-1.17-395.el8.noarch
  perl-Text-ParseWords-3.30-395.el8.noarch                                   perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch
  perl-Time-Local-1:1.280-1.el8.noarch                                       perl-URI-1.73-3.el8.noarch
  perl-Unicode-Normalize-1.25-396.el8.x86_64                                 perl-constant-1.33-396.el8.noarch
  perl-interpreter-4:5.26.3-420.el8.x86_64                                   perl-libnet-3.11-3.el8.noarch
  perl-libs-4:5.26.3-420.el8.x86_64                                          perl-macros-4:5.26.3-420.el8.x86_64
  perl-parent-1:0.237-1.el8.noarch                                           perl-podlators-4.11-1.el8.noarch
  perl-threads-1:2.21-2.el8.x86_64                                           perl-threads-shared-1.58-2.el8.x86_64

Complete!
[root@b402c92dee3f /]#
```





### 第五步：查看nginx的安装位置

```sh
whereis nginx
```

```sh
[root@b402c92dee3f /]# whereis nginx
nginx: /usr/sbin/nginx /usr/lib64/nginx /etc/nginx /usr/share/nginx /usr/share/man/man3/nginx.3pm.gz /usr/share/man/man8/nginx.8.gz
[root@b402c92dee3f /]# type nginx
nginx is /usr/sbin/nginx
[root@b402c92dee3f /]#
```





### 第六步：进入可执行文件目录

```sh
cd /usr/sbin
```



### 第七步：启动

```sh
./nginx
```



http://localhost:8090/

![image-20230428151227983](img/Nginx学习笔记/image-20230428151227983.png)



### 第八步：查看日志

```sh
[root@b402c92dee3f sbin]# cd /var/log/nginx
[root@b402c92dee3f nginx]# ll
bash: ll: command not found
[root@b402c92dee3f nginx]# ls -l
total 8
-rw-r--r-- 1 root root 1147 Apr 28 07:12 access.log
-rw-r--r-- 1 root root  248 Apr 28 07:10 error.log
[root@b402c92dee3f nginx]# cat access.log
172.17.0.1 - - [28/Apr/2023:07:10:52 +0000] "GET / HTTP/1.1" 200 4057 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:07:10:52 +0000] "GET /nginx-logo.png HTTP/1.1" 200 368 "http://localhost:8090/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:07:10:52 +0000] "GET /poweredby.png HTTP/1.1" 200 4148 "http://localhost:8090/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:07:10:52 +0000] "GET /favicon.ico HTTP/1.1" 404 3971 "http://localhost:8090/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
172.17.0.1 - - [28/Apr/2023:07:12:34 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.58" "-"
[root@b402c92dee3f nginx]# cat error.log
2023/04/28 07:10:52 [error] 117#0: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: _, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8090", referrer: "http://localhost:8090/"
[root@b402c92dee3f nginx]#
```









## Windows安装

### 第一步：下载压缩包

http://nginx.org/en/download.html



http://nginx.org/download/nginx-1.24.0.zip





### 第二步：解压

解压后的目录：

```sh
PS D:\opensoft\nginx-1.24.0> ls


    目录: D:\opensoft\nginx-1.24.0


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/4/11     23:31                conf
d-----         2023/4/11     23:31                contrib
d-----         2023/4/11     23:31                docs
d-----         2023/4/11     23:31                html
d-----         2023/4/11     23:31                logs
d-----         2023/4/11     23:31                temp
------         2023/4/11     23:29        3811328 nginx.exe


PS D:\opensoft\nginx-1.24.0>
```





### 第三步：运行

```sh
./nginx
```

或者

```sh
start ./nginx
```





### 第四步：访问

http://localhost/



![image-20230429133052564](img/Nginx学习笔记/image-20230429133052564.png)



### 第五步：查看访问日志

```sh
PS D:\opensoft\nginx-1.24.0> ls


    目录: D:\opensoft\nginx-1.24.0


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/4/11     23:31                conf
d-----         2023/4/11     23:31                contrib
d-----         2023/4/11     23:31                docs
d-----         2023/4/11     23:31                html
d-----         2023/4/29     13:31                logs
d-----         2023/4/29     13:29                temp
------         2023/4/11     23:29        3811328 nginx.exe


PS D:\opensoft\nginx-1.24.0> cd logs
PS D:\opensoft\nginx-1.24.0\logs> ls


    目录: D:\opensoft\nginx-1.24.0\logs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2023/4/29     13:30            843 access.log
-a----         2023/4/29     13:31            348 error.log
-a----         2023/4/29     13:31              7 nginx.pid


PS D:\opensoft\nginx-1.24.0\logs> cat .\access.log
127.0.0.1 - - [29/Apr/2023:13:30:25 +0800] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:30:25 +0800] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:30:27 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:30:27 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:25 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:25 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:26 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:26 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:26 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:26 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:26 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:26 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
127.0.0.1 - - [29/Apr/2023:13:32:27 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
PS D:\opensoft\nginx-1.24.0\logs> cat .\error.log
2023/04/29 13:30:25 [error] 36316#34412: *1 CreateFile() "D:\opensoft\nginx-1.24.0/html/favicon.ico" failed (2: The system cannot find the file specified), client: 127.0.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost", referrer: "http://localhost/"
2023/04/29 13:31:32 [notice] 15804#22388: signal process started
PS D:\opensoft\nginx-1.24.0\logs> cat .\nginx.pid
20112
PS D:\opensoft\nginx-1.24.0\logs>
```





### 第六步：添加至环境变量



在设置中打开系统属性窗口，点击高级



![image-20230429140928126](img/Nginx学习笔记/image-20230429140928126.png)



找到Path，点击编辑

![image-20230429141015933](img/Nginx学习笔记/image-20230429141015933.png)



点击新建

![image-20230429141049458](img/Nginx学习笔记/image-20230429141049458.png)





![image-20230429141150083](img/Nginx学习笔记/image-20230429141150083.png)



测试环境变量配置是否成功，在桌面打开控制台

```sh
PS C:\Users\mao\Desktop> nginx -v
nginx version: nginx/1.24.0
PS C:\Users\mao\Desktop>
```











# Nginx目录结构

在使用Nginx之前，先对安装好的Nginx目录文件进行一个分析

```sh
PS D:\opensoft\nginx-1.24.0> tree /F
文件夹 PATH 列表
卷序列号为 5E84-66B0
D:.
│  nginx.exe
│
├─conf
│      fastcgi.conf
│      fastcgi_params
│      koi-utf
│      koi-win
│      mime.types
│      nginx.conf
│      scgi_params
│      uwsgi_params
│      win-utf
│
├─contrib
│  │  geo2nginx.pl
│  │  README
│  │
│  ├─unicode2nginx
│  │      koi-utf
│  │      unicode-to-nginx.pl
│  │      win-utf
│  │
│  └─vim
│      ├─ftdetect
│      │      nginx.vim
│      │
│      ├─ftplugin
│      │      nginx.vim
│      │
│      ├─indent
│      │      nginx.vim
│      │
│      └─syntax
│              nginx.vim
│
├─docs
│      CHANGES
│      CHANGES.ru
│      LICENSE
│      OpenSSL.LICENSE
│      PCRE.LICENCE
│      README
│      zlib.LICENSE
│
├─html
│      50x.html
│      index.html
│
├─logs
│      access.log
│      error.log
│
└─temp
    ├─client_body_temp
    ├─fastcgi_temp
    ├─proxy_temp
    ├─scgi_temp
    └─uwsgi_temp
PS D:\opensoft\nginx-1.24.0>
```







## conf

**nginx所有配置文件目录**

 CGI(Common Gateway Interface)通用网关接口，主要解决的问题是从客户端发送一个请求和数据，服务端获取到请求和数据后可以调用调用CGI程序	处理及相应结果给客户端的一种标准规范



### fastcgi.conf

**fastcgi相关配置文件**



```sh
PS D:\opensoft\nginx-1.24.0\conf> cat .\fastcgi.conf

fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;

fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;
fastcgi_param  REQUEST_SCHEME     $scheme;
fastcgi_param  HTTPS              $https if_not_empty;

fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param  REDIRECT_STATUS    200;
PS D:\opensoft\nginx-1.24.0\conf>
```





### fastcgi_params

**fastcgi的参数文件**



```sh
PS D:\opensoft\nginx-1.24.0\conf> cat .\fastcgi_params

fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;

fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;
fastcgi_param  REQUEST_SCHEME     $scheme;
fastcgi_param  HTTPS              $https if_not_empty;

fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param  REDIRECT_STATUS    200;
PS D:\opensoft\nginx-1.24.0\conf>
```





### scgi_params

**scgi的参数文件**



```sh
PS D:\opensoft\nginx-1.24.0\conf> cat .\scgi_params

scgi_param  REQUEST_METHOD     $request_method;
scgi_param  REQUEST_URI        $request_uri;
scgi_param  QUERY_STRING       $query_string;
scgi_param  CONTENT_TYPE       $content_type;

scgi_param  DOCUMENT_URI       $document_uri;
scgi_param  DOCUMENT_ROOT      $document_root;
scgi_param  SCGI               1;
scgi_param  SERVER_PROTOCOL    $server_protocol;
scgi_param  REQUEST_SCHEME     $scheme;
scgi_param  HTTPS              $https if_not_empty;

scgi_param  REMOTE_ADDR        $remote_addr;
scgi_param  REMOTE_PORT        $remote_port;
scgi_param  SERVER_PORT        $server_port;
scgi_param  SERVER_NAME        $server_name;
PS D:\opensoft\nginx-1.24.0\conf>
```





### uwsgi_params

**uwsgi的参数文件**



```sh
PS D:\opensoft\nginx-1.24.0\conf> cat .\uwsgi_params

uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;
PS D:\opensoft\nginx-1.24.0\conf>
```





### mime.types

**记录的是HTTP协议中的Content-Type的值和文件后缀名的对应关系**



```sh
PS D:\opensoft\nginx-1.24.0\conf> cat .\mime.types

types {
    text/html                                        html htm shtml;
    text/css                                         css;
    text/xml                                         xml;
    image/gif                                        gif;
    image/jpeg                                       jpeg jpg;
    application/javascript                           js;
    application/atom+xml                             atom;
    application/rss+xml                              rss;

    text/mathml                                      mml;
    text/plain                                       txt;
    text/vnd.sun.j2me.app-descriptor                 jad;
    text/vnd.wap.wml                                 wml;
    text/x-component                                 htc;

    image/avif                                       avif;
    image/png                                        png;
    image/svg+xml                                    svg svgz;
    image/tiff                                       tif tiff;
    image/vnd.wap.wbmp                               wbmp;
    image/webp                                       webp;
    image/x-icon                                     ico;
    image/x-jng                                      jng;
    image/x-ms-bmp                                   bmp;

    font/woff                                        woff;
    font/woff2                                       woff2;

    application/java-archive                         jar war ear;
    application/json                                 json;
    application/mac-binhex40                         hqx;
    application/msword                               doc;
    application/pdf                                  pdf;
    application/postscript                           ps eps ai;
    application/rtf                                  rtf;
    application/vnd.apple.mpegurl                    m3u8;
    application/vnd.google-earth.kml+xml             kml;
    application/vnd.google-earth.kmz                 kmz;
    application/vnd.ms-excel                         xls;
    application/vnd.ms-fontobject                    eot;
    application/vnd.ms-powerpoint                    ppt;
    application/vnd.oasis.opendocument.graphics      odg;
    application/vnd.oasis.opendocument.presentation  odp;
    application/vnd.oasis.opendocument.spreadsheet   ods;
    application/vnd.oasis.opendocument.text          odt;
    application/vnd.openxmlformats-officedocument.presentationml.presentation
                                                     pptx;
    application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
                                                     xlsx;
    application/vnd.openxmlformats-officedocument.wordprocessingml.document
                                                     docx;
    application/vnd.wap.wmlc                         wmlc;
    application/wasm                                 wasm;
    application/x-7z-compressed                      7z;
    application/x-cocoa                              cco;
    application/x-java-archive-diff                  jardiff;
    application/x-java-jnlp-file                     jnlp;
    application/x-makeself                           run;
    application/x-perl                               pl pm;
    application/x-pilot                              prc pdb;
    application/x-rar-compressed                     rar;
    application/x-redhat-package-manager             rpm;
    application/x-sea                                sea;
    application/x-shockwave-flash                    swf;
    application/x-stuffit                            sit;
    application/x-tcl                                tcl tk;
    application/x-x509-ca-cert                       der pem crt;
    application/x-xpinstall                          xpi;
    application/xhtml+xml                            xhtml;
    application/xspf+xml                             xspf;
    application/zip                                  zip;

    application/octet-stream                         bin exe dll;
    application/octet-stream                         deb;
    application/octet-stream                         dmg;
    application/octet-stream                         iso img;
    application/octet-stream                         msi msp msm;

    audio/midi                                       mid midi kar;
    audio/mpeg                                       mp3;
    audio/ogg                                        ogg;
    audio/x-m4a                                      m4a;
    audio/x-realaudio                                ra;

    video/3gpp                                       3gpp 3gp;
    video/mp2t                                       ts;
    video/mp4                                        mp4;
    video/mpeg                                       mpeg mpg;
    video/quicktime                                  mov;
    video/webm                                       webm;
    video/x-flv                                      flv;
    video/x-m4v                                      m4v;
    video/x-mng                                      mng;
    video/x-ms-asf                                   asx asf;
    video/x-ms-wmv                                   wmv;
    video/x-msvideo                                  avi;
}
PS D:\opensoft\nginx-1.24.0\conf>
```





### nginx.conf

**这个是Nginx的核心配置文件**



```sh
PS D:\opensoft\nginx-1.24.0\conf> cat .\nginx.conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
PS D:\opensoft\nginx-1.24.0\conf>
```





### koi-utf、koi-win和win-utf

**这三个文件都是与编码转换映射相关的配置文件，用来将一种编码转换成另一种编码**







## html

**存放nginx自带的两个静态的html页面**



### 50x.html

**访问失败后的失败页面**



```sh
PS D:\opensoft\nginx-1.24.0\html> cat .\50x.html
<!DOCTYPE html>
<html>
<head>
<title>Error</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>An error occurred.</h1>
<p>Sorry, the page you are looking for is currently unavailable.<br/>
Please try again later.</p>
<p>If you are the system administrator of this resource then you should check
the error log for details.</p>
<p><em>Faithfully yours, nginx.</em></p>
</body>
</html>
PS D:\opensoft\nginx-1.24.0\html>
```



![image-20230429135549993](img/Nginx学习笔记/image-20230429135549993.png)



### index.html

**成功访问的默认首页**



```sh
PS D:\opensoft\nginx-1.24.0\html> cat .\index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
PS D:\opensoft\nginx-1.24.0\html>
```



![image-20230429135648553](img/Nginx学习笔记/image-20230429135648553.png)





## logs

**日志文件，当nginx服务器启动后，这里面会有 access.log 、error.log 和nginx.pid三个文件出现**



### access.log

**访问日志，用户访问时会留下一条记录**

包含IP地址、访问时间、请求方式、请求的URL、协议版本、状态码、浏览器UA标识等

```sh
127.0.0.1 - - [29/Apr/2023:13:32:27 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.64"
```



### error.log

**错误日志**

```sh
2023/04/29 13:30:25 [error] 36316#34412: *1 CreateFile() "D:\opensoft\nginx-1.24.0/html/favicon.ico" failed (2: The system cannot find the file specified), client: 127.0.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost", referrer: "http://localhost/"
2023/04/29 13:31:32 [notice] 15804#22388: signal process started
2023/04/29 13:33:42 [notice] 11564#25356: signal process started
```



### nginx.pid

**当nginx启动后，每一个进程都会有一个进程的PID，Nginx的PID就保存在这里**

```sh
PS D:\opensoft\nginx-1.24.0\logs> cat .\nginx.pid
32864
PS D:\opensoft\nginx-1.24.0\logs>
```



当前Master进程的PID为32864

![image-20230429140513674](img/Nginx学习笔记/image-20230429140513674.png)







## sbin

**sbin是存放执行程序文件的目录（Linux）**

Windows可执行文件放在跟目录下

用来控制Nginx的启动和停止等相关的命令









# Nginx服务器启停命令

有两种方式：

* Nginx服务的信号控制

* Nginx的命令行控制





## Nginx服务的信号控制

Nginx默认采用的是多进程的方式来工作的，当将Nginx启动后，我们通过`ps -ef | grep nginx`命令可以查看启动的相关进程

Nginx后台进程中包含一个master进程和多个worker进程，master进程主要用来管理worker进程，包含接收外界的信息，并将接收到的信号发送给各个worker进程，监控worker进程的状态，当worker进程出现异常退出后，会自动重新启动新的worker进程。而worker进程则是专门用来处理用户请求的，各个worker进程之间是平等的并且相互独立，处理请求的机会也是一样的。



|   信号   |                            作用                            |
| :------: | :--------------------------------------------------------: |
| TERM/INT |                      立即关闭整个服务                      |
|   QUIT   |                    "优雅"地关闭整个服务                    |
|   HUP    |            重读配置文件并使用服务对新配置项生效            |
|   USR1   |           重新打开日志文件，可以用来进行日志切割           |
|   USR2   |                  平滑升级到最新版的nginx                   |
|  WINCH   | 所有子进程不在接收处理新连接，相当于给work进程发送QUIT指令 |





调用命令为`kill -signal PID`

signal:即为信号；PID即为获取到的master线程ID



发送TERM/INT信号给master进程，会将Nginx服务立即关闭

```sh
kill -TERM PID / kill -TERM `cat /usr/local/nginx/logs/nginx.pid`
kill -INT PID / kill -INT `cat /usr/local/nginx/logs/nginx.pid`
```



发送QUIT信号给master进程，master进程会控制所有的work进程不再接收新的请求，等所有请求处理完后，在把进程都关闭掉

```sh
kill -QUIT PID / kill -TERM `cat /usr/local/nginx/logs/nginx.pid`
```



发送HUP信号给master进程，master进程会把控制旧的work进程不再接收新的请求，等处理完请求后将旧的work进程关闭掉，然后根据nginx的配置文件重新启动新的work进程

```sh
kill -HUP PID / kill -TERM `cat /usr/local/nginx/logs/nginx.pid`
```



发送USR1信号给master进程，告诉Nginx重新开启日志文件

```sh
kill -USR1 PID / kill -TERM `cat /usr/local/nginx/logs/nginx.pid`
```



发送USR2信号给master进程，告诉master进程要平滑升级，这个时候，会重新开启对应的master进程和work进程，整个系统中将会有两个master进程，并且新的master进程的PID会被记录在`/usr/local/nginx/logs/nginx.pid`而之前的旧的master进程PID会被记录

```sh
kill -USR2 PID / kill -USR2 `cat /usr/local/nginx/logs/nginx.pid`
```

```sh
kill -QUIT PID / kill -QUIT `cat /usr/local/nginx/logs/nginx.pid.oldbin`
```



发送WINCH信号给master进程,让master进程控制不让所有的work进程在接收新的请求了，请求处理完后关闭work进程。注意master进程不会被关闭掉

```sh
kill -WINCH PID /kill -WINCH`cat /usr/local/nginx/logs/nginx.pid`
```





## Nginx的命令行控制

此方式是通过Nginx安装目录下的sbin下的可执行文件nginx来进行Nginx状态的控制，我们可以通过`nginx -h`来查看都有哪些参数可以用：

```sh
root@4f3ddbf99a0e:/# nginx -h
nginx version: nginx/1.23.4
Usage: nginx [-?hvVtTq] [-s signal] [-p prefix]
             [-e filename] [-c filename] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /etc/nginx/)
  -e filename   : set error log file (default: /var/log/nginx/error.log)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file

root@4f3ddbf99a0e:/#
```



### -h

显示帮助信息



### -v

打印版本号信息并退出

```sh
root@4f3ddbf99a0e:/# nginx -v
nginx version: nginx/1.23.4
root@4f3ddbf99a0e:/#
```



### -V

打印版本号信息和配置信息并退出

```sh
root@4f3ddbf99a0e:/# nginx -V
nginx version: nginx/1.23.4
built by gcc 10.2.1 20210110 (Debian 10.2.1-6)
built with OpenSSL 1.1.1n  15 Mar 2022
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -ffile-prefix-map=/data/builder/debuild/nginx-1.23.4/debian/debuild-base/nginx-1.23.4=. -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'
root@4f3ddbf99a0e:/#
```



### -t

测试nginx的配置文件语法是否正确并退出

```sh
root@4f3ddbf99a0e:/# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@4f3ddbf99a0e:/#
```



### -T

测试nginx的配置文件语法是否正确并列出用到的配置文件信息然后退出

```sh
root@4f3ddbf99a0e:/# nginx -T
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
# configuration file /etc/nginx/nginx.conf:

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}

# configuration file /etc/nginx/mime.types:

types {
    text/html                                        html htm shtml;
    text/css                                         css;
    text/xml                                         xml;
    image/gif                                        gif;
    image/jpeg                                       jpeg jpg;
    application/javascript                           js;
    application/atom+xml                             atom;
    application/rss+xml                              rss;

    text/mathml                                      mml;
    text/plain                                       txt;
    text/vnd.sun.j2me.app-descriptor                 jad;
    text/vnd.wap.wml                                 wml;
    text/x-component                                 htc;

    image/avif                                       avif;
    image/png                                        png;
    image/svg+xml                                    svg svgz;
    image/tiff                                       tif tiff;
    image/vnd.wap.wbmp                               wbmp;
    image/webp                                       webp;
    image/x-icon                                     ico;
    image/x-jng                                      jng;
    image/x-ms-bmp                                   bmp;

    font/woff                                        woff;
    font/woff2                                       woff2;

    application/java-archive                         jar war ear;
    application/json                                 json;
    application/mac-binhex40                         hqx;
    application/msword                               doc;
    application/pdf                                  pdf;
    application/postscript                           ps eps ai;
    application/rtf                                  rtf;
    application/vnd.apple.mpegurl                    m3u8;
    application/vnd.google-earth.kml+xml             kml;
    application/vnd.google-earth.kmz                 kmz;
    application/vnd.ms-excel                         xls;
    application/vnd.ms-fontobject                    eot;
    application/vnd.ms-powerpoint                    ppt;
    application/vnd.oasis.opendocument.graphics      odg;
    application/vnd.oasis.opendocument.presentation  odp;
    application/vnd.oasis.opendocument.spreadsheet   ods;
    application/vnd.oasis.opendocument.text          odt;
    application/vnd.openxmlformats-officedocument.presentationml.presentation
                                                     pptx;
    application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
                                                     xlsx;
    application/vnd.openxmlformats-officedocument.wordprocessingml.document
                                                     docx;
    application/vnd.wap.wmlc                         wmlc;
    application/wasm                                 wasm;
    application/x-7z-compressed                      7z;
    application/x-cocoa                              cco;
    application/x-java-archive-diff                  jardiff;
    application/x-java-jnlp-file                     jnlp;
    application/x-makeself                           run;
    application/x-perl                               pl pm;
    application/x-pilot                              prc pdb;
    application/x-rar-compressed                     rar;
    application/x-redhat-package-manager             rpm;
    application/x-sea                                sea;
    application/x-shockwave-flash                    swf;
    application/x-stuffit                            sit;
    application/x-tcl                                tcl tk;
    application/x-x509-ca-cert                       der pem crt;
    application/x-xpinstall                          xpi;
    application/xhtml+xml                            xhtml;
    application/xspf+xml                             xspf;
    application/zip                                  zip;

    application/octet-stream                         bin exe dll;
    application/octet-stream                         deb;
    application/octet-stream                         dmg;
    application/octet-stream                         iso img;
    application/octet-stream                         msi msp msm;

    audio/midi                                       mid midi kar;
    audio/mpeg                                       mp3;
    audio/ogg                                        ogg;
    audio/x-m4a                                      m4a;
    audio/x-realaudio                                ra;

    video/3gpp                                       3gpp 3gp;
    video/mp2t                                       ts;
    video/mp4                                        mp4;
    video/mpeg                                       mpeg mpg;
    video/quicktime                                  mov;
    video/webm                                       webm;
    video/x-flv                                      flv;
    video/x-m4v                                      m4v;
    video/x-mng                                      mng;
    video/x-ms-asf                                   asx asf;
    video/x-ms-wmv                                   wmv;
    video/x-msvideo                                  avi;
}

# configuration file /etc/nginx/conf.d/default.conf:
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}


root@4f3ddbf99a0e:/#
```





### -q

在配置测试期间禁止显示非错误消息



### -s

signal信号，后面可以跟 ：

* stop：快速关闭，类似于TERM/INT信号的作用
* quit：优雅的关闭，类似于QUIT信号的作用
* reopen：重新打开日志文件，类似于USR1信号的作用
* reload：类似于HUP信号的作用



```sh
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0> nginx -s stop
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0> nginx -s quit
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0> nginx -s reopen
PS D:\opensoft\nginx-1.24.0> nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```





### -p

prefix，指定Nginx的prefix路径，(Linux默认为: /usr/local/nginx/)



### -c

filename，指定Nginx的配置文件路径，(Linux默认为: conf/nginx.conf)



### -g

用来补充Nginx配置文件，向Nginx服务指定启动时应用全局的配置







# 版本平滑升级和新增模块

如果想对Nginx的版本进行更新，或者要应用一些新的模块，最简单的做法就是停止当前的Nginx服务，然后开启新的Nginx服务。但是这样会导致在一段时间内，用户是无法访问服务器。为了解决这个问题，我们就需要用到Nginx服务器提供的平滑升级功能。

在整个过程中，其实Nginx是一直对外提供服务的。并且当Nginx的服务器启动成功后，我们是可以通过浏览器进行直接访问的。



有两种方案：

* 使用Nginx服务信号完成Nginx的升级
* 使用Nginx安装目录的make命令完成升级（Linux）





## 使用Nginx服务信号进行升级

### 第一步：备份可执行文件

```sh
cd /usr/local/nginx/sbin
mv nginx nginxold
```



### 第二步：拷贝

拷贝新的可执行文件到原来`/usr/local/nginx/sbin`目录下



### 第三步:发送信号

发送信号USR2

```sh
kill -USR2 当前PID
```



发送信号QUIT

```sh
kill -QUIT 当前PID
```







## 使用make命令完成升级

### 第一步：备份可执行文件

```sh
cd /usr/local/nginx/sbin
mv nginx nginxold
```



### 第二步：拷贝

拷贝新的可执行文件到原来`/usr/local/nginx/sbin`目录下



### 第三步：执行make upgrade

进入到安装目录，执行`make upgrade`



### 第四步：查看是否更新成功

```sh
./nginx -v
```











# Nginx核心配置文件

配置文件默认内容：

```sh
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}
```



nginx.conf配置文件中默认有三大块：

* 全局块
* events块
* http块



http块中可以配置多个server块，每个server块又可以配置多个location块





## 全局块

### user

user**用于配置运行Nginx服务器的worker进程的用户和用户组**

**使用user指令可以指定启动运行工作进程的用户及用户组，这样对于系统的权限访问控制的更加精细，也更加安全。**



|  语法  | user user [group] |
| :----: | :---------------: |
| 默认值 |      nobody       |
|  位置  |      全局块       |



该属性也可以在编译的时候指定，语法如下`./configure --user=user --group=group`,如果两个地方都进行了设置，最终生效的是配置文件中的配置



使用示例：

```sh
worker_processes  1;
user mao;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### master_process

master_process**用来指定是否开启工作进程。**



|  语法  | master_process on\|off; |
| :----: | :---------------------: |
| 默认值 |   master_process on;    |
|  位置  |         全局块          |



使用示例：

```sh
worker_processes  1;
# 开启工作进程
master_process on;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### worker_processes

**用于配置Nginx生成工作进程的数量**，这个是Nginx服务器实现并发处理服务的关键所在。理论上来说workder process的值越大，可以支持的并发处理量也越多，但事实上这个值的设定是需要受到来自服务器自身的限制，建议将该值和服务器CPU的内核数保存一致



|  语法  | worker_processes     num/auto; |
| :----: | :----------------------------: |
| 默认值 |               1                |
|  位置  |             全局块             |



使用示例：

```sh
# 开启工作进程
master_process on;
# 配置Nginx生成工作进程的数量
worker_processes 32;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### daemon

**设定Nginx是否以守护进程的方式启动**



|  语法  | daemon on\|off; |
| :----: | :-------------: |
| 默认值 |   daemon on;    |
|  位置  |     全局块      |



使用示例：

```sh
worker_processes  1;
# 以守护进程的方式启动
daemon on;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### pid

**用来配置Nginx当前master进程的进程号ID存储的文件路径**



|  语法  |                  pid file;                  |
| :----: | :-----------------------------------------: |
| 默认值 | Linux默认为:/usr/local/nginx/logs/nginx.pid |
|  位置  |                   全局块                    |



该属性可以通过`./configure --pid-path=PATH`来指定



使用示例：

```sh
worker_processes  1;
# 当前master进程的进程号ID存储的文件路径
pid ./nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### error_log

**用来配置Nginx的错误日志存放路径**



|  语法  |   error_log  file [日志级别];   |
| :----: | :-----------------------------: |
| 默认值 | error_log logs/error.log error; |
|  位置  | 全局块、http、server、location  |



该属性可以通过`./configure --error-log-path=PATH`来指定



其中日志级别的值有：debug|info|notice|warn|error|crit|alert|emerg



使用示例：

```sh
worker_processes  1;
# 错误日志
error_log ./error_log.log warn;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### include

**用来引入其他配置文件，使Nginx的配置更加灵活**



|  语法  | include file; |
| :----: | :-----------: |
| 默认值 |      无       |
|  位置  |      any      |







## events块

### accept_mutex

**用来设置Nginx网络连接序列化**



|  语法  | accept_mutex on\|off; |
| :----: | :-------------------: |
| 默认值 |   accept_mutex on;    |
|  位置  |        events         |



这个配置主要可以用来解决常说的**"惊群"问题**。大致意思是在某一个时刻，客户端发来一个请求连接，Nginx后台是以多进程的工作模式，也就是说有多个worker进程会被同时唤醒，但是最终只会有一个进程可以获取到连接，如果每次唤醒的进程数目太多，就会影响Nginx的整体性能。如果将上述值设置为on(开启状态)，将会对多个Nginx进程接收连接进行序列号，一个个来唤醒接收，就**防止了多个进程对连接的争抢**



使用示例：

```sh
worker_processes  1;

events {
    worker_connections  1024;
    # 网络连接序列化
    accept_mutex on;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### multi_accept

**用来设置是否允许同时接收多个网络连接**

如果multi_accept被禁止了，nginx一个工作进程只能同时接受一个新的连接。否则，一个工作进程可以同时接受所有的新连接



|  语法  | multi_accept on\|off; |
| :----: | :-------------------: |
| 默认值 |   multi_accept off;   |
|  位置  |        events         |



使用示例：

```sh
worker_processes  1;

events {
    worker_connections  1024;
    # 允许同时接收多个网络连接
    multi_accept on;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```







### worker_connections

**用来配置单个worker进程最大的连接数**

这里的连接数不仅仅包括和前端用户建立的连接数，而是包括所有可能的连接数。另外，**number值不能大于操作系统支持打开的最大文件句柄数量**。



|  语法  | worker_connections number; |
| :----: | :------------------------: |
| 默认值 |  worker_commections 512;   |
|  位置  |           events           |



使用示例：

```sh
worker_processes  1;

events {
    # 单个worker进程最大的连接数
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```





### use

**用来设置Nginx服务器选择哪种事件驱动来处理网络消息**

method的可选值有select/poll/epoll/kqueue等

epoll需要linux内核在2.6以上



|  语法  |  use  method;  |
| :----: | :------------: |
| 默认值 | 根据操作系统定 |
|  位置  |     events     |



使用示例：

```sh
worker_processes  1;

events {
    # 单个worker进程最大的连接数
    worker_connections  1024;
    use epoll;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
......
    }

}
```







## http块

### MIME-Type

浏览器中可以显示的内容有HTML、XML、GIF等种类繁多的文件、媒体等资源，浏览器为了区分这些资源，就需要使用MIME Type。所以说MIME Type是网络资源的媒体类型。Nginx作为web服务器，也需要能够识别前端请求的资源类型



在Nginx的配置文件中，默认有两行配置：

```sh
include mime.types;
default_type application/octet-stream;
```



### default_type

**用来配置Nginx响应前端请求默认的MIME类型**



|  语法  |  default_type mime-type;  |
| :----: | :-----------------------: |
| 默认值 | default_type text/plain； |
|  位置  |  http、server、location   |



在default_type之前还有一句`include mime.types`，相当于把mime.types文件中MIMT类型与相关类型文件的文件后缀名的对应关系加入到当前的配置文件中。



有些时候请求某些接口的时候需要返回指定的文本字符串或者json字符串，如果逻辑非常简单或者干脆是固定的字符串，那么可以使用nginx快速实现，这样就不用编写程序响应请求了，可以减少服务器资源占用并且响应性能非常快。



```sh
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;
        

        location /text {
           default_type text/html;
           return 200 "test";
         }
    
        location /json{
           default_type application/json;
           return 200 '{"id":10001,"name":"张三"}';
         }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}
```







### 自定义服务日志

Nginx中日志的类型分access.log、error.log。

access.log:用来记录用户所有的访问请求。

error.log:记录nginx本身运行时的错误信息，不会记录用户的访问请求。

Nginx服务器支持对服务日志的格式、大小、输出等进行设置，需要使用到两个指令，**分别是access_log和log_format指令**。



#### access_log

**用来设置用户访问日志的相关属性**



|  语法  | access_log path[format[buffer=size]] |
| :----: | :----------------------------------: |
| 默认值 | access_log logs/access.log combined; |
|  位置  |     `http`, `server`, `location`     |



#### log_format

**用来指定日志的输出格式**



|  语法  | log_format name [escape=default\|json\|none] string....; |
| :----: | :------------------------------------------------------: |
| 默认值 |                log_format combined "...";                |
|  位置  |                           http                           |





### sendfile

**用来设置Nginx服务器是否使用sendfile()传输文件**，该属性可以大大提高Nginx处理静态资源的性能



|  语法  |   sendfile on\|off；   |
| :----: | :--------------------: |
| 默认值 |     sendfile off;      |
|  位置  | http、server、location |



### keepalive_timeout

**用来设置长连接的超时时间**



|  语法  | keepalive_timeout time; |
| :----: | :---------------------: |
| 默认值 | keepalive_timeout 75s;  |
|  位置  | http、server、location  |





### keepalive_requests

**用来设置一个keep-alive连接使用的次数**



|  语法  | keepalive_requests number; |
| :----: | :------------------------: |
| 默认值 |  keepalive_requests 100;   |
|  位置  |   http、server、location   |









## server块和location块

```sh
server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
       
        error_page   500 502 503 504 404  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```





### 需求

有如下访问：

* http://127.0.0.1:8081/server1/location1：访问的是：index_sr1_location1.html
* http://127.0.0.1:8081/server1/location2：访问的是：index_sr1_location2.html
* http://127.0.0.1:8082/server2/location1：访问的是：index_sr2_location1.html
* http://127.0.0.1:8082/server2/location2：访问的是：index_sr2_location2.html



如果访问的资源不存在，返回自定义的404页面

将/server1和/server2的配置使用不同的配置文件分割

使用include进行合并配置文件

为/server1和/server2各自创建一个访问日志文件





### 实现

以windows系统为例



创建日志目录：

```sh
PS D:\opensoft\nginx-1.24.0> ls


    目录: D:\opensoft\nginx-1.24.0


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/4/29     14:43                conf
d-----         2023/4/11     23:31                contrib
d-----         2023/4/11     23:31                docs
d-----         2023/4/11     23:31                html
d-----          2023/5/1     13:48                logs
d-----         2023/4/29     13:29                temp
------         2023/4/11     23:29        3811328 nginx.exe


PS D:\opensoft\nginx-1.24.0> cd logs
PS D:\opensoft\nginx-1.24.0\logs> ls


    目录: D:\opensoft\nginx-1.24.0\logs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          2023/5/1     23:32          20220 access.log
-a----          2023/5/1     23:32          15005 error.log
-a----          2023/5/1     13:52              7 nginx.pid


PS D:\opensoft\nginx-1.24.0\logs> mkdir server1


    目录: D:\opensoft\nginx-1.24.0\logs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     13:55                server1


PS D:\opensoft\nginx-1.24.0\logs> mkdir server2


    目录: D:\opensoft\nginx-1.24.0\logs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     13:55                server2


PS D:\opensoft\nginx-1.24.0\logs> ls


    目录: D:\opensoft\nginx-1.24.0\logs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     13:55                server1
d-----          2023/5/3     13:55                server2
-a----          2023/5/1     23:32          20220 access.log
-a----          2023/5/1     23:32          15005 error.log
-a----          2023/5/1     13:52              7 nginx.pid


PS D:\opensoft\nginx-1.24.0\logs>
```





创建配置文件目录

```sh
PS D:\opensoft\nginx-1.24.0> cd conf
PS D:\opensoft\nginx-1.24.0\conf> ls


    目录: D:\opensoft\nginx-1.24.0\conf


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/4/29     14:43                conf-backup
------         2023/4/11     23:31           1103 fastcgi.conf
------         2023/4/11     23:31           1032 fastcgi_params
------         2023/4/11     23:31           2946 koi-utf
------         2023/4/11     23:31           2326 koi-win
------         2023/4/11     23:31           5448 mime.types
-a----          2023/5/1     13:52            556 nginx.conf
------         2023/4/11     23:31            653 scgi_params
------         2023/4/11     23:31            681 uwsgi_params
------         2023/4/11     23:31           3736 win-utf


PS D:\opensoft\nginx-1.24.0\conf> mkdir server


    目录: D:\opensoft\nginx-1.24.0\conf


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     13:56                server


PS D:\opensoft\nginx-1.24.0\conf> ls


    目录: D:\opensoft\nginx-1.24.0\conf


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/4/29     14:43                conf-backup
d-----          2023/5/3     13:56                server
------         2023/4/11     23:31           1103 fastcgi.conf
------         2023/4/11     23:31           1032 fastcgi_params
------         2023/4/11     23:31           2946 koi-utf
------         2023/4/11     23:31           2326 koi-win
------         2023/4/11     23:31           5448 mime.types
-a----          2023/5/1     13:52            556 nginx.conf
------         2023/4/11     23:31            653 scgi_params
------         2023/4/11     23:31            681 uwsgi_params
------         2023/4/11     23:31           3736 win-utf


PS D:\opensoft\nginx-1.24.0\conf>
```



创建静态资源目录

```sh
PS D:\opensoft\nginx-1.24.0> cd html
PS D:\opensoft\nginx-1.24.0\html> ls


    目录: D:\opensoft\nginx-1.24.0\html


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
------         2023/4/11      9:45            497 50x.html
------         2023/4/11      9:45            615 index.html


PS D:\opensoft\nginx-1.24.0\html> mkdir server1


    目录: D:\opensoft\nginx-1.24.0\html


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     14:04                server1


PS D:\opensoft\nginx-1.24.0\html> mkdir server2


    目录: D:\opensoft\nginx-1.24.0\html


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     14:04                server2


PS D:\opensoft\nginx-1.24.0\html> ls


    目录: D:\opensoft\nginx-1.24.0\html


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/5/3     14:04                server1
d-----          2023/5/3     14:04                server2
------         2023/4/11      9:45            497 50x.html
------         2023/4/11      9:45            615 index.html


PS D:\opensoft\nginx-1.24.0\html>
```





nginx.conf配置文件内容：

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
      #配置请求处理日志格式
      log_format server1 '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
      log_format server2 '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
	#配置允许使用sendfile方式运输
	sendfile on;
	#配置连接超时时间
	keepalive_timeout 65;
	# 导入配置
	include ./conf/server/*.conf;
}
```





server1.conf

位于conf/server目录下

```sh
server{
		#配置监听端口和主机名称
		listen 8081;
		server_name localhost;
		#配置请求处理日志存放路径
		access_log ./logs/server1/access.log server1;
		#配置错误页面
		error_page 404 /404.html;
		#配置处理/server1/location1请求的location
		location /server1/location1{
			root html;
			index index_sr1_location1.html;
		}
		#配置处理/server1/location2请求的location
		location /server1/location2{
			root html;
			index index_sr1_location2.html;
		}
		#配置错误页面转向
		location = /404.html {
			root html;
			index 404.html;
		}
}
```



创建html文件index_sr1_location1.html

位于html/server1/location1目录下



```html
<!DOCTYPE html>
<html>
<head>
<title>server1</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>index_sr1_location1</h1>
</body>
</html>
```



创建html文件index_sr1_location2.html

位于html/server1/location2目录下

```html
<!DOCTYPE html>
<html>
<head>
<title>server1</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>index_sr1_location2</h1>
</body>
</html>
```



server2.conf

位于conf/server目录下

```sh
server{
		#配置监听端口和主机名称
		listen 8082;
		server_name localhost;
		#配置请求处理日志存放路径
		access_log ./logs/server2/access.log server2;
		#配置错误页面,对404.html做了定向配置
		error_page 404 /404.html;
		#配置处理/server1/location1请求的location
		location /server2/location1{
			root html;
			index index_sr2_location1.html;
		}
		#配置处理/server2/location2请求的location
		location /server2/location2{
			root html;
			index index_sr2_location2.html;
		}
		#配置错误页面转向
		location = /404.html {
			root html;
			index 404.html;
		}
	}
```





创建html文件index_sr2_location1.html

位于html/server2/location1目录下

```html
<!DOCTYPE html>
<html>
<head>
<title>server2</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>index_sr2_location1</h1>
</body>
</html>
```



创建html文件index_sr2_location2.html

位于html/server2/location2目录下

```html
<!DOCTYPE html>
<html>
<head>
<title>server2</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>index_sr2_location2</h1>
</body>
</html>
```



创建404.html文件

位于html目录

```html
<!DOCTYPE html>
<html>
<head>
<title>Error</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>404</h1>

</body>
</html>

```





检查配置是否正确

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx  -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0>
```



```sh
PS D:\opensoft\nginx-1.24.0> tree /f
文件夹 PATH 列表
卷序列号为 5E84-66B0
D:.
│  nginx.exe
│
├─conf
│  │  fastcgi.conf
│  │  fastcgi_params
│  │  koi-utf
│  │  koi-win
│  │  mime.types
│  │  nginx.conf
│  │  scgi_params
│  │  uwsgi_params
│  │  win-utf
│  │
│  │
│  └─server
│          server1.conf
│          server2.conf
│
├─html
│  │  404.html
│  │  50x.html
│  │  index.html
│  │
│  ├─server1
│  │  ├─location1
│  │  │      index_sr1_location1.html
│  │  │
│  │  └─location2
│  │          index_sr1_location2.html
│  │
│  └─server2
│      ├─location1
│      │      index_sr2_location1.html
│      │
│      └─location2
│              index_sr2_location2.html
│
├─logs
│  ├─server1
│  └─server2
PS D:\opensoft\nginx-1.24.0>
```





### 测试

http://127.0.0.1:8081/server1/location1：访问的是：index_sr1_location1.html

![image-20230503160729094](img/Nginx学习笔记/image-20230503160729094.png)



http://127.0.0.1:8081/server1/location2：访问的是：index_sr1_location2.html

![image-20230503160741149](img/Nginx学习笔记/image-20230503160741149.png)





http://127.0.0.1:8082/server2/location1：访问的是：index_sr2_location1.html

![image-20230503160750934](img/Nginx学习笔记/image-20230503160750934.png)





http://127.0.0.1:8082/server2/location2：访问的是：index_sr2_location2.html

![image-20230503160759803](img/Nginx学习笔记/image-20230503160759803.png)





```sh
PS D:\opensoft\nginx-1.24.0> cd logs
PS D:\opensoft\nginx-1.24.0\logs> cat .\error.log
PS D:\opensoft\nginx-1.24.0\logs> cd .\server1\
PS D:\opensoft\nginx-1.24.0\logs\server1> ls


    目录: D:\opensoft\nginx-1.24.0\logs\server1


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          2023/5/3     16:07            450 access.log


PS D:\opensoft\nginx-1.24.0\logs\server1> cat .\access.log
127.0.0.1 - - [03/May/2023:16:07:21 +0800] "GET /server1/location1/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.68" "-"
127.0.0.1 - - [03/May/2023:16:07:37 +0800] "GET /server1/location2/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.68" "-"
PS D:\opensoft\nginx-1.24.0\logs\server1> cd ..
PS D:\opensoft\nginx-1.24.0\logs> cd .\server2\
PS D:\opensoft\nginx-1.24.0\logs\server2> ls


    目录: D:\opensoft\nginx-1.24.0\logs\server2


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          2023/5/3     16:07            450 access.log


PS D:\opensoft\nginx-1.24.0\logs\server2> cat .\access.log
127.0.0.1 - - [03/May/2023:16:07:47 +0800] "GET /server2/location1/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.68" "-"
127.0.0.1 - - [03/May/2023:16:07:56 +0800] "GET /server2/location2/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.68" "-"
PS D:\opensoft\nginx-1.24.0\logs\server2>
```













# Nginx静态资源部署

## 静态资源概述

请 求的内容就分为两种类型，一类是静态资源、一类是动态资源。

**静态资源即指在服务器端真实存在并且能直接拿来展示的一些文件**，比如常见的html页面、css文件、js文件、图 片、视频等资源；

**动态资源即指在服务器端真实存在但是要想获取需要经过一定的业务逻辑处理，根据不同的条件展示在页面不同这 一部分内容**，比如说报表数据展示、根据当前登录用户展示相关具体数据等资源；





## 静态资源的配置指令

### listen指令

**用来配置监听端口**



|  语法  | listen address[:port] [default_server]...;<br/>listen port [default_server]...; |
| :----: | :----------------------------------------------------------: |
| 默认值 |                    listen *:80 \| *:8000                     |
|  位置  |                            server                            |



* listen 127.0.0.1:8000; ：listen localhost:8000 监听指定的IP和端口
* listen 127.0.0.1; ：监听指定IP的所有端口
* listen 8000;  ：监听指定端口上的连接
* listen *:8000; ：监听指定端口上的连接



default_server属性是标识符，用来将此虚拟主机设置成默认主机。所谓的默认主机指的是如果没有匹配到对应的address:port，则会默认执行的。如果不指定默认使用的是第一个server。



使用示例：

```sh
server{
	listen 8080;
	server_name 127.0.0.1;
	location /{
		root html;
		index index.html;
	}
}
server{
	listen 8080 default_server;
	server_name localhost;
	default_type text/plain;
	return 444 'This is a error request';
}
```







### server_name指令

**用来设置虚拟主机服务名称**



|  语法  | server_name  name ...;<br/>name可以提供多个中间用空格分隔 |
| :----: | :-------------------------------------------------------: |
| 默认值 |                     server_name  "";                      |
|  位置  |                          server                           |



关于server_name的配置方式有三种，分别是：

* 精确匹配
* 通配符匹配
* 正则表达式匹配



#### 精确匹配

```sh
server {
	listen 80;
	server_name www.abc.com www.abc.cn www.abcd.com;
	...
}
```



#### 通配符匹配

server_name中支持通配符"*",但需要注意的是**通配符不能出现在域名的中间**，只能出现在首段或尾段

```sh
server {
	listen 80;
	server_name *.abc.com www.abcd.*;
	...
}
```



下面的配置就会报错

```sh
server {
	listen 80;
	server_name www.*.com;
	...
}
```

```sh
server {
	listen 80;
	server_name www.abc.c*;
	...
}
```





#### 正则表达式匹配

server_name中可以使用正则表达式，并且使用`~`作为正则表达式字符串的开始标记

| 代码  |                            说明                            |
| :---: | :--------------------------------------------------------: |
|   ^   |                   匹配搜索字符串开始位置                   |
|   $   |                   匹配搜索字符串结束位置                   |
|   .   |              匹配除换行符\n之外的任何单个字符              |
|   \   |            转义字符，将下一个字符标记为特殊字符            |
| [xyz] |               字符集，与任意一个指定字符匹配               |
| [a-z] |             字符范围，匹配指定范围内的任何字符             |
|  \w   | 与以下任意字符匹配 A-Z a-z 0-9 和下划线,等效于[A-Za-z0-9_] |
|  \d   |                 数字字符匹配，等效于[0-9]                  |
|  {n}  |                        正好匹配n次                         |
| {n,}  |                        至少匹配n次                         |
| {n,m} |                     匹配至少n次至多m次                     |
|   *   |                   零次或多次，等效于{0,}                   |
|   +   |                   一次或多次，等效于{1,}                   |
|   ?   |                  零次或一次，等效于{0,1}                   |



使用示例：

```sh
server {
	listen 80;
	server_name ~^www\.(\w+)\.com$;
	...
}
```





#### 匹配执行顺序

由于server_name指令支持通配符和正则表达式，因此在包含多个虚拟主机的配置文件中，可能会出现一个名称被多个虚拟主机的server_name匹配成功

执行顺序如下：

1. 准确匹配server_name
2. 通配符在开始时匹配server_name成功
3. 通配符在结束时匹配server_name成功
4. 正则表达式匹配server_name成功
5. 被默认的default_server处理，如果没有指定默认找第一个server







### location指令

用来设置请求的URI

uri变量是待匹配的请求字符串，可以不包含正则表达式，也可以包含正则表达式，那么nginx服务器在搜索匹配location的时候，是先使用不包含正则表达式进行匹配，找到一个匹配度最高的一个，然后在通过包含正则表达式的进行匹配，如果能匹配到直接访问，匹配不到，就使用刚才匹配度最高的那个location来处理请求。



|  语法  | location [  =  \|   ~  \|  ~*   \|   ^~   \|@ ] uri{...} |
| :----: | :------------------------------------------------------: |
| 默认值 |                            —                             |
|  位置  |                     server,location                      |





不带符号，要求必须以指定模式开始

```sh
server {
	listen 80;
	server_name 127.0.0.1;
	location /abc{
		default_type text/plain;
		return 200 "access success";
	}
}
```



以下访问都是正确的：

* http://127.0.0.1/abc
* http://127.0.0.1/abc?key=value
* http://127.0.0.1/abc/
* http://127.0.0.1/abcdef



= :  用于不包含正则表达式的uri前，必须与指定的模式精确匹配

```sh
server {
	listen 80;
	server_name 127.0.0.1;
	location =/abc{
		default_type text/plain;
		return 200 "access success";
	}
}
```



以下访问都是正确的：

* http://127.0.0.1/abc
* http://127.0.0.1/abc?key=value



以下访问都是错误的：

* http://127.0.0.1/abc/
* http://127.0.0.1/abcdef



~ ： 用于表示当前uri中包含了正则表达式，并且区分大小写

```sh
server {
	listen 80;
	server_name 127.0.0.1;
	location ~^/abc\w${
		default_type text/plain;
		return 200 "access success";
	}
}
```





~*:  用于表示当前uri中包含了正则表达式，并且不区分大小写

```sh
server {
	listen 80;
	server_name 127.0.0.1;
	location ~*^/abc\w${
		default_type text/plain;
		return 200 "access success";
	}
}
```



^~: 用于不包含正则表达式的uri前，功能和不加符号的一致，唯一不同的是，如果模式匹配，那么就停止搜索其他模式了。

```sh
server {
	listen 80;
	server_name 127.0.0.1;
	location ^~/abc{
		default_type text/plain;
		return 200 "access success";
	}
}
```





### root / alias

#### root指令

设置请求的根目录



|  语法  |       root path;       |
| :----: | :--------------------: |
| 默认值 |       root html;       |
|  位置  | http、server、location |



path为Nginx服务器接收到请求以后查找资源的根目录路径



#### alias指令

用来更改location的URI



|  语法  | alias path; |
| :----: | :---------: |
| 默认值 |      —      |
|  位置  |  location   |



path为修改后的根路径。





#### 区别

假设浏览器要访问/server1/abc/hello.html

页面文件在html/server1/abc/hello.html下

root的路径为：

```sh
location /images {
	root html;
}
```



如果把root改为alias，则路径变成：

```sh
location /images {
	alias html/server1/abc;
}
```





#### 总结

* root的处理结果是: root路径+location路径
* alias的处理结果是:使用alias路径替换location路径
* alias是一个目录别名的定义，root则是最上层目录的含义
* 如果location路径是以/结尾,则alias也必须是以/结尾，root没有要求







### index指令

用于设置网站的默认首页



|  语法  |    index file ...;     |
| :----: | :--------------------: |
| 默认值 |   index index.html;    |
|  位置  | http、server、location |



index后面可以跟多个设置，如果访问的时候没有指定具体访问的资源，则会依次进行查找，找到第一个为止



示例：

```sh
location / {
	root html;
	index index.html index.htm;
}
```



访问该location的时候，可以通过 http://ip:port/，地址后面如果不添加任何内容，则默认依次访问index.html和index.htm，找到第一个来进行返回





### error_page指令

用于设置网站的错误页面



|  语法  | error_page code ... [=[response]] uri; |
| :----: | :------------------------------------: |
| 默认值 |                   —                    |
|  位置  |      http、server、location......      |



可以指定具体跳转的地址

```sh
server {
	error_page 404 https://www.bilibili.com/;
}
```



可以指定重定向地址

```sh
server{
	error_page 404 /50x.html;
	error_page 500 502 503 504 /50x.html;
	location =/50x.html{
		root html;
	}
}
```



使用location的@符合完成错误信息展示

```sh
server{
	error_page 404 @jump_to_error;
	location @jump_to_error {
		default_type text/plain;
		return 404 'Not Found Page...';
	}
}
```



可选项`=[response]`的作用是用来将相应代码更改为另外一个

```sh
server{
	error_page 404 =200 /50x.html;
	location =/50x.html{
		root html;
	}
}
```



当返回404找不到对应的资源的时候，在浏览器上可以看到，最终返回的状态码是200

编写error_page后面的内容，404后面需要加空格，200前面不能加空格









## 静态资源优化配置

### sendﬁle

用来开启高效的文件传输模式。零拷贝。



|  语法  |     sendﬁle on \|oﬀ;      |
| :----: | :-----------------------: |
| 默认值 |        sendﬁle oﬀ;        |
|  位置  | http、server、location... |





### tcp_nopush

该指令必须在sendfile打开的状态下才会生效，主要是用来提升网络包的**传输效率**

|  语法  |  tcp_nopush on\|off;   |
| :----: | :--------------------: |
| 默认值 |     tcp_nopush oﬀ;     |
|  位置  | http、server、location |





### tcp_nodelay

该指令必须在keep-alive连接开启的情况下才生效，来提高网络包传输的**实时性**



|  语法  |  tcp_nodelay on\|off;  |
| :----: | :--------------------: |
| 默认值 |    tcp_nodelay on;     |
|  位置  | http、server、location |



sendfile可以开启高效的文件传输模式，tcp_nopush开启可以确保在发送到客户端之前数据包已经充分“填满”， 这大大减少了网络开销，并加快了文件发送的速度。 然后，当它到达最后一个可能因为没有“填满”而暂停的数据包时，Nginx会忽略tcp_nopush参数， 然后，tcp_nodelay强制套接字发送数据。由此可知，TCP_NOPUSH可以与TCP_NODELAY一起设置，它比单独配置TCP_NODELAY具有更强的性能。







## Nginx静态资源压缩

### Gzip模块配置指令

以下指令都来自ngx_http_gzip_module模块，该模块会在nginx安装的时候内置到nginx的安装环境中



#### gzip指令

**该指令用于开启或者关闭gzip功能**



|  语法  |       gzip on\|off;       |
| :----: | :-----------------------: |
| 默认值 |         gzip off;         |
|  位置  | http、server、location... |



使用示例：

```sh
http{
   gzip on;
}
```





#### gzip_types指令

该指令可以根据响应页的MIME类型选择性地开启Gzip压缩功能



|  语法  | gzip_types mime-type ...; |
| :----: | :-----------------------: |
| 默认值 |   gzip_types text/html;   |
|  位置  |  http、server、location   |



所选择的值可以从mime.types文件中进行查找，也可以使用"*"代表所有。



使用示例：对js文件压缩：

```sh
http{
	gzip_types application/javascript;
}
```





#### gzip_comp_level指令

该指令用于设置Gzip压缩程度，级别从1-9,1表示要是程度最低，要是效率最高，9刚好相反，压缩程度最高，但是效率最低



|  语法  | gzip_comp_level level; |
| :----: | :--------------------: |
| 默认值 |   gzip_comp_level 1;   |
|  位置  | http、server、location |



使用示例：

```sh
http{
	gzip_comp_level 5;
}
```





#### gzip_vary指令

该指令用于设置使用Gzip进行压缩发送是否携带“Vary:Accept-Encoding”头域的响应头部。主要是告诉接收方，所发送的数据经过了Gzip压缩处理



|  语法  |   gzip_vary on\|off;   |
| :----: | :--------------------: |
| 默认值 |     gzip_vary off;     |
|  位置  | http、server、location |



使用示例：

```sh
http{
	gzip_vary on;
}
```







#### gzip_buffers指令

该指令用于处理请求压缩的缓冲区数量和大小

其中number:指定Nginx服务器向系统申请缓存空间个数，size指的是每个缓存空间的大小。主要实现的是申请number个每个大小为size的内存空间。这个值的设定一般会和服务器的操作系统有关，所以建议此项不设置，使用默认值即可



|  语法  | gzip_buffers number size;  |
| :----: | :------------------------: |
| 默认值 | gzip_buffers 32 4k\|16 8k; |
|  位置  |   http、server、location   |



使用示例：

```sh
http{
	gzip_buffers 16 1024K;
}
```





#### gzip_disable指令

针对不同种类客户端发起的请求，可以选择性地开启和关闭Gzip功能

regex:根据客户端的浏览器标志(user-agent)来设置，支持使用正则表达式。指定的浏览器标志不使用Gzip.该指令一般是用来排除一些明显不支持Gzip的浏览器



|  语法  | gzip_disable regex ...; |
| :----: | :---------------------: |
| 默认值 |            —            |
|  位置  | http、server、location  |



使用示例：

```sh
http{
	gzip_disable "MSIE [1-6]\.";
}
```





#### gzip_http_version指令

针对不同的HTTP协议版本，可以选择性地开启和关闭Gzip功能



|  语法  | gzip_http_version 1.0\|1.1; |
| :----: | :-------------------------: |
| 默认值 |   gzip_http_version 1.1;    |
|  位置  |   http、server、location    |



使用示例：

```sh
http{
	gzip_http_version 1.1;
}
```







#### gzip_min_length指令

该指令针对传输数据的大小，可以选择性地开启和关闭Gzip功能

Gzip压缩功能对大数据的压缩效果明显，但是如果要压缩的数据比较小的化，可能出现越压缩数据量越大的情况，因此我们需要根据响应内容的大小来决定是否使用Gzip功能，响应页面的大小可以通过头信息中的`Content-Length`来获取。但是如何使用了Chunk编码动态压缩，该指令将被忽略。建议设置为1K或以上



|  语法  | gzip_min_length length; |
| :----: | :---------------------: |
| 默认值 |   gzip_min_length 20;   |
|  位置  | http、server、location  |



使用示例：

```sh
http{
	gzip_min_length 1024;
}
```

```sh
http{
	gzip_min_length 5K;
}
```

```sh
http{
	gzip_min_length 1M;
}
```







#### gzip_proxied指令

该指令设置是否对服务端返回的结果进行Gzip压缩



|  语法  | gzip_proxied  off\|expired\|no-cache\|<br/>no-store\|private\|no_last_modified\|no_etag\|auth\|any; |
| :----: | :----------------------------------------------------------: |
| 默认值 |                      gzip_proxied off;                       |
|  位置  |                    http、server、location                    |



使用示例：

```sh
http{
	gzip_proxied on;
}
```







### Gzip和sendfile共存问题

开启sendfile以后，在读取磁盘上的静态资源文件的时候，可以减少拷贝的次数，可以不经过用户进程将静态文件通过网络设备发送出去，但是Gzip要想对资源压缩，是需要经过用户进程进行操作的。所以两个指令不能共存。

可以使用ngx_http_gzip_static_module模块的gzip_static指令来解决。





#### 添加模块到Nginx

使用Linux nginx添加模块



(1)查询当前Nginx的配置参数

```
nginx -V
```



(2)将nginx安装目录下sbin目录中的nginx二进制文件进行更名

```
cd /usr/local/nginx/sbin
mv nginx nginxold
```



(3) 进入Nginx的安装目录

```
cd /root/nginx/core/nginx-1.16.1
```



(4)执行make clean清空之前编译的内容

```
make clean
```



(5)使用configure来配置参数

```
./configure --with-http_gzip_static_module
```



(6)使用make命令进行编译

```
make
```



(7) 将objs目录下的nginx二进制执行文件移动到nginx安装目录下的sbin目录中

```
mv objs/nginx /usr/local/nginx/sbin
```



(8)执行更新命令

```
make upgrade
```





#### gzip_static指令

 检查与访问资源同名的.gz文件时，response中以gzip相关的header返回.gz文件的内容



|  语法  | **gzip_static** on \| off \| always; |
| :----: | :----------------------------------: |
| 默认值 |           gzip_static off;           |
|  位置  |        http、server、location        |



使用示例：

```sh
http{
	gzip_static on;
}
```













## Nginx静态资源缓存

### 缓存的作用

* 成本最低的一种缓存实现
* 减少网络带宽消耗
* 降低服务器压力
* 减少网络延迟，加快页面打开速度




### 浏览器缓存的执行流程

HTTP协议中和页面缓存相关的字段：

|    header     |                    说明                     |
| :-----------: | :-----------------------------------------: |
|    Expires    |            缓存过期的日期和时间             |
| Cache-Control |          设置和缓存相关的配置信息           |
| Last-Modified |            请求资源最后修改时间             |
|     ETag      | 请求变量的实体标签的当前值，比如文件的MD5值 |





![image-20230506143327780](img/Nginx学习笔记/image-20230506143327780.png)





1. 用户首次通过浏览器发送请求到服务端获取数据，客户端是没有对应的缓存，所以需要发送request请求来获取数据；
2. 服务端接收到请求后，获取服务端的数据及服务端缓存的允许后，返回200的成功状态码并且在响应头上附上对应资源以及缓存信息；
3. 当用户再次访问相同资源的时候，客户端会在浏览器的缓存目录中查找是否存在响应的缓存文件
4. 如果没有找到对应的缓存文件，则走(2)步
5. 如果有缓存文件，接下来对缓存文件是否过期进行判断，过期的判断标准是(Expires),
6. 如果没有过期，则直接从本地缓存中返回数据进行展示
7. 如果Expires过期，接下来需要判断缓存文件是否发生过变化
8. 判断的标准有两个，一个是ETag(Entity Tag),一个是Last-Modified
9. 判断结果是未发生变化，则服务端返回304，直接从缓存文件中获取数据
10. 如果判断是发生了变化，重新从服务端获取数据，并根据缓存协商(服务端所设置的是否需要进行缓存数据的设置)来进行数据缓存。







### 浏览器缓存相关指令

#### expires指令

该指令用来控制页面缓存的作用。可以通过该指令控制HTTP响应中的“Expires"和”Cache-Control"



|  语法  | expires   [modified] time<br/>expires epoch\|max\|off; |
| :----: | :----------------------------------------------------: |
| 默认值 |                      expires off;                      |
|  位置  |                 http、server、location                 |



* time：可以整数也可以是负数，指定过期时间，如果是负数，Cache-Control则为no-cache,如果为整数或0，则Cache-Control的值为max-age=time
* epoch：指定Expires的值为'1 January,1970,00:00:01 GMT'(1970-01-01 00:00:00)，Cache-Control的值no-cache
* max：指定Expires的值为'31 December2037 23:59:59GMT' (2037-12-31 23:59:59) ，Cache-Control的值为10年
* off：默认不缓存





#### add_header指令

add_header指令是用来添加指定的响应头和响应值



|  语法  | add_header name value [always]; |
| :----: | :-----------------------------: |
| 默认值 |                —                |
|  位置  |    http、server、location...    |



Cache-Control作为响应头信息，可以设置如下值：

缓存响应指令：

```
Cache-control: must-revalidate
Cache-control: no-cache
Cache-control: no-store
Cache-control: no-transform
Cache-control: public
Cache-control: private
Cache-control: proxy-revalidate
Cache-Control: max-age=<seconds>
Cache-control: s-maxage=<seconds>
```



|       指令       |                      说明                      |
| :--------------: | :--------------------------------------------: |
| must-revalidate  |        可缓存但必须再向源服务器进行确认        |
|     no-cache     |             缓存前必须确认其有效性             |
|     no-store     |           不缓存请求或响应的任何内容           |
|   no-transform   |              代理不可更改媒体类型              |
|      public      |            可向任意方提供响应的缓存            |
|     private      |              仅向特定用户返回响应              |
| proxy-revalidate | 要求中间缓存服务器对缓存的响应有效性再进行确认 |
|   max-age=<秒>   |                 响应最大Age值                  |
|  s-maxage=<秒>   |         公共缓存服务器响应的最大Age值          |

max-age=[秒]：









## Nginx的跨域问题

### 概述

同源策略：

浏览器的同源策略：是一种约定，是浏览器最核心也是最基本的安全功能，如果浏览器少了同源策略，则浏览器的正常功能可能都会受到影响。

同源:  协议、域名(IP)、端口相同即为同源



跨域指有两台服务器分别为A,B,如果从服务器A的页面发送异步请求到服务器B获取数据，如果服务器A和服务器B不满足同源策略，则就会出现跨域问题。





### 解决

使用add_header指令，该指令可以用来添加一些头信息

此处用来解决跨域问题，需要添加两个头信息，一个是`Access-Control-Allow-Origin`,`Access-Control-Allow-Methods`

* Access-Control-Allow-Origin: 直译过来是允许跨域访问的源地址信息，可以配置多个(多个用逗号分隔)，也可以使用`*`代表所有源
* Access-Control-Allow-Methods:直译过来是允许跨域访问的请求方式，值可以为 GET POST PUT DELETE...,可以全部设置，也可以根据需要设置，多个用逗号分隔



使用示例：

```sh
location / {
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE;
    root html;
    index index.html;
}
```









## 静态资源防盗链

### 概述

资源盗链指的是此内容不在自己服务器上，而是通过技术手段，绕过别人的限制将别人的内容放到自己页面上最终展示给用户。以此来盗取大网站的空间和流量。简而言之就是用别人的东西成就自己的网站。



### 防盗链的实现原理

发起HTTP请求时会携带HTTP的头信息Referer，当浏览器向web服务器发送请求的时候，一般都会带上Referer，告诉浏览器该网页是从哪个页面链接过来的。

后台服务器可以根据获取到的这个Referer信息来判断是否为自己信任的网站地址，如果是则放行继续访问，如果不是则可以返回403(服务端拒绝访问)的状态信息。

![image-20230508141352404](img/Nginx学习笔记/image-20230508141352404.png)





#### valid_referers指令

nginx会通就过查看referer自动和valid_referers后面的内容进行匹配，如果匹配到了就将$invalid_referer变量置0，如果没有匹配到，则将\$invalid_referer变量置为1，匹配的过程中不区分大小写。



|  语法  | valid_referers none\|blocked\|server_names\|string... |
| :----: | :---------------------------------------------------: |
| 默认值 |                           —                           |
|  位置  |                   server、location                    |



* none：如果Header中的Referer为空，允许访问
* blocked：在Header中的Referer不为空，但是该值被防火墙或代理进行伪装过，如不带"http://" 、"https://"等协议头的资源允许访问。
* server_names：指定具体的域名或者IP
* string：可以支持正则表达式和*的字符串。如果是正则表达式，需要以`~`开头表示



```sh
location ~*\.(png|jpg|gif){
           valid_referers none blocked www.baidu.com;
           if ($invalid_referer){
                return 403;
           }
           root html;

}
```





#### 针对目录进行防盗链

```sh
location /images {
           valid_referers none blocked www.baidu.com;
           if ($invalid_referer){
                return 403;
           }
           root html;

}
```





### 存在的问题

Referer的限制比较粗，比如随意加一个Referer，上面的方式是无法进行限制的。

可以使用Nginx的第三方模块`ngx_http_accesskey_module`来解决问题















## Rewrite

### 概述

Rewrite是Nginx服务器提供的一个重要基本功能，是Web服务器产品中几乎必备的功能。主要的作用是用来实现URL的重写。

Nginx服务器的Rewrite功能的实现依赖于PCRE的支持，因此在编译安装Nginx服务器之前，需要安装PCRE库。Nginx使用的是ngx_http_rewrite_module模块来解析和处理Rewrite功能的相关配置。



### 地址重写与地址转发

* 地址重写浏览器地址会发生变化而地址转发则不变
* 一次地址重写会产生两次请求而一次地址转发只会产生一次请求
* 地址重写到的页面必须是一个完整的路径而地址转发则不需要
* 地址重写因为是两次请求所以request范围内属性不能传递给新页面而地址转发因为是一次请求所以可以传递值
* 地址转发速度快于地址重写





### Rewrite常用全局变量

|        变量        |                             说明                             |
| :----------------: | :----------------------------------------------------------: |
|       $args        | 变量中存放了请求URL中的请求指令。比如http://192.168.200.133:8080?arg1=value1&args2=value2中的"arg1=value1&arg2=value2"，功能和$query_string一样 |
|  $http_user_agent  | 变量存储的是用户访问服务的代理信息(如果通过浏览器访问，记录的是浏览器的相关版本信息) |
|       $host        |            变量存储的是访问服务器的server_name值             |
|   $document_uri    | 变量存储的是当前访问地址的URI。比如http://192.168.200.133/server?id=10&name=zhangsan中的"/server"，功能和$uri一样 |
|   $document_root   | 变量存储的是当前请求对应location的root值，如果未设置，默认指向Nginx自带html目录所在位置 |
|  $content_length   |           变量存储的是请求头中的Content-Length的值           |
|   $content_type    |            变量存储的是请求头中的Content-Type的值            |
|    $http_cookie    | 变量存储的是客户端的cookie信息，可以通过add_header Set-Cookie 'cookieName=cookieValue'来添加cookie数据 |
|    $limit_rate     | 变量中存储的是Nginx服务器对网络连接速率的限制，也就是Nginx配置中对limit_rate指令设置的值，默认是0，不限制。 |
|    $remote_addr    |                 变量中存储的是客户端的IP地址                 |
|    $remote_port    |          变量中存储了客户端与服务端建立连接的端口号          |
|    $remote_user    |      变量中存储了客户端的用户名，需要有认证模块才能获取      |
|      $scheme       |                     变量中存储了访问协议                     |
|    $server_addr    |                   变量中存储了服务端的地址                   |
|    $server_name    |           变量中存储了客户端请求到达的服务器的名称           |
|    $server_port    |           变量中存储了客户端请求到达服务器的端口号           |
|  $server_protocol  |       变量中存储了客户端请求协议的版本，比如"HTTP/1.1"       |
| $request_body_file |        变量中存储了发给后端服务器的本地文件资源的名称        |
|  $request_method   |       变量中存储了客户端的请求方式，比如"GET","POST"等       |
| $request_filename  |            变量中存储了当前请求的资源文件的路径名            |
|    $request_uri    | 变量中存储了当前请求的URI，并且携带请求参数，比如http://192.168.200.133/server?id=10&name=zhangsan中的"/server?id=10&name=zhangsan" |





### Rewrite规则

#### set指令

该指令用来设置一个新的变量。



|  语法  | set $variable value; |
| :----: | :------------------: |
| 默认值 |          —           |
|  位置  | server、location、if |



* variable：变量的名称，该变量名称要用"$"作为变量的第一个字符，且不能与Nginx服务器预设的全局变量同名。
* value：变量的值，可以是字符串、其他变量或者变量的组合等。



示例：

```sh
location /images {
           set $disabled 1
           if ($disabled){
                return 404;
           }
           root html;

}
```





#### if指令

该指令用来支持条件判断，并根据条件判断结果选择不同的Nginx配置



|  语法  | if  (condition){...} |
| :----: | :------------------: |
| 默认值 |          —           |
|  位置  |   server、location   |



condition为判定条件，可以支持以下写法：

* 变量名。如果变量名对应的值为空或者是0，if都判断为false,其他条件为true

```sh
if ($param){
	
}
```



* 使用"="和"!="比较变量和字符串是否相等，满足条件为true，不满足为false

```sh
if ($request_method = POST){
	return 405;
}
```



* 使用正则表达式对变量进行匹配，匹配成功返回true，否则返回false。变量与正则表达式之间使用"~","~*","!~","!~\*"来连接
  * "~"代表匹配正则表达式过程中区分大小写
  * "~\*"代表匹配正则表达式过程中不区分大小写
  * "!~"和"!~\*"刚好和上面取相反值，如果匹配上返回false,匹配不上返回true

```sh
if ($http_user_agent ~ MSIE){
	#$http_user_agent的值中是否包含MSIE字符串，如果包含返回true
}
```

正则表达式字符串一般不需要加引号，但是如果字符串中包含"}"或者是";"等字符时，就需要把引号加上。



* 判断请求的文件是否存在使用"-f"和"!-f"
  * 当使用"-f"时，如果请求的文件存在返回true，不存在返回false
  * 当使用"!f"时，如果请求文件不存在，但该文件所在目录存在返回true,文件和目录都不存在返回false,如果文件存在返回false

```sh
if (-f $request_filename){
	#判断请求的文件是否存在
}
```

```sh
if (!-f $request_filename){
	#判断请求的文件是否不存在
}
```



* 判断请求的目录是否存在使用"-d"和"!-d"
  * 当使用"-d"时，如果请求的目录存在，如果目录存在返回true，如果目录不存在则返回false
  * 当使用"!-d"时，如果请求的目录不存在但该目录的上级目录存在则返回true，该目录和它上级目录都不存在则返回false,如果请求目录存在也返回false

```sh
set $request_dir html/a/b/c
if (-d $request_dir){
	#判断请求的目录是否存在
}
```

```sh
set $request_dir html/a/b/c
if (!-d $request_dir){
	#判断请求的目录是否不存在
}
```



* 判断请求的目录或者文件是否存在使用"-e"和"!-e"
  * 当使用"-e",如果请求的目录或者文件存在时，如果目录或者文件存在返回true,否则返回false.
  * 当使用"!-e",如果请求的文件和文件所在路径上的目录都不存在返回true,否则返回false

```sh
if (-e $request_filename){
	#判断请求的文件或者目录是否存在
}
```



* 判断请求的文件是否可执行使用"-x"和"!-x"
  * 当使用"-x",如果请求的文件可执行，if返回true,否则返回false
  * 当使用"!-x",如果请求文件不可执行，返回true,否则返回false

```sh
if (-x $request_filename){
	#判断请求的文件是否可执行
}
```





#### break指令

该指令用于中断当前相同作用域中的其他Nginx配置。与该指令处于同一作用域的Nginx配置中，位于它前面的指令配置生效，位于后面的指令配置无效。



|  语法  |        break;        |
| :----: | :------------------: |
| 默认值 |          —           |
|  位置  | server、location、if |



示例：

```sh
location / {
	if ($param){
		set $id $1;
		break;
		limit_rate 10k;
	}
}
```





#### return指令

该指令用于完成对请求的处理，直接向客户端返回响应状态代码。在return后的所有Nginx配置都是无效的。



|  语法  | return code [text];<br/>return code URL;<br/>return URL; |
| :----: | :------------------------------------------------------: |
| 默认值 |                            —                             |
|  位置  |                   server、location、if                   |



* code：为返回给客户端的HTTP状态代理。可以返回的状态代码为0~999的任意HTTP状态代理
* text：为返回给客户端的响应体内容，支持变量的使用
* URL：为返回给客户端的URL地址



```sh
location /test {
	return 405;
}
```

```sh
location /test {
	return 200 "<h1>test</h1>";
}
```

```sh
location /test {
	return 200 https://www.bilibili.com/;
}
```





#### rewrite指令

该指令通过正则表达式的使用来改变URI。可以同时存在一个或者多个指令，按照顺序依次对URL进行匹配和处理。



|  语法  | rewrite regex replacement [flag]; |
| :----: | :-------------------------------: |
| 默认值 |                 —                 |
|  位置  |       server、location、if        |



* regex：用来匹配URI的正则表达式
* replacement：匹配成功后，用于替换URI中被截取内容的字符串。如果该字符串是以"http://"或者"https://"开头的，则不会继续向下对URI进行其他处理，而是直接返回重写后的URI给客户端
* flag：用来设置rewrite对URI的处理行为，可选值有如下：
  * last:
  * break
  * redirect
  * permanent





#### rewrite_log指令

该指令配置是否开启URL重写日志的输出功能



|  语法  |    rewrite_log on\|off;    |
| :----: | :------------------------: |
| 默认值 |      rewrite_log off;      |
|  位置  | http、server、location、if |

开启后，URL重写的相关日志将以notice级别输出到error_log指令配置的日志文件汇总。



















# Nginx反向代理

## 概述

反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源。同时，用户不需要知道目标服务器的地址，也无须在用户端作任何设定。反向代理服务器通常可用来作为Web加速，即使用反向代理作为Web服务器的前置机来降低网络和服务器的负载，提高访问效率

简而言之就是正向代理代理的对象是客户端，反向代理代理的是服务端

Nginx即可以实现正向代理，也可以实现反向代理。







## 反向代理的配置语法

Nginx反向代理模块的指令是由`ngx_http_proxy_module`模块进行解析，该模块在安装Nginx的时候已经自己加装到Nginx中了





### proxy_pass

该指令用来设置被代理服务器地址，可以是主机名称、IP地址加端口号形式。



|  语法  | proxy_pass URL; |
| :----: | :-------------: |
| 默认值 |        —        |
|  位置  |    location     |



* URL：要设置的被代理服务器地址。包含传输协议(`http`,`https://`)、主机名称或IP地址加端口号、URI等要素。



使用示例：

```sh
location /{
  proxy_pass http://192.168.200.146
}
```





### proxy_set_header

该指令可以更改Nginx服务器接收到的客户端请求的请求头信息，然后将新的请求头发送给代理的服务器



|  语法  |                proxy_set_header field value;                 |
| :----: | :----------------------------------------------------------: |
| 默认值 | proxy_set_header Host $proxy_host;<br/>proxy_set_header Connection close; |
|  位置  |                    http、server、location                    |



使用示例：

```sh
        location /server {
                proxy_pass http://192.168.200.146:8080/;
                proxy_set_header key value;
        }
```







### proxy_redirect

该指令是用来重置头信息中的"Location"和"Refresh"的值



|  语法  | proxy_redirect redirect replacement;<br/>proxy_redirect default;<br/>proxy_redirect off; |
| :----: | :----------------------------------------------------------: |
| 默认值 |                   proxy_redirect default;                    |
|  位置  |                    http、server、location                    |



* redirect：目标,Location的值
* replacement：要替换的值
* default：将location块的uri变量作为replacement，将proxy_pass变量作为redirect进行替换
* off：关闭功能



服务端[192.168.200.146]

```sh
server {
    listen  8081;
    server_name localhost;
    if (!-f $request_filename){
    	return 302 http://192.168.200.146;
    }
}
```



代理服务端[192.168.200.133]

```sh
server {
	listen  8081;
	server_name localhost;
	location / {
		proxy_pass http://192.168.200.146:8081/;
		proxy_redirect http://192.168.200.146 http://192.168.200.133;
	}
}
```



如果不使用proxy_redirect指令，服务端重定向时，会把服务端的真实ip地址发送到客户端（浏览器）











## 反向代理实现

### 需求

有三台服务器，服务器的内容不一样，要求访问/server1地址，访问的是服务器1的内容，访问/server2地址，访问的是服务器2的内容，访问/server3地址，访问的是服务器3的内容

![image-20230510133746575](img/Nginx学习笔记/image-20230510133746575.png)







### 实现

创建一个spring boot程序

* server1：端口为9091
* server2：端口为9092
* server3：端口为9093



程序的Controller如下：

```java
package mao.nginx_reverse_proxy_demo.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import java.util.UUID;
import java.util.concurrent.atomic.AtomicLong;

/**
 * Project name(项目名称)：nginx-reverse-proxy-demo
 * Package(包名): mao.nginx_reverse_proxy_demo.controller
 * Class(类名): TestController
 * Author(作者）: mao
 * Author QQ：1296193245
 * GitHub：https://github.com/maomao124/
 * Date(创建日期)： 2023/5/10
 * Time(创建时间)： 13:44
 * Version(版本): 1.0
 * Description(描述)： 测试nginx反向代理
 */


@RestController
public class TestController
{
    /**
     * 日志
     */
    private static final Logger log = LoggerFactory.getLogger(TestController.class);

    private static final String UUID;

    private static final AtomicLong ATOMIC_LONG = new AtomicLong(0);

    static
    {
        UUID = java.util.UUID.randomUUID().toString();
        log.info("当前实例id为：" + UUID);
    }

    @GetMapping("/")
    public String ping(HttpServletRequest httpServletRequest)
    {
        long count = ATOMIC_LONG.incrementAndGet();
        String remoteAddr = httpServletRequest.getRemoteAddr();
        log.info(remoteAddr + "访问当前实例,访问计数：" + count);
        return "当前机器id：" + UUID + "<br>" + "当前访问ip：" + remoteAddr + "<br>访问计数：" + count;
    }
}
```



server2的启动命令：

```sh
java -jar nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar --server.port=9092
```



server3的启动命令：

```sh
java -jar nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar --server.port=9093
```



访问测试

![image-20230510140510427](img/Nginx学习笔记/image-20230510140510427.png)

![image-20230510140524142](img/Nginx学习笔记/image-20230510140524142.png)



![image-20230510140535353](img/Nginx学习笔记/image-20230510140535353.png)





配置代理服务器nginx.conf

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location /server1 {
                proxy_pass http://127.0.0.1:9091/;
        }
        location /server2 {
                proxy_pass http://127.0.0.1:9092/;
        }
        location /server3 {
                proxy_pass http://127.0.0.1:9093/;
        }
}
}
```



检查配置是否有错误：

```sh
PS D:\opensoft\nginx-1.24.0> nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0>
```



启动nginx，访问8080端口



http://localhost:8080/server1

![image-20230510141038648](img/Nginx学习笔记/image-20230510141038648.png)



http://localhost:8080/server2

![image-20230510141051832](img/Nginx学习笔记/image-20230510141051832.png)

http://localhost:8080/server3

![image-20230510141114090](img/Nginx学习笔记/image-20230510141114090.png)











## 反向代理系统调优

### proxy_buffering指令

该指令用来开启或者关闭代理服务器的缓冲区



|  语法  | proxy_buffering on\|off; |
| :----: | :----------------------: |
| 默认值 |   proxy_buffering on;    |
|  位置  |  http、server、location  |





### proxy_buffers指令

该指令用来指定单个连接从代理服务器读取响应的缓存区的个数和大小



|  语法  |        proxy_buffers number size;         |
| :----: | :---------------------------------------: |
| 默认值 | proxy_buffers 8 4k \| 8K;(与系统平台有关) |
|  位置  |          http、server、location           |



* number：缓冲区的个数
* size：每个缓冲区的大小，缓冲区的总大小就是number*size





### proxy_buffer_size指令

该指令用来设置从被代理服务器获取的第一部分响应数据的大小



|  语法  |           proxy_buffer_size size;           |
| :----: | :-----------------------------------------: |
| 默认值 | proxy_buffer_size 4k \| 8k;(与系统平台有关) |
|  位置  |           http、server、location            |





### proxy_busy_buffers_size指令

该指令用来限制同时处于BUSY状态的缓冲总大小



|  语法  |  proxy_busy_buffers_size size;   |
| :----: | :------------------------------: |
| 默认值 | proxy_busy_buffers_size 8k\|16K; |
|  位置  |      http、server、location      |





### proxy_temp_path指令

当缓冲区存满后，仍未被Nginx服务器完全接受，响应数据就会被临时存放在磁盘文件上，该指令设置文件路径



|  语法  |   proxy_temp_path  path;    |
| :----: | :-------------------------: |
| 默认值 | proxy_temp_path proxy_temp; |
|  位置  |   http、server、location    |





### proxy_temp_file_write_size指令

该指令用来设置磁盘上缓冲文件的大小



|  语法  |  proxy_temp_file_write_size size;   |
| :----: | :---------------------------------: |
| 默认值 | proxy_temp_file_write_size 8K\|16K; |
|  位置  |       http、server、location        |















# Nginx安全控制

## 安全隔离

通过代理分开了客户端到web程序服务器端的连接，实现了安全措施。在反向代理之前设置防火墙，仅留一个入口供代理服务器访问



![image-20230511141503410](img/Nginx学习笔记/image-20230511141503410.png)





## 使用SSL对流量进行加密

### 概述

就是使用https请求，而不是使用http请求。这两个之间的区别简单的来说两个都是HTTP协议，只不过https是身披SSL外壳的http

HTTPS是一种通过计算机网络进行安全通信的传输协议。它经由HTTP进行通信，利用SSL/TLS建立全通信，加密数据包，确保数据的安全性。

SSL(Secure Sockets Layer)安全套接层

TLS(Transport Layer Security)传输层安全

上述这两个是为网络通信提供安全及数据完整性的一种安全协议，TLS和SSL在传输层和应用层对网络连接进行加密。



为什么要使用HTTPS？

http协议是明文传输数据，存在安全问题，而https是加密传输，相当于http+ssl，并且可以防止流量劫持。

Nginx要想使用SSL，需要满足一个条件即需要添加一个模块`--with-http_ssl_module`,而该模块在编译的过程中又需要OpenSSL的支持





### nginx添加SSL的支持

1. 将原有/usr/local/nginx/sbin/nginx进行备份
2. 拷贝nginx之前的配置信息
3. 在nginx的安装源码进行配置指定对应模块  ./configure --with-http_ssl_module
4. 通过make模板进行编译
5. 将objs下面的nginx移动到/usr/local/nginx/sbin下
6. 在源码目录下执行  make upgrade进行升级，这个可以实现不停机添加新模块的功能







### 相关指令

#### ssl指令

该指令用来在指定的服务器开启HTTPS,可以使用 listen 443 ssl,后面这种方式更通用些



|  语法  | ssl on \| off; |
| :----: | :------------: |
| 默认值 |    ssl off;    |
|  位置  |  http、server  |



使用示例：

```sh
server{
	listen 443 ssl;
}
```





#### ssl_certificate指令

为当前这个虚拟主机指定一个带有PEM格式证书的证书



|  语法  | ssl_certificate file; |
| :----: | :-------------------: |
| 默认值 |           —           |
|  位置  |     http、server      |







#### ssl_certificate_key指令

该指令用来指定PEM secret key文件的路径



|  语法  | ssl_ceritificate_key file; |
| :----: | :------------------------: |
| 默认值 |             —              |
|  位置  |        http、server        |





#### ssl_session_cache指令

该指令用来配置用于SSL会话的缓存



|  语法  | ssl_sesion_cache off\|none\|[builtin[:size]] [shared:name:size] |
| :----: | :----------------------------------------------------------: |
| 默认值 |                   ssl_session_cache none;                    |
|  位置  |                         http、server                         |



* off：禁用会话缓存，客户端不得重复使用会话
* none：禁止使用会话缓存，客户端可以重复使用，但是并没有在缓存中存储会话参数
* builtin：内置OpenSSL缓存，仅在一个工作进程中使用
* shared：所有工作进程之间共享缓存，缓存的相关信息用name和size来指定





#### ssl_session_timeout指令

开启SSL会话功能后，设置客户端能够反复使用储存在缓存中的会话参数时间



|  语法  | ssl_session_timeout time; |
| :----: | :-----------------------: |
| 默认值 |  ssl_session_timeout 5m;  |
|  位置  |       http、server        |





#### ssl_ciphers指令

指出允许的密码，密码指定为OpenSSL支持的格式



|  语法  |     ssl_ciphers ciphers;      |
| :----: | :---------------------------: |
| 默认值 | ssl_ciphers HIGH:!aNULL:!MD5; |
|  位置  |         http、server          |





#### ssl_prefer_server_ciphers指令

该指令指定是否服务器密码优先客户端密码



|  语法  | ssl_perfer_server_ciphers on\|off; |
| :----: | :--------------------------------: |
| 默认值 |   ssl_perfer_server_ciphers off;   |
|  位置  |            http、server            |





### 生成证书

生成证书的方式主要有两种：

* 使用阿里云/腾讯云等第三方服务进行购买
* 使用openssl生成证书





#### 阿里云购买

登录阿里云，点击左上方

https://home.console.aliyun.com/home/dashboard/ProductAndService

![image-20230512135036508](img/Nginx学习笔记/image-20230512135036508.png)



搜索ssl

![image-20230512140322740](img/Nginx学习笔记/image-20230512140322740.png)



点击SSL证书

购买需要域名

![image-20230512140538910](img/Nginx学习笔记/image-20230512140538910.png)





#### openssl生成证书

先要确认当前系统是否有安装openssl

```sh
PS C:\Users\mao\Desktop> openssl version
OpenSSL 3.1.0 14 Mar 2023 (Library: OpenSSL 3.1.0 14 Mar 2023)
PS C:\Users\mao\Desktop>
```



命令：

```sh
openssl genrsa -des3 -out server.key 1024
openssl req -new -key server.key -out server.csr
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```



```sh
PS C:\Users\mao\Desktop> openssl genrsa -des3 -out server.key 1024
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
PS C:\Users\mao\Desktop> openssl req -new -key server.key -out server.csr
Enter pass phrase for server.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:cn
State or Province Name (full name) [Some-State]:test
Locality Name (eg, city) []:test
Organization Name (eg, company) [Internet Widgits Pty Ltd]:test
Organizational Unit Name (eg, section) []:test
Common Name (e.g. server FQDN or YOUR name) []:test
Email Address []:test

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:test
An optional company name []:test
PS C:\Users\mao\Desktop> cp server.key server.key.org
PS C:\Users\mao\Desktop> openssl rsa -in server.key.org -out server.key
Enter pass phrase for server.key.org:
writing RSA key
PS C:\Users\mao\Desktop> openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
Certificate request self-signature ok
subject=C = cn, ST = test, L = test, O = test, OU = test, CN = test, emailAddress = test
PS C:\Users\mao\Desktop>
```





![image-20230512141556774](img/Nginx学习笔记/image-20230512141556774.png)







### 示例

```sh
server {
    listen       443 ssl;
    server_name  localhost;

    ssl_certificate      server.cert;
    ssl_certificate_key  server.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   html;
        index  index.html index.htm;
    }
}
```

















## Nginx的用户认证

### 概述

对应系统资源的访问，我们往往需要限制谁能访问，谁不能访问。这块就是我们通常所说的认证部分，认证需要做的就是根据用户输入的用户名和密码来判定用户是否为合法用户，如果是则放行访问，如果不是则拒绝访问。

Nginx对应用户认证这块是通过ngx_http_auth_basic_module模块来实现的，它允许通过使用"HTTP基本身份验证"协议验证用户名和密码来限制对资源的访问。默认情况下nginx是已经安装了该模块，如果不需要则使用--without-http_auth_basic_module







### 相关指令

#### auth_basic指令

使用“ HTTP基本认证”协议启用用户名和密码的验证

开启后，服务端会返回401，指定的字符串会返回到客户端，给用户以提示信息，但是不同的浏览器对内容的展示不一致



|  语法  |      auth_basic string\|off;      |
| :----: | :-------------------------------: |
| 默认值 |          auth_basic off;          |
|  位置  | http,server,location,limit_except |





#### auth_basic_user_file指令

指定用户名和密码所在文件

指定文件路径，该文件中的用户名和密码的设置，密码需要进行加密。可以采用工具自动生成



|  语法  |    auth_basic_user_file file;     |
| :----: | :-------------------------------: |
| 默认值 |                 —                 |
|  位置  | http,server,location,limit_except |





### 实现

密码文件的格式如下：

```sh
name1:password1
name2:password2
name3:password3
......
```



生成文件

可以用Apache HTTP Server发行包中的htpasswd命令或者openssl passwd来创建此类文件



```sh
PS C:\Users\mao\Desktop> openssl passwd 12345
$1$1vr9J5qv$W3uesrt13hR16STkCbKir/
PS C:\Users\mao\Desktop> openssl passwd 12345
$1$b79DyDEL$.Ir3OZixewfBznpmwzyx50
PS C:\Users\mao\Desktop>
```



```sh
echo "admin:$1$b79DyDEL$.Ir3OZixewfBznpmwzyx50" > password
```



将文件放入的nginx的conf目录里

```sh
PS D:\opensoft\nginx-1.24.0\conf> ls


    目录: D:\opensoft\nginx-1.24.0\conf


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/4/29     14:43                conf-backup
d-----          2023/5/3     14:16                server
------         2023/4/11     23:31           1103 fastcgi.conf
------         2023/4/11     23:31           1032 fastcgi_params
------         2023/4/11     23:31           2946 koi-utf
------         2023/4/11     23:31           2326 koi-win
------         2023/4/11     23:31           5448 mime.types
-a----         2023/5/12     14:55            990 nginx.conf
-a----         2023/5/12     14:37             64 password
------         2023/4/11     23:31            653 scgi_params
------         2023/4/11     23:31            681 uwsgi_params
------         2023/4/11     23:31           3736 win-utf


PS D:\opensoft\nginx-1.24.0\conf>
```





配置nginx.conf

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location / {
                root html;
                index index.html;
                auth_basic 'need auth';
                auth_basic_user_file password;
        }
}
}
```



校验和启动

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0>
```



访问

随便输入一个uri

http://localhost:8080/test

![image-20230512145050206](img/Nginx学习笔记/image-20230512145050206.png)



![image-20230512145748173](img/Nginx学习笔记/image-20230512145748173.png)

















# Nginx负载均衡

## 负载均衡概述

早期的网站流量和业务功能都比较简单，单台服务器足以满足基本的需求，但是随着互联网的发展，业务流量越来越大并且业务逻辑也跟着越来越复杂，单台服务器的性能及单点故障问题就凸显出来了，因此需要多台服务器进行性能的水平扩展及避免单点故障出现。



## 负载均衡的原理

系统的扩展可以分为纵向扩展和横向扩展。

纵向扩展是从单机的角度出发，通过增加系统的硬件处理能力来提升服务器的处理能力

横向扩展是通过添加机器来满足大型网站服务的处理能力。



负载均衡：将用户访问的请求根据对应的负载均衡算法，分发到集群中的一台服务器进行处理。





## 负载均衡的作用

* 解决服务器的高并发压力，提高应用程序的处理性能
* 提供故障转移，实现高可用
* 通过添加或减少服务器数量，增强网站的可扩展性
* 在负载均衡器上进行过滤，可以提高系统的安全性





## 常用负载均衡方式

主要有三种：

* 用户手动选择
* DNS轮询方式
* 四/七层负载均衡



### 用户手动选择

这种方式比较原始，只要实现的方式就是在网站主页上面提供不同线路、不同服务器链接方式，让用户来选择自己访问的具体服务器，来实现负载均衡。

![image-20230513124808611](img/Nginx学习笔记/image-20230513124808611.png)





### DNS轮询方式

大多域名注册商都支持对同一个主机名添加多条A记录，这就是DNS轮询，DNS服务器将解析请求按照A记录的顺序，随机分配到不同的IP上，这样就能完成简单的负载均衡。DNS轮询的成本非常低，在一些不重要的服务器，被经常使用。



![image-20230513124853910](img/Nginx学习笔记/image-20230513124853910.png)



虽然DNS轮询成本低廉，但是DNS负载均衡存在明显的缺点：

* **可靠性低**：假设一个域名DNS轮询多台服务器，如果其中的一台服务器发生故障，那么所有的访问该服务器的请求将不会有所回应，即使你将该服务器的IP从DNS中去掉，但是由于各大宽带接入商将众多的DNS存放在缓存中，以节省访问时间，导致DNS不会实时更新。所以DNS轮流上一定程度上解决了负载均衡问题，但是却存在可靠性不高的缺点
* **负载均衡不均衡**：DNS负载均衡采用的是简单的轮询负载算法，不能区分服务器的差异，不能反映服务器的当前运行状态，不能做到为性能好的服务器多分配请求，另外本地计算机也会缓存已经解析的域名到IP地址的映射，这也会导致使用该DNS服务器的用户在一定时间内访问的是同一台Web服务器，从而引发Web服务器减的负载不均衡







### 四/七层负载均衡

四层负载均衡指的是OSI七层模型中的传输层，主要是基于IP+PORT的负载均衡

实现负载均衡的有基于硬件的F5 BIG-IP、Radware等，基于软件的LVS、Nginx、Hayproxy等



七层负载均衡指的是在应用层，主要是基于虚拟的URL或主机IP的负载均衡

实现负载均衡的有Nginx、Hayproxy等软件



区别：

* 四层负载均衡数据包是在底层就进行了分发，而七层负载均衡数据包则在最顶端进行分发，所以四层负载均衡的效率比七层负载均衡的要高
* 四层负载均衡不识别域名，而七层负载均衡识别域名













## Nginx七层负载均衡

Nginx要实现七层负载均衡需要用到proxy_pass代理模块配置

Nginx的负载均衡是在Nginx的反向代理基础上把用户的请求根据指定的算法分发到一组**upstream虚拟服务池**



### 相关指令

#### upstream指令

该指令是用来定义一组服务器，它们可以是监听不同端口的服务器，并且也可以是同时监听TCP和Unix socket的服务器。服务器可以指定不同的权重，默认为1



|  语法  | upstream name {...} |
| :----: | :-----------------: |
| 默认值 |          —          |
|  位置  |        http         |





#### server指令

该指令用来指定后端服务器的名称和一些参数，可以使用域名、IP、端口或者unix socket



|  语法  | server name [paramerters] |
| :----: | :-----------------------: |
| 默认值 |             —             |
|  位置  |         upstream          |



使用示例：

```sh
upstream testserver{
	server 127.0.0.1:9091;
	server 127.0.0.1:9092;
	server 127.0.0.1:9093;
}
```







### 实现

启动反向代理章节生成的jar包

* server1：端口为9091
* server2：端口为9092
* server3：端口为9093







server1的启动命令：

```sh
java -jar nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar --server.port=9091
```



server2的启动命令：

```sh
java -jar nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar --server.port=9092
```



server3的启动命令：

```sh
java -jar nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar --server.port=9093
```



![image-20230513130749175](img/Nginx学习笔记/image-20230513130749175.png)



![image-20230513130847122](img/Nginx学习笔记/image-20230513130847122.png)



![image-20230513130859760](img/Nginx学习笔记/image-20230513130859760.png)



![image-20230513130911421](img/Nginx学习笔记/image-20230513130911421.png)





配置nginx.conf

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```





检查配置并启动

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0>
```



访问

![image-20230513131645698](img/Nginx学习笔记/image-20230513131645698.png)

![image-20230513131654112](img/Nginx学习笔记/image-20230513131654112.png)

![image-20230513131702156](img/Nginx学习笔记/image-20230513131702156.png)

![image-20230513131710964](img/Nginx学习笔记/image-20230513131710964.png)



![image-20230513131719646](img/Nginx学习笔记/image-20230513131719646.png)



默认采用轮询的方式



```sh
PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：f3e1f11b-2873-4167-bf04-ed93a75cb2a4<br>当前访问ip：127.0.0.1<br>访问计数：7
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:17:58 GMT
                    Server: nginx/1.24.0

                    当前机器id：f3e1f11b-2873-4167-bf04-ed93a75c...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:17:5                     8 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：4709e1eb-03aa-4b24-8f23-83b794d521d9<br>当前访问ip：127.0.0.1<br>访问计数：8
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:22 GMT
                    Server: nginx/1.24.0

                    当前机器id：4709e1eb-03aa-4b24-8f23-83b794d5...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:2                     2 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：088d7871-6657-4f4a-be89-636206d5a4a0<br>当前访问ip：127.0.0.1<br>访问计数：9
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:24 GMT
                    Server: nginx/1.24.0

                    当前机器id：088d7871-6657-4f4a-be89-636206d5...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:2                     4 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：f3e1f11b-2873-4167-bf04-ed93a75cb2a4<br>当前访问ip：127.0.0.1<br>访问计数：8
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:26 GMT
                    Server: nginx/1.24.0

                    当前机器id：f3e1f11b-2873-4167-bf04-ed93a75c...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:2                     6 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：4709e1eb-03aa-4b24-8f23-83b794d521d9<br>当前访问ip：127.0.0.1<br>访问计数：9
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:28 GMT
                    Server: nginx/1.24.0

                    当前机器id：4709e1eb-03aa-4b24-8f23-83b794d5...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:2                     8 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：088d7871-6657-4f4a-be89-636206d5a4a0<br>当前访问ip：127.0.0.1<br>访问计数：10
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:28 GMT
                    Server: nginx/1.24.0

                    当前机器id：088d7871-6657-4f4a-be89-636206d5...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:2                     8 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：f3e1f11b-2873-4167-bf04-ed93a75cb2a4<br>当前访问ip：127.0.0.1<br>访问计数：9
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:29 GMT
                    Server: nginx/1.24.0

                    当前机器id：f3e1f11b-2873-4167-bf04-ed93a75c...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:2
                    9 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：4709e1eb-03aa-4b24-8f23-83b794d521d9<br>当前访问ip：127.0.0.1<br>访问计数：10
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sat, 13 May 2023 05:18:31 GMT
                    Server: nginx/1.24.0

                    当前机器id：4709e1eb-03aa-4b24-8f23-83b794d5...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Sat, 13 May 2023 05:18:3
                    1 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS D:\opensoft\nginx-1.24.0>
```









### 负载均衡状态

代理服务器在负责均衡调度中的状态有以下几个：



|     状态     |               概述                |
| :----------: | :-------------------------------: |
|     down     |  当前的server暂时不参与负载均衡   |
|    backup    |         预留的备份服务器          |
|  max_fails   |        允许请求失败的次数         |
| fail_timeout | 经过max_fails失败后, 服务暂停时间 |
|  max_conns   |       限制最大的接收连接数        |





#### down

将该服务器标记为永久不可用，那么该代理服务器将不参与负载均衡

该状态一般会对需要停机维护的服务器进行设置





```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091 down;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```





```sh
PS C:\Users\mao> curl http://localhost:8080/                                                                                                    

StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：1
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:15:17 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:15:1                     7 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：1
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:15:47 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:15:4                     7 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:15:52 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:15:5                     2 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:15:53 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:15:5
                    3 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:15:55 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:15:5
                    5 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao>
```



![image-20230514131743095](img/Nginx学习笔记/image-20230514131743095.png)



![image-20230514131753876](img/Nginx学习笔记/image-20230514131753876.png)



![image-20230514131802791](img/Nginx学习笔记/image-20230514131802791.png)



修改配置文件：

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091;
        server 127.0.0.1:9092 down;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```



重新加载

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> ./nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```



访问

```sh
PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f903f4d<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:19:48 GMT
                    Server: nginx/1.24.0

                    当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f90...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:19:4                     8 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：5
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:19:54 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:19:5                     4 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f903f4d<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:19:55 GMT
                    Server: nginx/1.24.0

                    当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f90...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:19:5
                    5 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：6
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:19:56 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:19:5
                    6 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao>
```







#### backup

将该服务器标记为备份服务器，当主服务器不可用时，此节点开始工作



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093 backup;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```



```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> ./nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```



访问

```sh
PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f903f4d<br>当前访问ip：127.0.0.1<br>访问计数：4
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:24:17 GMT
                    Server: nginx/1.24.0

                    当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f90...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:24:1                     7 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：5
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:24:21 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:24:2                     1 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f903f4d<br>当前访问ip：127.0.0.1<br>访问计数：5
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:24:23 GMT
                    Server: nginx/1.24.0

                    当前机器id：1bc0fb77-e7c2-459b-9d2b-c6d66f90...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:24:2
                    3 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：6
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:24:24 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:24:2
                    4 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao>
```





这时候关闭9091端口的服务1bc0fb77-e7c2-459b-9d2b-c6d66f903f4d

再次访问

```sh
PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：7
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:26:10 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:26:1                     0 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：8
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:26:15 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:26:1                     5 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：9
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:26:16 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:26:1                     6 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：10
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:26:16 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:26:1
                    6 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：2c0a3988-546a-4104-97b8-59b4553489f2<br>当前访问ip：127.0.0.1<br>访问计数：11
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:26:17 GMT
                    Server: nginx/1.24.0

                    当前机器id：2c0a3988-546a-4104-97b8-59b45534...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:26:1
                    7 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS C:\Users\mao>
```



这时候只有一个节点工作，关闭9092节点2c0a3988-546a-4104-97b8-59b4553489f2

再次访问

```sh
PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：7
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:27:55 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:27:5                     5 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：8
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:27:57 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:27:5                     7 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：9
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:27:58 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:27:5
                    8 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS C:\Users\mao> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：30f9b492-dbea-4e67-ad93-b2b2466274d4<br>当前访问ip：127.0.0.1<br>访问计数：10
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Sun, 14 May 2023 05:27:59 GMT
                    Server: nginx/1.24.0

                    当前机器id：30f9b492-dbea-4e67-ad93-b2b24662...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Sun, 14 May 2023 05:27:5
                    9 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS C:\Users\mao>
```



备份节点再次工作







#### max_conns

max_conns=number是用来设置代理服务器同时活动链接的最大数量，默认为0，表示不限制，使用该配置可以根据后端服务器处理请求的并发量来进行设置，防止后端服务器被压垮



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091 down;
        server 127.0.0.1:9092 down;
        server 127.0.0.1:9093 max_conns=1;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```



```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> ./nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```



使用jmeter测试

![image-20230514133336142](img/Nginx学习笔记/image-20230514133336142.png)



![image-20230514133401509](img/Nginx学习笔记/image-20230514133401509.png)



![image-20230514133411975](img/Nginx学习笔记/image-20230514133411975.png)



![image-20230514133437295](img/Nginx学习笔记/image-20230514133437295.png)



![image-20230514133451743](img/Nginx学习笔记/image-20230514133451743.png)



![image-20230514133604121](img/Nginx学习笔记/image-20230514133604121.png)



启动测试

![image-20230514133723109](img/Nginx学习笔记/image-20230514133723109.png)



吞吐量18000左右，异常率98%，只有1.5%左右的请求是正常进入到后端服务器的。

![image-20230514133857035](img/Nginx学习笔记/image-20230514133857035.png)



![image-20230514133910575](img/Nginx学习笔记/image-20230514133910575.png)





nginx日志如下：

![image-20230514134028648](img/Nginx学习笔记/image-20230514134028648.png)



```sh
2023/05/14 13:36:25 [error] 8060#10672: *2992 no live upstreams while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.1", upstream: "http://testserver/", host: "localhost:8080"
2023/05/14 13:36:25 [error] 8060#10672: *3027 no live upstreams while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.1", upstream: "http://testserver/", host: "localhost:8080"
2023/05/14 13:36:25 [error] 8060#10672: *3040 no live upstreams while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.1", upstream: "http://testserver/", host: "localhost:8080"
2023/05/14 13:36:25 [error] 8060#10672: *3078 no live upstreams while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.1", upstream: "http://testserver/", host: "localhost:8080"
2023/05/14 13:36:25 [error] 8060#10672: *3155 no live upstreams while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.1", upstream: "http://testserver/", host: "localhost:8080"
2023/05/14 13:36:25 [error] 8060#10672: *3128 no live upstreams while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.1", upstream: "http://testserver/", host: "localhost:8080"
```







再将max_conns更改到30

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091 down;
        server 127.0.0.1:9092 down;
        server 127.0.0.1:9093 max_conns=30;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```



```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> ./nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```



再次测试

![image-20230514134814659](img/Nginx学习笔记/image-20230514134814659.png)



将线程数更改成60

![image-20230514135040620](img/Nginx学习笔记/image-20230514135040620.png)



测试结果异常率为61%，期望值为50%左右

![image-20230514135053140](img/Nginx学习笔记/image-20230514135053140.png)









#### max_fails

max_fails=number设置允许请求代理服务器失败的次数，默认为1



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091 down;
        server 127.0.0.1:9092 down;
        server 127.0.0.1:9093 max_conns=30 max_fails=3;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```





#### fail_timeout

fail_timeout=time设置经过max_fails失败后，服务暂停的时间，默认是10秒



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091 down;
        server 127.0.0.1:9092 down;
        server 127.0.0.1:9093 max_conns=30 max_fails=3 fail_timeout=30;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```











### 负载均衡策略

Nginx的upstream支持如下六种方式的分配算法：

|  算法名称  |       说明       |
| :--------: | :--------------: |
|    轮询    |     默认方式     |
|   weight   |     权重方式     |
|  ip_hash   |  依据ip分配方式  |
| least_conn | 依据最少连接方式 |
|  url_hash  | 依据URL分配方式  |
|    fair    | 依据响应时间方式 |





#### 轮询

轮询是upstream模块负载均衡默认的策略。每个请求会按时间顺序逐个分配到不同的后端服务器。轮询不需要额外的配置



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```





#### weight

加权轮询

weight=number:用来设置服务器的权重，默认为1，权重数据越大，被分配到请求的几率越大；该权重值，主要是针对实际工作环境中不同的后端服务器硬件配置进行调整的，所有此策略比较适合服务器的硬件配置差别比较大的情况



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        server 127.0.0.1:9091 weight=1;
        server 127.0.0.1:9092 weight=1;
        server 127.0.0.1:9093 weight=2;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```



启动后端服务

![image-20230515145344813](img/Nginx学习笔记/image-20230515145344813.png)



![image-20230515145358379](img/Nginx学习笔记/image-20230515145358379.png)

![image-20230515145409105](img/Nginx学习笔记/image-20230515145409105.png)



修改配置文件并启动nginx



访问10次

```sh
PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/                                                                                        

StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:06 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:0                     6 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53f60a<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:10 GMT
                    Server: nginx/1.24.0

                    当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     0 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：160b3b04-c9a2-4279-97c5-e52fc391afcf<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:12 GMT
                    Server: nginx/1.24.0

                    当前机器id：160b3b04-c9a2-4279-97c5-e52fc391...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     2 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:13 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     3 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：4
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:14 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     4 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53f60a<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:15 GMT
                    Server: nginx/1.24.0

                    当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     5 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：160b3b04-c9a2-4279-97c5-e52fc391afcf<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:16 GMT
                    Server: nginx/1.24.0

                    当前机器id：160b3b04-c9a2-4279-97c5-e52fc391...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     6 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：5
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:17 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1                     7 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：6
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:17 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1
                    7 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53f60a<br>当前访问ip：127.0.0.1<br>访问计数：4
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 06:56:19 GMT
                    Server: nginx/1.24.0

                    当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 06:56:1
                    9 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0>
```



访问计数：

* a96b297d-4bc5-49d2-8fc8-ec54bf53f60a：3次
* 160b3b04-c9a2-4279-97c5-e52fc391afcf：2次
* 3f8df244-f927-4536-91fd-f646f8587ff7：5次



符合预期





#### ip_hash

当对后端的多台动态应用服务器做负载均衡时，ip_hash指令能够将某个客户端IP的请求通过哈希算法定位到同一台后端服务器上。这样，当来自某一个IP的用户在后端Web服务器A上登录后，在访问该站点的其他URL，能保证其访问的还是后端web服务器A

使用ip_hash指令无法保证后端服务器的负载均衡，可能导致有些后端服务器接收到的请求多，有些后端服务器接收的请求少，而且设置后端服务器权重等方法将不起作用



|  语法  | ip_hash; |
| :----: | :------: |
| 默认值 |    —     |
|  位置  | upstream |



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        ip_hash;
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```





```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> ./nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```



访问测试

```sh
PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：10
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:04:12 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:04:1                     2 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：11
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:04:12 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:04:1                     2 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：12
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:04:13 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:04:1
                    3 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS D:\opensoft\nginx-1.24.0> curl http://localhost:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：3f8df244-f927-4536-91fd-f646f8587ff7<br>当前访问ip：127.0.0.1<br>访问计数：13
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 104
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:04:14 GMT
                    Server: nginx/1.24.0

                    当前机器id：3f8df244-f927-4536-91fd-f646f858...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 104], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:04:1
                    4 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 104



PS D:\opensoft\nginx-1.24.0>
```



127.0.0.1的ip路由到了3f8df244-f927-4536-91fd-f646f8587ff7机器上



```sh
PS D:\opensoft\nginx-1.24.0> curl http://192.168.15.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53f60a<br>当前访问ip：127.0.0.1<br>访问计数：7
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:06:23 GMT
                    Server: nginx/1.24.0

                    当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:06:2                     3 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://192.168.15.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53f60a<br>当前访问ip：127.0.0.1<br>访问计数：8
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:06:24 GMT
                    Server: nginx/1.24.0

                    当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:06:2
                    4 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://192.168.15.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53f60a<br>当前访问ip：127.0.0.1<br>访问计数：9
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:06:26 GMT
                    Server: nginx/1.24.0

                    当前机器id：a96b297d-4bc5-49d2-8fc8-ec54bf53...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:06:2
                    6 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0>
```



192.168.15.1的ip路由到了a96b297d-4bc5-49d2-8fc8-ec54bf53f60a机器上



```sh
PS D:\opensoft\nginx-1.24.0> curl http://172.20.240.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：160b3b04-c9a2-4279-97c5-e52fc391afcf<br>当前访问ip：127.0.0.1<br>访问计数：6
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:07:34 GMT
                    Server: nginx/1.24.0

                    当前机器id：160b3b04-c9a2-4279-97c5-e52fc391...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:07:3                     4 GMT]...}                                                                                                                  Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://172.20.240.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：160b3b04-c9a2-4279-97c5-e52fc391afcf<br>当前访问ip：127.0.0.1<br>访问计数：7
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:07:35 GMT
                    Server: nginx/1.24.0

                    当前机器id：160b3b04-c9a2-4279-97c5-e52fc391...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:07:3
                    5 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://172.20.240.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：160b3b04-c9a2-4279-97c5-e52fc391afcf<br>当前访问ip：127.0.0.1<br>访问计数：8
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Mon, 15 May 2023 07:07:37 GMT
                    Server: nginx/1.24.0

                    当前机器id：160b3b04-c9a2-4279-97c5-e52fc391...
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8], [Date, Mon, 15 May 2023 07:07:3
                    7 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0>
```



172.20.240.1的ip路由到了160b3b04-c9a2-4279-97c5-e52fc391afcf机器上





#### least_conn

最少连接，把请求转发给连接数较少的后端服务器。轮询算法是把请求平均的转发给各个后端，使它们的负载大致相同；但是，有些请求占用的时间很长，会导致其所在的后端负载较高。这种情况下，least_conn这种方式就可以达到更好的负载均衡效果

此负载均衡策略适合请求处理时间长短不一造成服务器过载的情况



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        least_conn;
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```







#### url_hash

按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，要配合缓存命中来使用。同一个资源多次请求，可能会到达不同的服务器上，导致不必要的多次下载，缓存命中率不高，以及一些资源时间的浪费。而使用url_hash，可以使得同一个url（也就是同一个资源请求）会到达同一台服务器，一旦缓存住了资源，再此收到请求，就可以从缓存中读取。



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        url_hash;
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```





#### fair

fair采用的不是内建负载均衡使用的轮换的均衡算法，而是可以根据页面大小、加载时间长短智能的进行负载均衡

fair属于第三方模块实现的负载均衡。需要添加`nginx-upstream-fair`模块



1. 下载nginx-upstream-fair模块

```
下载地址为:
	https://github.com/gnosek/nginx-upstream-fair
```

2. 将下载的文件上传到服务器并进行解压缩

```
unzip nginx-upstream-fair-master.zip
```

3. 重命名资源

```
mv nginx-upstream-fair-master fair
```

4. 使用./configure命令将资源添加到Nginx模块中

```
./configure --add-module=/root/fair
```

5. 编译

```
make
```

6. 更新Nginx
7. 编译测试使用Nginx



```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     
upstream testserver{
        fair;
        server 127.0.0.1:9091;
        server 127.0.0.1:9092;
        server 127.0.0.1:9093;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
        }
}
}
```















## Nginx四层负载均衡

### 概述

Nginx在1.9之后，增加了一个stream模块，用来实现四层协议的转发、代理、负载均衡等。stream模块的用法跟http的用法类似，允许我们配置一组TCP或者UDP等协议的监听，然后通过proxy_pass来转发我们的请求，通过upstream添加多个后端服务，实现负载均衡。

四层协议负载均衡的实现，一般都会用到LVS、HAProxy、F5等，要么很贵要么配置很麻烦，而Nginx的配置相对来说更简单，更能快速完成工作。





### 添加stream模块

Nginx默认是没有编译这个模块的，需要使用到stream模块，那么需要在编译的时候加上`--with-stream`





### 相关指令

#### stream指令

该指令提供在其中指定流服务器指令的配置文件上下文。和http指令同级



|  语法  | stream { ... } |
| :----: | :------------: |
| 默认值 |       —        |
|  位置  |      main      |



#### upstream指令

该指令和http的upstream指令类似





#### 示例

```sh
stream {
        upstream redisbackend {
                server 127.0.0.1:6379;
                server 127.0.0.1:6378;
        }
        upstream tomcatbackend {
        		server 127.0.0.1:8080;
        }
        server {
                listen  81;
                proxy_pass redisbackend;
        }
        server {
        		listen	82;
        		proxy_pass tomcatbackend;
        }
}
```













# Nginx缓存集成

## 概述

缓存就是数据交换的缓冲区(称作:Cache),当用户要获取数据的时候，会先从缓存中去查询获取数据，如果缓存中有就会直接返回给用户，如果缓存中没有，则会发请求从服务器重新查询数据，将数据返回给用户的同时将数据放入缓存，下次用户就会直接从缓存中获取数据。



缓存其实在很多场景中都有用到，比如：

|       场景       |           作用           |
| :--------------: | :----------------------: |
| 操作系统磁盘缓存 |     减少磁盘机械操作     |
|    数据库缓存    |   减少文件系统的IO操作   |
|   应用程序缓存   |    减少对数据库的查询    |
|  Web服务器缓存   | 减少对应用服务器请求次数 |
|    浏览器缓存    |   减少与后台的交互次数   |





## 缓存的优缺点

优点：

* 减少数据传输，节省网络流量，加快响应速度，提升用户体验
* 减轻服务器压力
* 提供服务端的高可用性



缺点：

* 数据的不一致
* 增加成本





## Nginx的web缓存

Nginx是从0.7.48版开始提供缓存功能。Nginx是基于Proxy Store来实现的，其原理是把URL及相关组合当做Key,在使用MD5算法对Key进行哈希，得到硬盘上对应的哈希目录路径，从而将缓存内容保存在该目录中。它可以支持任意URL连接，同时也支持404/301/302这样的非200状态码。Nginx即可以支持对指定URL或者状态码设置过期时间，也可以使用purge命令来手动清除指定URL的缓存。





## 相关指令

Nginx的web缓存服务主要是使用`ngx_http_proxy_module`模块相关指令集来完成





### proxy_cache_path指令

该指定用于设置缓存文件的存放路径



| 默认值 |                              —                               |
| :----: | :----------------------------------------------------------: |
|  语法  | proxy_cache_path path [levels=number] <br/>keys_zone=zone_name:zone_size [inactive=time]\[max_size=size]; |
|  位置  |                             http                             |



* path：缓存路径地址
* levels:：指定该缓存空间对应的目录，最多可以设置3层，每层取值为1|2
* keys_zone：用来为这个缓存区设置名称和指定大小
* inactive：指定缓存的数据多次时间未被访问就将被删除
* max_size：设置最大缓存空间，如果缓存空间存满，默认会覆盖缓存时间最长的资源



使用示例：

```sh
http{
	proxy_cache_path /usr/local/proxy_cache keys_zone=itcast:200m  levels=1:2:1 inactive=1d max_size=20g;
}
```







### proxy_cache指令

该指令用来开启或关闭代理缓存，如果是开启则自定使用哪个缓存区来进行缓存



|  语法  | proxy_cache zone_name\|off; |
| :----: | :-------------------------: |
| 默认值 |      proxy_cache off;       |
|  位置  |   http、server、location    |



* zone_name：指定使用缓存区的名称







### proxy_cache_key指令

该指令用来设置web缓存的key值，Nginx会根据key值MD5哈希存缓存



|  语法  |               proxy_cache_key key;                |
| :----: | :-----------------------------------------------: |
| 默认值 | proxy_cache_key \$scheme\$proxy_host$request_uri; |
|  位置  |              http、server、location               |





### proxy_cache_valid指令

该指令用来对不同返回状态码的URL设置不同的缓存时间



|  语法  | proxy_cache_valid [code ...] time; |
| :----: | :--------------------------------: |
| 默认值 |                 —                  |
|  位置  |       http、server、location       |




为200和302的响应URL设置10分钟缓存，为404的响应URL设置1分钟缓存：

```sh
proxy_cache_valid 200 302 10m;
proxy_cache_valid 404 1m;
```




对所有响应状态码的URL都设置1分钟缓存：

```sh
proxy_cache_valid any 1m;
```





### proxy_cache_min_uses指令

该指令用来设置资源被访问多少次后被缓存



|  语法  | proxy_cache_min_uses number; |
| :----: | :--------------------------: |
| 默认值 |   proxy_cache_min_uses 1;    |
|  位置  |    http、server、location    |





### proxy_cache_methods指令

该指令用户设置缓存哪些HTTP方法

默认缓存HTTP的GET和HEAD方法，不缓存POST方法



|  语法  | proxy_cache_methods GET\|HEAD\|POST; |
| :----: | :----------------------------------: |
| 默认值 |    proxy_cache_methods GET HEAD;     |
|  位置  |        http、server、location        |







## 实现

启动刚才的后端服务

```sh
java -jar nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar --server.port=9091
```



配置文件：

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     proxy_cache_path ./cache levels=2:1 keys_zone=test:200m inactive=1d max_size=10g;
     
upstream testserver{
        server 127.0.0.1:9091;
}

server {
        listen          8080;
        server_name     localhost;
        location / {
                proxy_pass http://testserver;
                proxy_cache test;
                proxy_cache_min_uses 3;
                proxy_cache_valid 200 5d;
                proxy_cache_valid 404 30s;
                proxy_cache_valid any 1m;
                add_header nginx-cache "$upstream_cache_status";
        }
}
}
```



启动

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0>
```



访问：

```sh
PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：1
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: MISS
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:39 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161...
Forms             : {}                                                                                                                          Headers           : {[Connection, keep-alive], [nginx-cache, MISS], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}         Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：2
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: MISS
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:42 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161...
Forms             : {}                                                                                                                          Headers           : {[Connection, keep-alive], [nginx-cache, MISS], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}         Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: MISS
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:43 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161...
Forms             : {}                                                                                                                          Headers           : {[Connection, keep-alive], [nginx-cache, MISS], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}         Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: HIT
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:44 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161-...
Forms             : {}                                                                                                                          Headers           : {[Connection, keep-alive], [nginx-cache, HIT], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}          Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: HIT
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:45 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161-...
Forms             : {}                                                                                                                          Headers           : {[Connection, keep-alive], [nginx-cache, HIT], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}          Images            : {}                                                                                                                          InputFields       : {}                                                                                                                          Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: HIT
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:45 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161-...
Forms             : {}
Headers           : {[Connection, keep-alive], [nginx-cache, HIT], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0> curl http://127.0.0.1:8080/


StatusCode        : 200
StatusDescription :
Content           : 当前机器id：181b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>当前访问ip：127.0.0.1<br>访问计数：3
RawContent        : HTTP/1.1 200
                    Connection: keep-alive
                    nginx-cache: HIT
                    Content-Length: 103
                    Content-Type: text/plain;charset=UTF-8
                    Date: Wed, 17 May 2023 06:24:46 GMT
                    Server: nginx/1.24.0

                    当前机器id：181b9ee6-1161-...
Forms             : {}
Headers           : {[Connection, keep-alive], [nginx-cache, HIT], [Content-Length, 103], [Content-Type, text/plain;charset=UTF-8]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 103



PS D:\opensoft\nginx-1.24.0>
```



服务器日志：

```sh
2023-05-17 14:24:39.480  INFO 28612 --- [nio-9091-exec-1] m.n.controller.TestController            : 127.0.0.1访问当前实例,访问计数：1
2023-05-17 14:24:42.561  INFO 28612 --- [nio-9091-exec-2] m.n.controller.TestController            : 127.0.0.1访问当前实例,访问计数：2
2023-05-17 14:24:43.537  INFO 28612 --- [nio-9091-exec-3] m.n.controller.TestController            : 127.0.0.1访问当前实例,访问计数：3
```



当访问3次后，nginx将数据缓存，之后的访问直接从缓存里读取，访问计数不再增加



```sh
PS D:\opensoft\nginx-1.24.0> ls


    目录: D:\opensoft\nginx-1.24.0


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/5/17     14:24                cache
d-----         2023/5/12     14:54                conf
d-----         2023/4/11     23:31                contrib
d-----         2023/4/11     23:31                docs
d-----          2023/5/3     14:45                html
d-----         2023/5/12     14:54                key
d-----         2023/5/17     14:23                logs
d-----         2023/4/29     13:29                temp
-a----         2023/4/11     23:29        3811328 nginx.exe


PS D:\opensoft\nginx-1.24.0> cd .\cache\
PS D:\opensoft\nginx-1.24.0\cache> ls


    目录: D:\opensoft\nginx-1.24.0\cache


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/5/17     14:24                96


PS D:\opensoft\nginx-1.24.0\cache> cd .\96\
PS D:\opensoft\nginx-1.24.0\cache\96> ls


    目录: D:\opensoft\nginx-1.24.0\cache\96


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/5/17     14:24                a


PS D:\opensoft\nginx-1.24.0\cache\96> cd .\a\
PS D:\opensoft\nginx-1.24.0\cache\96\a> ls


    目录: D:\opensoft\nginx-1.24.0\cache\96\a


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2023/5/17     14:24            598 b5268943f8305b7b1216f7b0efe65a96


PS D:\opensoft\nginx-1.24.0\cache\96\a> cat .\b5268943f8305b7b1216f7b0efe65a96
?kd+sdd悩F"i?
KEY: http://testserver/
HTTP/1.1 200
Content-Type: text/plain;charset=UTF-8
Content-Length: 103
Date: Wed, 17 May 2023 06:24:42 GMT
Connection: close

褰撳墠鏈哄櫒id锛?81b9ee6-1161-4ebb-bc33-fb93d3cfd541<br>褰撳墠璁块棶ip锛?27.0.0.1<br>璁块棶璁℃暟锛?
PS D:\opensoft\nginx-1.24.0\cache\96\a>
```



nginx缓存目录生成了文件









## Nginx缓存的清除

可以使用ngx_cache_purge模块来实现



### 添加模块

（1）下载ngx_cache_purge模块对应的资源包，并上传到服务器上。

```
ngx_cache_purge-2.3.tar.gz
```



（2）对资源文件进行解压缩

```
tar -zxf ngx_cache_purge-2.3.tar.gz
```



（3）修改文件夹名称，方便后期配置

```
mv ngx_cache_purge-2.3 purge
```



（4）查询Nginx的配置参数

```
nginx -V
```



（5）进入Nginx的安装目录，使用./configure进行参数配置

```
./configure --add-module=/root/nginx/module/purge
```



（6）使用make进行编译

```
make
```



（7）将nginx安装目录的nginx二级制可执行文件备份

```
mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginxold
```



（8）将编译后的objs中的nginx拷贝到nginx的sbin目录下

```
cp objs/nginx /usr/local/nginx/sbin
```



（9）使用make进行升级

```
make upgrade
```







### ngx_cache_purge指令



|  语法  | ngx_cache_purge zone_name key; |
| :----: | :----------------------------: |
| 默认值 |               -                |
|  位置  |            location            |



使用示例：

```sh
server{
	location ~/purge(/.*) {
		proxy_cache_purge test test1;
	}
}
```











## Nginx设置资源不缓存

### 概述

不是所有的数据都适合进行缓存。比如说对于一些经常发生变化的数据。如果进行缓存的话，就很容易出现用户访问到的数据不是服务器真实的数据。所以对于这些资源我们在缓存的过程中就需要进行过滤，不进行缓存





### 相关指令

#### proxy_no_cache指令

该指令是用来定义不将数据进行缓存的条件



|  语法  | proxy_no_cache string ...; |
| :----: | :------------------------: |
| 默认值 |             —              |
|  位置  |   http、server、location   |



示例：

```sh
proxy_no_cache $cookie_nocache $arg_nocache $arg_comment;
```



* $cookie_nocache：当前请求的cookie中键的名称为nocache对应的值
* $arg_nocache：当前请求的参数中属性名为nocache对应的属性值
* $arg_comment：当前请求的参数中属性名comment对应的属性值



多个条件中至少有一个不为空且不等于"0"，则条件满足成立





#### proxy_cache_bypass指令

该指令是用来设置不从缓存中获取数据的条件



|  语法  | proxy_cache_bypass string ...; |
| :----: | :----------------------------: |
| 默认值 |               —                |
|  位置  |     http、server、location     |



```sh
proxy_cache_bypass $cookie_nocache $arg_nocache $arg_comment;
```



















# Nginx制作下载站点

## 概述

nginx下载站点示例：http://nginx.org/download/

![image-20230518140335077](img/Nginx学习笔记/image-20230518140335077.png)



下载站点是用来提供用户来下载相关资源的网站



nginx使用的是模块ngx_http_autoindex_module来实现的，该模块处理以斜杠("/")结尾的请求，并生成目录列表





## 相关指令

### autoindex指令

启用或禁用目录列表输出



|  语法  |   autoindex on\|off;   |
| :----: | :--------------------: |
| 默认值 |     autoindex off;     |
|  位置  | http、server、location |





### autoindex_exact_size指令

对应HTLM格式，指定是否在目录列表展示文件的详细大小



|  语法  | autoindex_exact_size  on\|off; |
| :----: | :----------------------------: |
| 默认值 |   autoindex_exact_size  on;    |
|  位置  |     http、server、location     |



* on：显示出文件的确切大小，单位是bytes
* off：显示出文件的大概大小，单位是kB或者MB或者GB





### autoindex_format指令

设置目录列表的格式，该指令在1.7.9及以后版本中出现



|  语法  | autoindex_format html\|xml\|json\|jsonp; |
| :----: | :--------------------------------------: |
| 默认值 |          autoindex_format html;          |
|  位置  |          http、server、location          |





### autoindex_localtime指令

对应HTML格式，是否在目录列表上显示时间



|  语法  | autoindex_localtime on \| off; |
| :----: | :----------------------------: |
| 默认值 |    autoindex_localtime off;    |
|  位置  |     http、server、location     |



* off：显示的文件时间为GMT时间
* on：显示的文件时间为文件的服务器时间









## 实现

随便准备几个文件，放入nginx的file目录下



```sh
PS D:\opensoft\nginx-1.24.0> ls


    目录: D:\opensoft\nginx-1.24.0


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/5/17     14:24                cache
d-----         2023/5/12     14:54                conf
d-----         2023/4/11     23:31                contrib
d-----         2023/4/11     23:31                docs
d-----         2023/5/18     14:11                file
d-----          2023/5/3     14:45                html
d-----         2023/5/12     14:54                key
d-----         2023/5/17     14:23                logs
d-----         2023/4/29     13:29                temp
-a----         2023/4/11     23:29        3811328 nginx.exe


PS D:\opensoft\nginx-1.24.0> cd .\file\
PS D:\opensoft\nginx-1.24.0\file> ls


    目录: D:\opensoft\nginx-1.24.0\file


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2023/5/10     13:46             22 application.yml
-a----         2023/5/10     14:01       17620762 nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar
-a----         2023/5/10     14:16           7701 readme.md


PS D:\opensoft\nginx-1.24.0\file>
```



修改配置文件：

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location /download{
            alias ./file;
            autoindex on;
            autoindex_exact_size on;
            autoindex_format html;
            autoindex_localtime on;
         }
   }
}
```





校验并启动

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0>
```



访问

http://localhost:8080/download/



![image-20230518142759949](img/Nginx学习笔记/image-20230518142759949.png)



点击下载

![image-20230518142821816](img/Nginx学习笔记/image-20230518142821816.png)





更改配置文件，autoindex_exact_size改成off

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location /download{
            alias ./file;
            autoindex on;
            autoindex_exact_size off;
            autoindex_format html;
            autoindex_localtime on;
         }
   }
}
```



```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> ./nginx -s reload
PS D:\opensoft\nginx-1.24.0>
```



再次访问

![image-20230518143035148](img/Nginx学习笔记/image-20230518143035148.png)



更改配置文件，autoindex_localtime改成off

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location /download{
            alias ./file;
            autoindex on;
            autoindex_exact_size off;
            autoindex_format html;
            autoindex_localtime off;
         }
   }
}
```





重新加载并访问

![image-20230518143215516](img/Nginx学习笔记/image-20230518143215516.png)

```sh
../
application.yml                                    10-May-2023 13:46                  22
nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar        10-May-2023 14:01            17620762
readme.md                                          10-May-2023 14:16                7701
```



```html
<html>
<head><title>Index of /download/</title></head>
<body>
<h1>Index of /download/</h1><hr><pre><a href="../">../</a>
<a href="application.yml">application.yml</a>                                    10-May-2023 13:46                  22
<a href="nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar">nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar</a>        10-May-2023 14:01            17620762
<a href="readme.md">readme.md</a>                                          10-May-2023 14:16                7701
</pre><hr></body>
</html>
```









更改配置文件，更改autoindex_format为xml格式

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location /download{
            alias ./file;
            autoindex on;
            autoindex_exact_size on;
            autoindex_format xml;
            autoindex_localtime on;
         }
   }
}
```





重新加载并访问

![image-20230518143422839](img/Nginx学习笔记/image-20230518143422839.png)



```xml
<list>
   <file mtime="2023-05-10T05:46:49Z" size="22">application.yml</file>
   <file mtime="2023-05-10T06:01:21Z" size="17620762">nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar</file>
   <file mtime="2023-05-10T06:16:16Z" size="7701">readme.md</file>
</list>
```





更改配置文件，更改autoindex_format为json格式

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

server {
        listen          8080;
        server_name     localhost;
        location /download{
            alias ./file;
            autoindex on;
            autoindex_exact_size on;
            autoindex_format json;
            autoindex_localtime on;
         }
   }
}
```





重新加载并访问

![image-20230518143558030](img/Nginx学习笔记/image-20230518143558030.png)



```json
[
    {
        "name": "application.yml",
        "type": "file",
        "mtime": "Wed, 10 May 2023 05:46:49 GMT",
        "size": 22
    },
    {
        "name": "nginx-reverse-proxy-demo-0.0.1-SNAPSHOT.jar",
        "type": "file",
        "mtime": "Wed, 10 May 2023 06:01:21 GMT",
        "size": 17620762
    },
    {
        "name": "readme.md",
        "type": "file",
        "mtime": "Wed, 10 May 2023 06:16:16 GMT",
        "size": 7701
    }
]
```











# Nginx实现动静分离

## 概述

动：指的是动态资源，后台应用程序的业务处理

静：网站的静态资源(html,javaScript,css,images等文件)



什么是动静分离？

动静分离是将两者进行分开部署访问，提供用户进行访问。举例说明就是以后所有和静态资源相关的内容都交给Nginx来部署访问，非静态内容则交个类似于Tomcat的服务器来部署访问



Nginx在处理静态资源的时候，效率是非常高的，而且Nginx的并发访问量也是名列前茅，而Tomcat则相对比较弱一些，所以把静态资源交个Nginx后，可以减轻Tomcat服务器的访问压力并提高静态资源的访问速度

动静分离以后，降低了动态资源和静态资源的耦合度。如动态资源宕机了也不影响静态资源的展示

实现动静分离的方式很多，比如静态资源可以部署到CDN、Nginx等服务器上，动态资源可以部署到Tomcat,weblogic或者websphere上







## 实现

创建一个spring boot程序，端口为9888

controller内容如下：

```java
package mao.nginx_dynamic_and_static_separation_demo.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;
import java.util.StringJoiner;

/**
 * Project name(项目名称)：nginx_dynamic_and_static_separation_demo
 * Package(包名): mao.nginx_dynamic_and_static_separation_demo.controller
 * Class(类名): TestController
 * Author(作者）: mao
 * Author QQ：1296193245
 * GitHub：https://github.com/maomao124/
 * Date(创建日期)： 2023/5/19
 * Time(创建时间)： 13:22
 * Version(版本): 1.0
 * Description(描述)： 无
 */

@RestController
@RequestMapping("/student")
public class TestController
{
    private static final Logger log = LoggerFactory.getLogger(TestController.class);


    public static int getIntRandom(int min, int max)
    {
        if (min > max)
        {
            min = max;
        }
        return min + (int) (Math.random() * (max - min + 1));
    }


    /**
     * 通过id得到学生信息
     *
     * @param id id
     * @return {@link Student}
     */
    @GetMapping("/{id}")
    public Student getById(@PathVariable Long id)
    {
        Student student = new Student();
        student.id = id;
        student.name = "张三";
        student.sex = "男";
        student.age = getIntRandom(15, 25);
        log.info(student.toString());
        return student;
    }


    /**
     * 得到所有学生信息
     *
     * @return {@link List}<{@link Student}>
     */
    @GetMapping("/all")
    public List<Student> getAll()
    {
        List<Student> studentList = new ArrayList<>(3);
        Student student = new Student();
        student.id = 10001L;
        student.name = "张三";
        student.sex = "男";
        student.age = getIntRandom(15, 25);
        studentList.add(student);
        student = new Student();
        student.id = 10002L;
        student.name = "李四";
        student.sex = "女";
        student.age = getIntRandom(15, 25);
        studentList.add(student);
        student = new Student();
        student.id = 10003L;
        student.name = "王五";
        student.sex = "男";
        student.age = getIntRandom(15, 25);
        studentList.add(student);
        log.info(studentList.toString());
        return studentList;
    }

    /**
     * 学生实体类
     *
     * @author mao
     * @date 2023/05/19
     */
    public static class Student
    {
        public Long id;
        public String name;
        public String sex;
        public Integer age;

        public Long getId()
        {
            return id;
        }

        public String getName()
        {
            return name;
        }

        public String getSex()
        {
            return sex;
        }

        public Integer getAge()
        {
            return age;
        }

        @Override
        public String toString()
        {
            return new StringJoiner(", ", Student.class.getSimpleName() + "[", "]")
                    .add("id=" + id)
                    .add("name='" + name + "'")
                    .add("sex='" + sex + "'")
                    .add("age=" + age)
                    .toString();
        }
    }
}

```





启动程序



准备两张图片1.png和2.png，放入nginx安装目录的static目录

```sh
PS D:\opensoft\nginx-1.24.0> ls


    目录: D:\opensoft\nginx-1.24.0


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/5/17     14:24                cache
d-----         2023/5/12     14:54                conf
d-----         2023/4/11     23:31                contrib
d-----         2023/4/11     23:31                docs
d-----         2023/5/18     14:11                file
d-----          2023/5/3     14:45                html
d-----         2023/5/12     14:54                key
d-----         2023/5/18     14:16                logs
d-----         2023/5/19     13:58                static
d-----         2023/4/29     13:29                temp
-a----         2023/4/11     23:29        3811328 nginx.exe


PS D:\opensoft\nginx-1.24.0> cd .\static\
PS D:\opensoft\nginx-1.24.0\static> ls


    目录: D:\opensoft\nginx-1.24.0\static


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2023/5/13     13:07         449442 1.png
-a----         2023/5/18     14:35          32901 2.png


PS D:\opensoft\nginx-1.24.0\static>
```





更改html目录下的index.html页面

```html
<!DOCTYPE html>

<!--
Project name(项目名称)：nginx_dynamic_and_static_separation_demo
  File name(文件名): index
  Authors(作者）: mao
  Author QQ：1296193245
  GitHub：https://github.com/maomao124/
  Date(创建日期)： 2023/5/19
  Time(创建时间)： 13:33
  Description(描述)： 无
-->

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>索引</title>
</head>
<body onload="load()">

<p>以下为动态资源：</p>


<p>学号为10001的学生：</p>
<p id="student"></p>

<p>所有学生：</p>
<p id="studentList"></p>



<br>
<br>

<p>以下为静态资源：</p>

<img src="/static/1.png">

<br>

<img src="/static/2.png">

</body>

<script>

    function load()
    {
        //XMLHttpRequest对象
        let xhr;
        //是否正在发送请求
        let isSending = false;
        //如果正在发送请求
        if (isSending === true)
        {
            //取消正在发送的请求
            xhr.abort();
        }
        //发起异步请求
        xhr = new XMLHttpRequest();
        //设置响应信息为json
        xhr.responseType = "json";
        //超时设置，单位为毫秒
        xhr.timeout = 5000;
        //超时的回调函数
        xhr.ontimeout = function ()
        {
            alert("请求超时，请稍后再试！");
        }
        //初始化，设置请求方式和url
        xhr.open("GET", "/student/10001");
        //设置状态为正在发送
        isSending = true;
        //发送异步请求
        xhr.send();

        xhr.onreadystatechange = function ()
        {
            //状态为4时处理
            if (xhr.readyState === 4)
            {
                //落在200-300之间处理
                if (xhr.status >= 200 && xhr.status < 300)
                {
                    //将状态设置成false
                    isSending = false;
                    console.log(xhr.response);
                    document.getElementById("student").innerHTML = JSON.stringify(xhr.response);
                }
            }
        }
        load2();
    }

    function load2()
    {
        //XMLHttpRequest对象
        let xhr;
        //是否正在发送请求
        let isSending = false;
        //如果正在发送请求
        if (isSending === true)
        {
            //取消正在发送的请求
            xhr.abort();
        }
        //发起异步请求
        xhr = new XMLHttpRequest();
        //设置响应信息为json
        xhr.responseType = "json";
        //超时设置，单位为毫秒
        xhr.timeout = 5000;
        //超时的回调函数
        xhr.ontimeout = function ()
        {
            alert("请求超时，请稍后再试！");
        }
        //初始化，设置请求方式和url
        xhr.open("GET", "/student/all");
        //设置状态为正在发送
        isSending = true;
        //发送异步请求
        xhr.send();

        xhr.onreadystatechange = function ()
        {
            //状态为4时处理
            if (xhr.readyState === 4)
            {
                //落在200-300之间处理
                if (xhr.status >= 200 && xhr.status < 300)
                {
                    //将状态设置成false
                    isSending = false;
                    console.log(xhr.response);
                    document.getElementById("studentList").innerHTML = JSON.stringify(xhr.response);
                }
            }
        }
    }

</script>

</html>
```



修改nginx配置文件

```sh
#配置运行Nginx进程生成的worker进程数
worker_processes 2;
#配置Nginx服务器运行对错误日志存放的路径
error_log ./logs/error.log;
#配置Nginx服务器允许时记录Nginx的master进程的PID文件路径和名称
pid ./logs/nginx.pid;
#配置Nginx服务是否以守护进程方法启动
#daemon on;



events{
	#设置Nginx网络连接序列化
	accept_mutex on;
	#设置Nginx的worker进程是否可以同时接收多个请求
	multi_accept on;
	#设置Nginx的worker进程最大的连接数
	worker_connections 1024;
	#设置Nginx使用的事件驱动模型
	#use epoll;
}


http{
	#定义MIME-Type
	include mime.types;
	default_type application/octet-stream;
     

upstream studentserver
{
  server 127.0.0.1:9888;
}

server {
        listen          8080;
        server_name     localhost;

          location /static{
            alias static;
         }

         location ~/.*\.(html|css|js){
                root html;
        }
           location /{
            proxy_pass http://studentserver;
         }
   }
}
```



检查并启动：

```sh
PS D:\opensoft\nginx-1.24.0> ./nginx -t
nginx: the configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\opensoft\nginx-1.24.0/conf/nginx.conf test is successful
PS D:\opensoft\nginx-1.24.0> start ./nginx
PS D:\opensoft\nginx-1.24.0>
```



访问

http://localhost:8080/index.html



![image-20230519141245149](img/Nginx学习笔记/image-20230519141245149.png)



![image-20230519141306555](img/Nginx学习笔记/image-20230519141306555.png)



![image-20230519141338115](img/Nginx学习笔记/image-20230519141338115.png)





都能正常访问

关闭后端服务，再次访问

![image-20230519141509674](img/Nginx学习笔记/image-20230519141509674.png)



![image-20230519141517779](img/Nginx学习笔记/image-20230519141517779.png)



无法获取到动态资源，但是静态资源依旧能获取













# Nginx高可用解决方案

## 概述

一台服务器宕机，还有其他两台对外提供服务，同时也可以实现后台服务器的不间断更新。如果是Nginx宕机了呢，那么整套系统都将服务对外提供服务了，这个如何解决？

![image-20230520124835763](img/Nginx学习笔记/image-20230520124835763.png)



需要两台以上的Nginx服务器对外提供服务，这样的话就可以解决其中一台宕机了，另外一台还能对外提供服务，但是如果是两台Nginx服务器的话，会有两个IP地址，用户该访问哪台服务器，用户怎么知道哪台是好的，哪台是宕机了的?



可以使用Keepalived来解决，Keepalived 软件由 C 编写的，最初是专为 LVS 负载均衡软件设计的，Keepalived 软件主要是通过 VRRP 协议实现高可用功能。





## VRRP介绍

VRRP（Virtual Route Redundancy Protocol）协议，翻译过来为虚拟路由冗余协议。VRRP协议将两台或多台路由器设备虚拟成一个设备，对外提供虚拟路由器IP,而在路由器组内部，如果实际拥有这个对外IP的路由器如果工作正常的话就是MASTER,MASTER实现针对虚拟路由器IP的各种网络功能。其他设备不拥有该虚拟IP，状态为BACKUP,处了接收MASTER的VRRP状态通告信息以外，不执行对外的网络功能。当主机失效时，BACKUP将接管原先MASTER的网络功能。



![image-20230520125048289](img/Nginx学习笔记/image-20230520125048289.png)



VRRP可以把一个虚拟路由器的责任动态分配到局域网上的 VRRP 路由器中的一台。其中的虚拟路由即Virtual路由是由VRRP路由群组创建的一个不真实存在的路由，这个虚拟路由也是有对应的IP地址。而且VRRP路由1和VRRP路由2之间会有竞争选择，通过选择会产生一个Master路由和一个Backup路由。

Master路由和Backup路由之间会有一个心跳检测，Master会定时告知Backup自己的状态，如果在指定的时间内，Backup没有接收到这个通知内容，Backup就会替代Master成为新的Master。Master路由有一个特权就是虚拟路由和后端服务器都是通过Master进行数据传递交互的，而备份节点则会直接丢弃这些请求和数据，不做处理，只是去监听Master的状态。





## Keepalived

解决方案如下：

![image-20230520125404450](img/Nginx学习笔记/image-20230520125404450.png)



虚拟IP(VIP)会在MASTER节点上，当MASTER节点上的keepalived出问题以后，因为BACKUP无法收到MASTER发出的VRRP状态通过信息，就会直接升为MASTER。VIP也会"漂移"到新的MASTER。



### 环境搭建

|       VIP       |       IP        |   主机名    | 主/从  |
| :-------------: | :-------------: | :---------: | :----: |
|                 | 192.168.200.133 | keepalived1 | Master |
| 192.168.200.222 |                 |             |        |
|                 | 192.168.200.122 | keepalived2 | Backup |





### keepalived的安装

1. 从官方网站下载keepalived,官网地址https://keepalived.org/
2. 将下载的资源上传到服务器
3. 创建keepalived目录，方便管理资源
4. 将压缩文件进行解压缩，解压缩到指定的目录
5. 对keepalived进行配置，编译和安装：`./configure --sysconf=/etc --prefix=/usr/local` 和`make && make install`命令



/usr/local/sbin目录下的`keepalived`,是系统配置脚本，用来启动和关闭keepalived

`/etc/keepalived/keepalived.conf`：keepalived的系统配置文件





### Keepalived配置文件

打开keepalived.conf配置文件

这里面会分三部，第一部分是global全局配置、第二部分是vrrp相关配置、第三部分是LVS相关配置。



```sh
global全局部分：
global_defs {
   #通知邮件，当keepalived发送切换时需要发email给具体的邮箱地址
   notification_email {
     12345@qq.com
   }
   #设置发件人的邮箱信息
   notification_email_from 12345@qq.com
   #指定smpt服务地址
   smtp_server 192.168.200.1
   #指定smpt服务连接超时时间
   smtp_connect_timeout 30
   #运行keepalived服务器的一个标识，可以用作发送邮件的主题信息
   router_id LVS_DEVEL
   
   #默认是不跳过检查。检查收到的VRRP通告中的所有地址可能会比较耗时，设置此命令的意思是，如果通告与接收的上一个通告来自相同的master路由器，则不执行检查(跳过检查)
   vrrp_skip_check_adv_addr
   #严格遵守VRRP协议。
   vrrp_strict
   #在一个接口发送的两个免费ARP之间的延迟。可以精确到毫秒级。默认是0
   vrrp_garp_interval 0
   #在一个网卡上每组na消息之间的延迟时间，默认为0
   vrrp_gna_interval 0
}
```

```sh
VRRP部分，该部分可以包含以下四个子模块
1. vrrp_script
2. vrrp_sync_group
3. garp_group
4. vrrp_instance
我们会用到第一个和第四个，
#设置keepalived实例的相关信息，VI_1为VRRP实例名称
vrrp_instance VI_1 {
    state MASTER  		#有两个值可选MASTER主 BACKUP备
    interface ens33		#vrrp实例绑定的接口，用于发送VRRP包[当前服务器使用的网卡名称]
    virtual_router_id 51#指定VRRP实例ID，范围是0-255
    priority 100		#指定优先级，优先级高的将成为MASTER
    advert_int 1		#指定发送VRRP通告的间隔，单位是秒
    authentication {	#vrrp之间通信的认证信息
        auth_type PASS	#指定认证方式。PASS简单密码认证(推荐)
        auth_pass 1111	#指定认证使用的密码，最多8位
    }
    virtual_ipaddress { #虚拟IP地址设置虚拟IP地址，供用户访问使用，可设置多个，一行一个
        192.168.200.222
    }
}
```



服务器1的配置：

```sh
global_defs {
   notification_email {
        12345@qq.com
   }
   notification_email_from 12345@qq.com
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id keepalived1
   vrrp_skip_check_adv_addr
   vrrp_strict
   vrrp_garp_interval 0
   vrrp_gna_interval 0
}

vrrp_instance VI_1 {
    state MASTER
    interface ens33
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.222
    }
}
```



服务器2的配置：

```sh
! Configuration File for keepalived

global_defs {
   notification_email {
        12345@qq.com
   }
   notification_email_from 12345@qq.com
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id keepalived2
   vrrp_skip_check_adv_addr
   vrrp_strict
   vrrp_garp_interval 0
   vrrp_gna_interval 0
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens33
    virtual_router_id 51
    priority 90
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.222
    }
}
```







### vrrp_script

keepalived只能做到对网络故障和keepalived本身的监控，即当出现网络故障或者keepalived本身出现问题时，进行切换。但是这些还不够，我们还需要监控keepalived所在服务器上的其他业务，比如Nginx,如果Nginx出现异常了，仅仅keepalived保持正常，是无法完成系统的正常工作的，因此需要根据业务进程的运行状态决定是否需要进行主备切换，这个时候，我们可以通过编写脚本对业务进程进行检测监控。



在keepalived配置文件中添加对应的配置

```sh
vrrp_script 脚本名称
{
    script "脚本位置"
    interval 3 #执行时间间隔
    weight -20 #动态调整vrrp_instance的优先级
}
```





编写脚本ck_nginx.sh

```sh
#!/bin/bash
num=`ps -C nginx --no-header | wc -l`
if [ $num -eq 0 ];then
 /usr/local/nginx/sbin/nginx
 sleep 2
 if [ `ps -C nginx --no-header | wc -l` -eq 0 ]; then
  killall keepalived
 fi
fi
```



* Linux ps命令用于显示当前进程 (process) 的状态。
* -C(command) :指定命令的所有进程
* --no-header 排除标题



为脚本文件设置权限

```sh
chmod 755 ck_nginx.sh
```





将脚本添加到

```sh
vrrp_script ck_nginx {
   script "/etc/keepalived/ck_nginx.sh" #执行脚本的位置
   interval 2		#执行脚本的周期，秒为单位
   weight -20		#权重的计算方式
}
vrrp_instance VI_1 {
    state MASTER
    interface ens33
    virtual_router_id 10
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.111
    }
    track_script {
      ck_nginx
    }
}
```













# Nginx的扩展模块

## Lua

### 概述

Lua是一种轻量、小巧的脚本语言，用标准C语言编写并以源代码形式开发。设计的目的是为了嵌入到其他应用程序中，从而为应用程序提供灵活的扩展和定制功能。



### 特点

* 轻量级：Lua用标准C语言编写并以源代码形式开发，编译后仅仅一百余千字节，可以很方便的嵌入到其他程序中
* 可扩展：Lua提供非常丰富易于使用的扩展接口和机制，由宿主语言(通常是C或C++)提供功能，Lua可以使用它们，就像内置的功能一样
* 支持面向过程编程和函数式编程





### 应用场景

* 游戏开发
* 独立应用脚本
* web应用脚本
* 扩展和数据库插件
* 系统安全





### 安装

Lua的官网地址为:`https://www.lua.org`



![image-20230522135557858](img/Nginx学习笔记/image-20230522135557858.png)



点击下载



![image-20230522135834059](img/Nginx学习笔记/image-20230522135834059.png)



点击 get a binary 

![image-20230522135901480](img/Nginx学习笔记/image-20230522135901480.png)



再点击下载

![image-20230522135939017](img/Nginx学习笔记/image-20230522135939017.png)



找到对应的版本下载



下载完之后，解压

```sh
PS D:\opensoft\lua-5.4.2> ls


    目录: D:\opensoft\lua-5.4.2


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2021/1/13      1:35         356234 lua54.dll
-a----         2021/1/13      1:38         122006 lua54.exe
-a----         2021/1/13      1:38         299268 luac54.exe
-a----         2021/1/13      1:38         125374 wlua54.exe


PS D:\opensoft\lua-5.4.2>
```



复制可执行文件，将54去掉

```sh
PS D:\opensoft\lua-5.4.2> ls


    目录: D:\opensoft\lua-5.4.2


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2021/1/13      1:38         122006 lua.exe
-a----         2021/1/13      1:35         356234 lua54.dll
-a----         2021/1/13      1:38         122006 lua54.exe
-a----         2021/1/13      1:38         299268 luac.exe
-a----         2021/1/13      1:38         299268 luac54.exe
-a----         2021/1/13      1:38         125374 wlua.exe
-a----         2021/1/13      1:38         125374 wlua54.exe


PS D:\opensoft\lua-5.4.2>
```





测试

```sh
PS D:\opensoft\lua-5.4.2> ./lua
Lua 5.4.2  Copyright (C) 1994-2020 Lua.org, PUC-Rio
> print("hello")
hello
> 1+1
2
> 0.1+0.2
0.3
>
```





添加到环境变量

D:\opensoft\lua-5.4.2

![image-20230522140806222](img/Nginx学习笔记/image-20230522140806222.png)



![image-20230522140825930](img/Nginx学习笔记/image-20230522140825930.png)



![image-20230522140857243](img/Nginx学习笔记/image-20230522140857243.png)



![image-20230522140912142](img/Nginx学习笔记/image-20230522140912142.png)



从桌面访问

```sh
PS C:\Users\mao\Desktop> lua
Lua 5.4.2  Copyright (C) 1994-2020 Lua.org, PUC-Rio
> 99-5
94
> 
```







### 语法

#### HELLOWORLD

```lua
print("Hello World!!")
```



Lua交互式编程模式可以通过命令：

```sh
PS C:\Users\mao\Desktop> lua
Lua 5.4.2  Copyright (C) 1994-2020 Lua.org, PUC-Rio
> print("hello world")
hello world
>
```



脚本式：

编写hello.lua

```lua
print("Hello World!!")
```



运行命令：

```sh
lua hello.lua
```



```sh
PS C:\Users\mao\Desktop> cat .\hello.lua
print("Hello World!!")
PS C:\Users\mao\Desktop> lua hello.lua
Hello World!!
PS C:\Users\mao\Desktop>
```



如果想在交互式中运行脚本式的hello.lua中的内容，我们可以使用一个dofile函数：

```sh
dofile("./hello.lua")
```

```sh
> dofile("./hello.lua")
Hello World!!
>
```



在Lua语言中，连续语句之间的分隔符并不是必须的，也就是说后面可以不需要加分号







#### 注释

关于Lua的注释要分两种，第一种是单行注释，第二种是多行注释



单行注释的语法为：

```lua
--注释内容
```



多行注释的语法为:

```lua
--[[
	注释内容
	注释内容
--]]
```



如果想取消多行注释，只需要在第一个--之前在加一个-即可，如：

```lua
---[[
	注释内容
	注释内容
--]]
```





#### 标识符

Lua定义变量名以一个字母 A 到 Z 或 a 到 z 或下划线 _ 开头后加上0个或多个字母，下划线，数字（0到9）。这块建议大家最好不要使用下划线加大写字母的标识符，因为Lua的保留字也是这样定义的，容易发生冲突。注意Lua是区分大小写字母的





#### 关键字

|   and    | break |  do   |  else  |
| :------: | :---: | :---: | :----: |
|  elseif  |  end  | false |  for   |
| function |  if   |  in   | local  |
|   nil    |  not  |  or   | repeat |
|  return  | then  | true  | until  |
|  while   | goto  |       |        |





#### 运算符

Lua中支持的运算符有算术运算符、关系运算符、逻辑运算符、其他运算符



算术运算符：

```sh
+   加法
-	减法
*	乘法
/	除法
%	取余
^	乘幂
-	负号
```



```sh
> 9+7
16
> 257-32
225
> 6*50
300
> 60/5
12.0
> 31%5
1
> 2^7
128.0
> -54
-54
>
```



关系运算符：

```sh
==	等于
~=	不等于
>	大于
<	小于
>=	大于等于
<=	小于等于
```



```sh
> 1==1
true
> 2==2.4
false
> 2~=2
false
> 3.5~=4
true
> 3>2
true
> 4>6
false
> 2<1
false
> 3<5
true
> 2>=2
true
> 2>=4
false
> 4<=4
true
>
```



逻辑运算符：

```sh
and	逻辑与	 A and B     &&   
or	逻辑或	 A or B     ||
not	逻辑非  取反，如果为true,则返回false  !
```





```sh
..	连接两个字符串
#	一元预算法，返回字符串或表的长度
```



```sh
> "abc".."de"
abcde
> #"1234567"
7
>
```









#### 全局变量和局部变量

在Lua语言中，全局变量无须声明即可使用。在默认情况下，变量总是认为是全局的，如果未提前赋值，默认为nil

要想声明一个局部变量，需要使用local来声明



```sh
> a=12
> print(a)
12
> a=a+1
> print(a)
13
>
```

```sh
> local b=34
> print(b)
nil
> local b=34; print(b)
34
>
```







#### 数据类型

8个数据类型：

* nil(空，无效值)
* boolean(布尔，true/false)
* number(数值)
* string(字符串)
* function(函数)
* table（表）
* thread(线程)
* userdata（用户数据）



可以使用type函数测试给定变量或者的类型

```lua
> print(type(true))
boolean
> print(type("Hello world"))
string
>
```





#### nil

nil是一种只有一个nil值的类型，它的作用可以用来与其他所有值进行区分，也可以当想要移除一个变量时，只需要将该变量名赋值为nil，垃圾回收就会会释放该变量所占用的内存。



#### boolean

boolean类型具有两个值，true和false。boolean类型一般被用来做条件判断的真与假。在Lua语言中，只会将false和nil视为假，其他的都视为真，特别是在条件检测中0和空字符串都会认为是真



#### number

在Lua5.3版本开始，Lua语言为数值格式提供了两种选择:integer(整型)和float(双精度浮点型)

不管是整型还是双精度浮点型，使用type()函数来取其类型，都会返回的是number



#### string

Lua语言中的字符串即可以表示单个字符，也可以表示一整本书籍。在Lua语言中，操作100K或者1M个字母组成的字符串的程序很常见

可以使用单引号或双引号来声明字符串



```lua
> a="abcdefg"
> b='qwerty'
> print(a)
abcdefg
> print(b)
qwerty
> print(a..b)
abcdefgqwerty
> print(#a)
7
>
```



如果声明的字符串比较长或者有多行，则可以使用如下方式进行声明：

```sh
> c=[[
>> 121312
>> 1241241
>> 1343241
>> 34543645
>> 5675685685
>> 678567346252
>> ]]
> print(c)
121312
1241241
1343241
34543645
5675685685
678567346252

> print(#c)
56
>
```



#### table

table是Lua语言中最主要和强大的数据结构。使用表， Lua 语言可以以一种简单、统一且高效的方式表示数组、集合、记录和其他很多数据结构。 Lua语言中的表本质上是一种辅助数组。这种数组比Java中的数组更加灵活，可以使用数值做索引，也可以使用字符串或其他任意类型的值作索引(除nil外)

创建表的最简单方式：

```sh
> t={}
> print(type(t))
table
>
```



获取数组中的值，数组的下标默认是从1开始的

```sh
> t={"1","2","3"}
> print(t[1])
1
> print(t[2])
2
> print(t[3])
3
>
```



数组赋值：

```sh
> t2={}
> t2[1]="11"
> t2[2]="22"
> t2[3]="33"
> print(t2[2])
22
> print(t2[3])
33
>
```





也可以将索引更改为字符串来创建

```sh
> t3={}
> t3["a"]=3
> t3["b"]=4
> t3["c"]=5
>
```



如果想要获取这些数组中的值，可以使用下面的方式：

```sh
> t3={}
> t3["a"]=3
> t3["b"]=4
> t3["c"]=5
>
> print(t3["a"])
3
> print(t3["b"])
4
> print(t3["c"])
5
> print(t3.a)
3
> print(t3.b)
4
> print(t3.c)
5
>
```





#### function

在 Lua语言中，函数（ Function ）是对语句和表达式进行抽象的主要方式

定义函数的语法为：

```sh
function functionName(params)

end
```



```sh
> function f()
>> print("hello")
>> print(" - world")
>> end
>
> f()
hello
 - world
>
```



参数传递

```sh
> function f(a,b)
>> print(a.."---"..b)
>> end
> f()
stdin:2: attempt to concatenate a nil value (local 'b')
stack traceback:
        stdin:2: in function 'f'
        (...tail calls...)
        [C]: in ?
> f(1,2)
1---2
> f(1,2,3)
1---2
> f(1)
stdin:2: attempt to concatenate a nil value (local 'b')
stack traceback:
        stdin:2: in function 'f'
        (...tail calls...)
        [C]: in ?
>
```





可变长参数函数

```sh
> function f(...)
>> print(...)
>> end
>
> f(1,2,3)
1       2       3
> f(1,2)
1       2
> f(1,2,3,5,7,"hello")
1       2       3       5       7       hello
> f("21")
21
> f()

>
```





函数返回值可以有多个

```sh
> function f(a,b)
>> return b,a,(a+b);
>> end
>
> print(f(1,3))
3       1       4
> print(f(4,3))
3       4       7
> print(f(7,2))
2       7       9
> print(f(21,43))
43      21      64
>
```





#### thread

thread翻译过来是线程的意思，在Lua中，thread用来表示执行的独立线路，用来执行协同程序



#### userdata

userdata是一种用户自定义数据，用于表示一种由应用程序或C/C++语言库所创建的类型







#### Lua控制结构

Lua 语言提供了一组精简且常用的控制结构，包括用于条件执行的证 以及用于循环的 while、 repeat 和 for。 所有的控制结构语法上都有一个显式的终结符： end 用于终结 if、 for 及 while 结构， until 用于终结 repeat 结构。



#### if then elseif else

if语句先测试其条件，并根据条件是否满足执行相应的 then 部分或 else 部分。 else 部分 是可选的



```sh
> function f(a,b)
>> if a>3 then
>> print("a>3")
>> end
>> end
>
> f(1,2)
> f(4,2)
a>3
>
```

```sh
> function f(a,b)
>> if a>3 then
>> print("a>3")
>> else
>> print("a<=3")
>> end
>> end
>
> f(4,2)
a>3
> f(2,2)
a<=3
> f(3,2)
a<=3
>
```



如果要编写嵌套的 if 语句，可以使用 elseif。 它类似于在 else 后面紧跟一个if

```sh
> function show(age)
>> if age<=18 then
>>  return "青少年"
>> elseif age>18 and age<=45 then
>>  return "青年"
>> elseif age>45 and age<=60 then
>>  return "中年人"
>> elseif age>60 then
>>  return "老年人"
>> end
>> end
>
> show(14)
青少年
> show(19)
青年
> show(43)
青年
> show(47)
中年人
> show(76)
老年人
>
```





#### while循环

当条件为真时 while 循环会重复执行其循环体。 Lua 语言先测试 while 语句 的条件，若条件为假则循环结束；否则， Lua 会执行循环体并不断地重复这个过程

语法：

```sh
while 条件 do
  循环体
end
```



```sh
> function f(a)
>> while a>0 do
>> print(a)
>> a=a-1
>> end
>> end
>
> f(3)
3
2
1
> f(2)
2
1
> f(5)
5
4
3
2
1
>
```





#### repeat循环


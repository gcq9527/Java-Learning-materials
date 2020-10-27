---

typora-root-url: image
typora-copy-images-to: image
---

# Nginx



内容介绍

1、nginx 基本概念

1. nginx 是什么，做什么事情
2. 反向代理
3. 负载均衡
4. 动静分离

2、nginx 安装，常用命令和配置文件

1. 在 linux 系统中安装nginx
2. nginx 常用命令
3. nginx 配置文件

3、Nginx 的常用命令和配置文件

4、Nginx 配置示例 1 - 反向代理

5、Nginx 配置实例 2 - 负载均衡

6、Nginx 配置实例 3 - 动静分离

7、Nginx 的高可用集群

1. nginx 配置主从模式
2. nginx 配置双主模式

## 第 1 章 Nginx 简介



### 1、 什么是 Nginx

Nginx是高性能的 HTTP 和反向代理的服务器，处理高并发能力是十分强大的，能经受高负载的考验,有报告表明能支持高达 50,000 个并发连接数。



### 2、 反向代理

> 2.1 正向代理

Nginx不仅可以做反向代理，实现负载均衡。还能用作正向代理来进行上网等功能。正向代理：如果把局域网外的Internet想象成一个巨大的资源库，则局域网中的客户端要访问Internet，则需要通过代理服务器来访问，这种代理服务就称为正向代理

![image-20201012090431772](D:\java\脑图\Nginx\image\image-20201012090431772.png)

> 2.1 反向代理

反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。

![image-20201012091133753](D:\java\脑图\Nginx\image\image-20201012091133753.png)

> 3、负载均衡

客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服务器处理完毕后，再将结果返回给客户端。

这种架构模式对于早期的系统相对单一，并发请求相对较少的情况下是比较适合的，成本也低。但是随着信息数量的不断增长，访问量和数据量的飞速增长，以及系统业务的复杂度增加，这种架构会造成服务器相应客户端的请求日益缓慢，并发量特别大的时候，还容易造成服务器直接崩溃。很明显这是由于服务器性能的瓶颈造成的问题，那么如何解决这种情况呢？

我们首先想到的可能是升级服务器的配置，比如提高CPU执行频率，加大内存等提高机器的物理性能来解决此问题，但是我们知道摩尔定律的日益失效，硬件的性能提升已经不能满足日益提升的需求了。最明显的一个例子，天猫双十一当天，某个热销商品的瞬时访问量是极其庞大的，那么类似上面的系统架构，将机器都增加到现有的顶级物理配置，都是不能够满足需求的。那么怎么办呢？

上面的分析我们去掉了增加服务器物理配置来解决问题的办法，也就是说纵向解决问题的办法行不通了，那么横向增加服务器的数量呢？这时候集群的概念产生了，单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上**，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡**



![image-20201012091656779](D:\java\脑图\Nginx\image\image-20201012091656779.png)



> 4、动静分离

为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速度。降低原来单个服务器的压力

![image-20201012093751081](D:\java\脑图\Nginx\image\image-20201012093751081.png)

# 第 2 章 Nginx 安装

### 2.1 进入 nginx 官网 下载

http://nginx.org/en/download.html

![image-20201012102144709](D:\java\脑图\Nginx\image\image-20201012102144709.png)

![image-20201012102154688](D:\java\脑图\Nginx\image\image-20201012102154688.png)



### 2.2 安装 nginx

> 第一步: 安装 pcre

wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz

解压文件  tar -zxvf pcre-8.37.tar.gz

进入目录 执行 ./configure 完成后 

在执行 make && make install

> 第二步：安装 openssl

同上

> 第三步： 安装 zlib

一键安装上面三个依赖

yum -y install make zlib zlib-devel gcc-c++ libtool    openssl openssl-devel

第四步：安装 nginx

1、解压 nginx 包

2、进入解压目录，执行 ./configure

3、make && make install

此时查看 /usr/local/ 下就会发现 nginx目录

![image-20201012102554035](D:\java\脑图\Nginx\image\image-20201012102554035.png)

查看开放的端口号

firewall-cmd --list-all

设置开放的端口号

firewall-cmd --add-service=http –permanentsudo

 firewall-cmd --add-port=80/tcp --permanent

重启防火墙

firewall-cmd –reload

关闭防火墙

 systemctl stop firewalld.service



# 第 3 章 Nginx 操作常用命令和配置文件

### 3.1 nginx常用命令

> 1、启动命令

在/usr/local/nginx/sbin 目录下执行 ./nginx

> 2、关闭命令

在/usr/local/nginx/sbin 目录下执行 ./nginx -s stop

> 3、重新加载命令

在/usr/local/nginx/sbin目录下执行./nginx -s reload



### 3.2 nginx 配置文件 

nginx配置文件位置 /usr/local/nginx/conf

![image-20201012103014991](D:\java\脑图\Nginx\image\image-20201012103014991.png)

我们能将 nginx.conf 文件分为 三 部分

> 第一部分：全局块

从配置文件开始到events 块之间的内容，主要会设置一些影响nginx 服务器整体运行的配置指令，主要包括配置运行Nginx 服务器的用户（组）、允许生成的worker process 数，进程PID 存放路径、日志存放路径和类型以及配置文件的引入等。比如上面第一行配置的

![image-20201012105805808](D:\java\脑图\Nginx\image\image-20201012105805808.png)

这是nginx 服务器并发处理服务的关键配置，worker_processes 值越大，可以支持的并发处理量就越多，但是会受到硬件、软件等设备的制约

> 第二部分：events 块

![image-20201012105928481](D:\java\脑图\Nginx\image\image-20201012105928481.png)

events 块涉及的指令主要影响Nginx 服务器与用户的网络连接，常用的设置包括是否开启对多work process 下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个word process 可以同时支持的最大连接数等。

上述例子就表示每个work process 支持的最大连接数为1024

这部分的配置对Nginx 的性能影响较大，在实际中应该灵活配置



> 第 三 部分 ：http块

![image-20201012110023268](D:\java\脑图\Nginx\image\image-20201012110023268.png)

这是 Nginx 服务器中最频繁的部分，代理，缓存，日志，定义的功能绝大多数功能和第三方模块都在这里

需要注意的是：http 块 可以包含 **http全局块，server块**

**1、http 全局块**

http全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数上限等

**2、server 块**

这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。

每个http 块可以包括多个server 块，而每个server 块就相当于一个虚拟主机。

而每个server 块也分为全局server 块，以及可以同时包含多个locaton 块。

1、全局server 块

​	最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。

2、location 块

​	一个server 块可以配置多个location 块。

这块的主要作用是基于Nginx服务器接收到的请求字符串（例如server_name/uri-string），对虚拟主机名称（也可以是IP别名）之外的字符串（例如前面的/uri-string）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。



# 第 4 章 Nginx 配置实例 - 反向代理



### 4.1 实现效果

​		打开浏览器，在浏览器地址栏中输入地址 www.123.com，跳转到  linux系统 tomcat主页面中

### 4.2 准备工作

> ​	1、启动一个tomcat，浏览器输入127.0.0.1：8080 出现如下页面

![image-20201012114258625](/image-20201012114258625.png)

> ​	2、修改本地 host 文件，将 www.123.com 映射到 虚拟机上的Centos的ip地址上

![image-20201012114308609](/image-20201012114308609.png)

配置完成之后，我们便可以通过www.123.com:8080 访问到第一步出现的Tomcat初始界面。那么如何只需要输入www.123.com 便可以跳转到Tomcat初始界面呢？便用到nginx的反向代理.

> 3、在 nginx.conf 配置文件中 增加如下配置

![image-20201012114424672](/image-20201012114424672.png)



如上配置，我们监听80端口，访问域名为www.123.com，不加端口号时默认为80端口，故访问该域名时会跳转到127.0.0.1:8080路径上。在浏览器端输入www.123.com 结果如下



![image-20201012114445172](/image-20201012114445172.png)

 

反向代理实例二

实现效果：使用nginx反向代理，根据访问的路径跳转到不同端口的服务中

nginx监听端口为9001，

访问http://127.0.0.1:9001/edu/直接跳转到127.0.0.1:8081

访问http://127.0.0.1:9001/vod/直接跳转到127.0.0.1:8082

 实验代码

> 1、准备两个 tomcat 一个 8080 端口 一个8081端口，并准备好测试的页面

> 2、修改 nginx.conf 在 http块中 添加 server

![image-20201012114659172](/image-20201012114659172.png)



localtion 指令说明

该指令用于匹配 URL

语法如下

```
location [ = | ` | `* | ^`] uri {

}
```

1、= ：用于不含正则表达式的uri 前，要求请求字符串与uri 严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求。

2、~：用于表示uri 包含正则表达式，并且区分大小写。

3、~*：用于表示uri 包含正则表达式，并且不区分大小写。*

*4、^~：用于不含正则表达式的uri 前，要求Nginx 服务器找到标识uri 和请求字符串匹配度最高的location 后，立即使用此location 处理请求，而不再使用location 块中的正则uri 和请求字符串做匹配。*

**注意：如果uri 包含正则表达式，则必须要有~ 或者~ 标识**



#  第 5 章 nginx 配置示例 - 负载均衡

实现效果：配置负载均衡

### 5.1 实验代码

1、首先准备两个同时启动的 Tomcat

2、在 nginx.conf 中进行配置

![image-20201012150441604](/image-20201012150441604.png)

随着互联网信息的爆炸性增长，负载均衡（load balance）已经不再是一个很陌生的话题，顾名思义，负载均衡即是将负载分摊到不同的服务单元，既保证服务的可用性，又保证响应足够快，给用户很好的体验。快速增长的访问量和数据流量催生了各式各样的负载均衡产品，很多专业的负载均衡硬件提供了很好的功能，但却价格不菲，这使得负载均衡软件大受欢迎，nginx就是其中的一个，在linux下有Nginx、LVS、Haproxy等等服务可以提供负载均衡服务，而且Nginx提供了几种分配方式(策略)：



> 1、轮询 (默认)

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down 掉，能自动剔除

> 2、weight

weight 代表权重，默认为1，权重越高被分配的客户端越多

指定轮询几率，weight 和访问比率成正比。用于后端服务器性能不均的情况下，例如

![image-20201012150914312](/image-20201012150914312.png)

> 3、ip_hash

每个请求按访问呢 IP 的 hash 结果分配 这样每隔访客固定访问一个后端服务器，可以解决 session问题 例如：

![image-20201012151617614](/image-20201012151617614.png)

> 4、fair (第三方)

按后端服务器的响应时间来分配请求，响应时间短的优先分配

![image-20201012151647597](/image-20201012151647597.png)





# 第 6 章 Nginx 配置实例 - 动静分离

Nginx 动静分离简单来说就是把动态跟静态请求分开，不能理解成只是单纯的把动态页面和静态页面物理分离。严格意义上说应该是动态请求跟静态请求分开，可以理解成使用Nginx 处理静态页面，Tomcat处理动态页面。动静分离从目前实现角度来讲大致分为两种，

一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上，也是目前主流推崇的方案；

另外一种方法就是动态跟静态文件混合在一起发布，通过nginx 来分开。

通过location 指定不同的后缀名实现不同的请求转发。通过 expires 参数设置，可以使浏览器缓存过期时间，减少与服务器之前的请求和流量。具体Expires 定义：是给一个资源设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器自身确认是否过期即可，所以不会产生额外的流量。此种方法非常适合不经常变动的资源。（如果经常更新的文件，不建议使用Expires 来缓存），我这里设置 3d，表示在这 3 天之内访问这个URL，发送一个请求，比对服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码304，如果有修改，则直接从服务器重新下载，返回状态码200





![image-20201012152126806](/image-20201012152126806.png)



### 6.1 实验代码

1、项目资源准备

2、进行 nginx 配置

nginx.conf 配置文件

准备静态资源

![image-20201012154039310](/image-20201012154039310.png)



添加监听端口，访问名字

重点是添加 location

最后检查Nginx 配置是否正确即可，然后测试动静分离是否成功，之需要删除后端tomcat 服务器上的某个静态文件，查看是否能访问，如果可以访问说明静态资源nginx 直接返回了，不走后端tomcat 服务器



# 第 7 章 Nginx 原理与优化参数设置

![image-20201012163929754](/image-20201012163929754.png)



![image-20201012164152886](/image-20201012164152886.png)

> master-workers的机制的好处首先

对于每个worker进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销，同时在编程以及问题查找时，也会方便很多。其次，采用独立的进程，可以让互相之间不会影响，一个进程退出后，其它进程还在工作，服务不会中断，master进程则很快启动新的worker进程。当然，worker进程的异常退出，肯定是程序有bug了，异常退出，会导致当前worker上的所有请求失败，不过不会影响到所有请求，所以降低了风险



> 需要设置多少个worker

Nginx 同 redis 类似都采用了 io 多路复用机制，每个worker都是一个独立的进程，但每个进程里只有一个主线程，通过异步非阻塞的方式来处理请求，即使是千上万个请求也不在话下。每个 worker 的线程可以把一个 cpu 的性能发挥到极致。所以worker数和服务器的cpu数相等是最为适宜的。设少了会浪费cpu，设多了会造成cpu频繁切换上下文带来的损耗

\#设置worker数量。

worker_processes 4

#work绑定cpu(4 work绑定4cpu)。

worker_cpu_affinity 0001 0010 0100 1000

#work绑定cpu (4 work绑定8cpu中的4个) 。

worker_cpu_affinity 0000001 00000010 00000100 0000100



> 连接数 worker_connection

这个值是表示每个worker进程所能建立连接的最大值，所以，一个nginx能建立的最大连接数，应该是worker_connections  *  worker_processes。当然，这里说的是最大连接数，对于HTTP请 求 本 地 资 源 来 说 ， 能 够 支 持 的 最 大 并 发 数 量 是worker_connections   * worker_processes，如果是支持http1.1的浏览器每次访问要占两个连接，**所以普通的静态访问最大并发数是：worker_connections  *  worker_processes  / 2**，**而如果是HTTP作为反向代理来说，最大并发数量应该是worker_connections * worker_processes/4**。因为作为反向代理服务器，每个并发会建立与客户端的连接和与后端服务的连接，会占用两个连接。



![image-20201012165622020](/image-20201012165622020.png)



























# 第 8 章 Nginx搭建高可用集群

### 8.1 Keepalived + Nginx 高可用集群 ( 主从模式 )

![image-20201012162454102](/image-20201012162454102.png)

> 1、配置高可用准备工作

1、需要两台服务器 192.168.17.129 和192.168.17.131

2、在两台服务器 安装 nginx

3、在两台服务器安装 keepalived



> 2、在两台服务中 安装keepalived

1、使用 yum 命令安装

yum install keepalived -y

2、安装后 在 etc里面生成目录 keepalive 有文件 keepalivec.conf配置文件



> 3、完成高可用配置 ( 主从配置 )

1、修改/etc/keepalived/keepalivec.conf配置文件

```bash
global_defs {
	notification_email {
	acassen@firewall.loc
	failover@firewall.loc
	sysadmin@firewall.loc
	}
	notification_email_from Alexandre.Cassen@firewall.loc
	smtp_server 192.168.17.129
	smtp_connect_timeout 30
	router_id LVS_DEVEL
	}
	
vrrp_script chk_http_port {
    script "/usr/local/src/nginx_check.sh"
    interval 2      #（检测脚本执行的间隔）
    weight 2
}


vrrp_instance VI_1 {
state BACKUP   # 备份服务器上将MASTER 改为BACKUP  
interface ens33  //网卡
virtual_router_id 51   # 主、备机的virtual_router_id必须相同
priority 100     # 主、备机取不同的优先级，主机值较大，备份机值较小
advert_int 1
authentication {
    auth_type PASS
    auth_pass 1111
}
virtual_ipaddress {
192.168.17.50 // VRRP H虚拟地址
	}
}


```

2、在/usr/local/src 添加检测脚本

```bash
#!/bin/bash
A=`ps -C nginx –no-header |wc -l`
if [ $A -eq 0 ];then
    /usr/local/nginx/sbin/nginx
    sleep 2
    if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
        killall keepalived
    fi
fi
```

3、把两台服务器上nginx和keepalived启动

启动nginx：./nginx

启动keepalived：systemctl start keepalived.service

> 5 最终测试



1、在浏览器地址栏输入虚拟ip地址192.168.17.50 

![image-20201012163807688](/image-20201012163807688.png)

![image-20201012163817357](/image-20201012163817357.png)

（2）把主服务器（192.168.17.129）nginx和keepalived停止，再输入192.168.17.50

![image-20201012163836255](/image-20201012163836255.png)

![image-20201012163840741](/image-20201012163840741.png)



![image-20201012162304285](/image-20201012162304285.png)
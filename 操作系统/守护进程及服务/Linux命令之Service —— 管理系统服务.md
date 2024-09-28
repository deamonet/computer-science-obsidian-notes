[

![头像](media/头像.jpg)

**浩Coding**

562



](https://segmentfault.com/u/haocoding)

[

发布于

2020-03-02

](https://segmentfault.com/a/1190000021888011/revision)

**service命令用于对系统服务进行管理**，比如**启动（start）**、**停止（stop）**、**重启（restart）**、**查看状态（status）**等。相关的命令还包括chkconfig、ntsysv等，chkconfig用于查看、设置服务的运行级别，ntsysv用于直观方便的设置各个服务是否自动启动。**service命令本身是一个shell脚本**，它在/etc/init.d/目录查找指定的服务脚本，然后**调用该服务脚本来完成任务**。这个命令不是在所有的linux发行版本中都有。主要是在redhat、fedora、mandriva和centos中。

**常用的service命令：**

**重启**MySQL：`service mysqld` `restart`

`` **启动**：`service mysqld` `start` ``

``` ``**停止**：`service mysqld` `stop` `` ```

**查看状态**：`service mysqld` `status`

`` **查看**所有服务的状态：`service` `--status-all` ``

**重载**配置：`service mysqld` `reload``【不同于重启，restart是重启了整个mysql服务，而reload则是重新加载了my.conf配置，也并不是每一个应用程序都有所谓的 reload 和 restart】`

![](media/1460000021888014.png)

有兴趣的童鞋可以看下service脚本的源码：

tail /sbin/service

![](media/1460000021888015.png)

其实这个脚本service主要做了如下两点：

1.**初始化执行环境变量PATH和TERM**

PATH=/sbin:/usr/sbin:/bin:/usr/bin

TERM，为显示外设的值，一般为xterm

2.**调用/etc/init.d/文件夹下的相应脚本**，脚本的参数为service命令第二个及之后的参数

从下图可以看到mysqld为/etc/init.d/下面的一个可执行文件：

![](media/1460000021888017.png)

以service mysqld restart命令为例，其中restart为参数，将传递给mysqld脚本，这个命令在service执行到后面最终调用的是：

env -i PATH="$PATH" TERM="$TERM" "${SERVICEDIR}/${SERVICE}" ${OPTIONS}

 就相当于执行了：**/etc/init.d/mysqld restart**  
**拓展知识 -- 自定义Linux Service**

有兴趣的童鞋可以跟我一起写个service服务脚本玩玩，我们可以先看下mysqld服务的内容，仿照着写一写：

tail /etc/init.d/mysqld
# mysqld  This shell script takes care of starting and stopping
# chkconfig: 345 64 36
# description:  MySQL database server.
# Source function library.
. /etc/rc.d/init.d/functions
--- start、stop等函数 ---
restart(){
    stop
    start
}

我们不需要全部看懂，我们自己写个service服务脚本的目的是为了更好的了解service相关知识，所以我们看懂下面几个关键地方就行了：

chkconfig: 345  64  36：用chkconfig命令管理我们的新服务脚本 
. /etc/rc.d/init.d/functions ：functions这个脚本是给/etc/init.d里边的文件使用的（可理解为全局文件）。
start、stop等函数的定义和调用

源码如下：

#!/bin/bash
# source function library
. /etc/rc.d/init.d/functions
# chkconfig: 345 85 15
# description: This is a haoCoding Test Service.
usage() {
  echo " usage:$0 {start|stop|restart} "
}
​
start() {
  echo "haoCoding Service Started!"
}
​
stop() {
  echo "haoCoding Service Stopped!"
}
​
restart() {
  stop
  start
}
​
#main function
case $1 in
  start)
     start
     ;;
  stop)
     stop
     ;;
  restart)
     restart
     ;;
  *)
     usage
     ;;
esac

记得chmod a+x haoCodingService给予权限。

好的，我们现在测试下，输入**service haoCodingService start**命令试试【注意：我的service脚本文件名称是**haoCodingService**，所以服务名就是文件名】

![](media/1460000021888016.png)

非常成功，Yes！！！

Github源码下载地址：

**[https://github.com/jiahaoit/h...](https://link.segmentfault.com/?enc=sWyg0FHmoezZgIR0%2Bs2kFw%3D%3D.%2BQBHhzxDxV0LE%2BfVe3bod9cWRoLibjnWpKAvL0BCrM0uM%2BvVSQcRMieO4HBOANMW)**  
**最后附上超实用的Linux系统信息查看命令：**

**系统**

# uname -a # 查看内核/操作系统/CPU信息  
# head -n 1 /etc/issue # 查看操作系统版本  
# cat /proc/cpuinfo # 查看CPU信息  
# hostname # 查看计算机名  
# lspci -tv # 列出所有PCI设备  
# lsusb -tv # 列出所有USB设备  
# lsmod # 列出加载的内核模块  
# env # 查看环境变量

**资源**

# free -m # 查看内存使用量和交换区使用量  
# df -h # 查看各分区使用情况  
# du -sh <目录名> # 查看指定目录的大小  
# grep MemTotal /proc/meminfo # 查看内存总量  
# grep MemFree /proc/meminfo # 查看空闲内存量  
# uptime # 查看系统运行时间、用户数、负载  
# cat /proc/loadavg # 查看系统负载

**磁盘和分区**

# mount | column -t # 查看挂接的分区状态  
# fdisk -l # 查看所有分区  
# swapon -s # 查看所有交换分区  
# hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备)  
# dmesg | grep IDE # 查看启动时IDE设备检测状况

**网络**

# ifconfig # 查看所有网络接口的属性  
# iptables -L # 查看防火墙设置  
# route -n # 查看路由表  
# netstat -lntp # 查看所有监听端口  
# netstat -antp # 查看所有已经建立的连接  
# netstat -s # 查看网络统计信息

**进程**

# ps -ef # 查看所有进程  
# top # 实时显示进程状态

**用户**

# w # 查看活动用户  
# id <用户名> # 查看指定用户信息  
# last # 查看用户登录日志  
# cut -d: -f1 /etc/passwd # 查看系统所有用户  
# cut -d: -f1 /etc/group # 查看系统所有组  
# crontab -l # 查看当前用户的计划任务

**服务**

# chkconfig --list # 列出所有系统服务  
# chkconfig --list | grep on # 列出所有启动的系统服务

**程序**

# rpm -qa # 查看所有安装的软件包

参考文章：

自定义Linux Service：

[https://www.iteye.com/blog/mo...](https://link.segmentfault.com/?enc=R7nsr7IRNsRYYOGk8Ei8Rg%3D%3D.lrtiDyDcvo1sgjS0IQ7UqC%2B6Ws5ru2s8sr0RJy%2F2tbpd91rm7YLbg%2BazDOroH%2Fqk)

service命令：

[https://www.cnblogs.com/wuhen...](https://link.segmentfault.com/?enc=PtzF6MzBZOzYMdntF4pOFQ%3D%3D.0ulOB28QLHM85McwQA%2F17pRDlFmld9JzFdCKyhWWKf0xGCL7u9bGSdA%2F5gWNT6HPJIOuYP2i50y2FXZM9r5nMg%3D%3D)

linux service命令解析（重要）：

[https://www.cnblogs.com/qlqwj...](https://link.segmentfault.com/?enc=taqr2dwPKYJ2LJCW%2Bphmtw%3D%3D.3BkATnu5II4xSKlwEY1oSUGze3sEA78T1vDx1qtxgg24IceSBSLcoyfjGIY1ai70)

[linux](https://segmentfault.com/t/linux)[shell](https://segmentfault.com/t/shell)[linux运维](https://segmentfault.com/t/linux%E8%BF%90%E7%BB%B4)

阅读 8k[发布于 2020-03-02](https://segmentfault.com/a/1190000021888011/revision)
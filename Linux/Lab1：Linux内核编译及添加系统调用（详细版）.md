[![返回主页](media/返回主页.gif)](https://www.cnblogs.com/tsruixi/)

# [睿晞](https://www.cnblogs.com/tsruixi/)

-   [博客园](https://www.cnblogs.com/)
-   [首页](https://www.cnblogs.com/tsruixi/)
-   [新随笔](https://i.cnblogs.com/EditPosts.aspx?opt=1)
-   [联系](https://msg.cnblogs.com/send/%E7%9D%BF%E6%99%9E)
-   [管理](https://i.cnblogs.com/)
-   订阅 [![订阅](media/订阅.gif)](https://www.cnblogs.com/tsruixi/rss/)

随笔- 283  文章- 0  评论- 13  阅读- 22万 

# [Lab1：Linux内核编译及添加系统调用（详细版）](https://www.cnblogs.com/tsruixi/p/10777242.html)

# 实验一：Linux内核编译及添加系统调用（HDU）

花了一上午的时间来写这个，良心制作，发现自己刚学的时候没有找到很详细的，就是泛泛的说了下细节地方也没有，于是自己写了这个，有点长，如果你认真的看完了，也应该是懂了。

## 一、前期准备工作

1.  需要准备虚拟机上安装Ubuntu，笔者安装的是**Ubuntu18.04**，安装的教程自行百度解决，教程很多。**有几点需要提一下，就是内存分配至少60G，核分配4个最好，为了在编译的时候别崩溃。**  
    建议去熟悉一下Linux下面的文件目录结构，根目录下每个目录一般会存放什么样的文件![](media/1504548-20190426234341124-1632240340.png)  
    ，然后常见命令操作也要熟悉一下。
2.  下载Linux内核[地址](https://www.kernel.org/),自行选择版本，建议选择4.xx版本，因为版本高出错的概率也大。  
    下载好了之后，会放在自己的Ubuntu中的Downloads目录下，同时是一个压缩文件，到时候需要解压到放内核目录文件下。首先进入到该Downloads文件目录下，查看是否下载好了。

```shell
$cd ~/Downloads
$ls
linux-4.19.25.tar.xz
```

之后开始解压上面的那个压缩文件到存放内核的地方，就是Linux系统的**/usr/src**目录下，此目录用来存放内核源码的。从上图也可以了解到。

```shell
cd ~/Downloads
tar xvJf linux-4.19.25.tar.xz -C /usr/src
```

进入**/usr/src**目录查看是否有，如果有就可以开始后续工作了。

## 二、实验要求和内容

#### 1. 内容要求：

（1）添加一个系统调用，实现对指定进程的nice值的修改或读取功能，并返回进程最新的nice值及优先级。建议调用原型是int mysetniec(pid_t pid, int flag, int nicevalue, void_user* prio, void_user* nice);  
参数含义：  
pid：进程ID  
flag：若为0，则表示读取nice的值；若为1，则表示修改nice的值。  
nicevalue：为指定的进程设置新的nice。  
prio，nice：指向进程的优先级和nice值。  
返回值：系统调用成功时返回0；失败时返回错误码EFAULT。  
（2）写一个简单的应用程序测试（1）中添加的系统调用。  
（3）若系统调用了Linux的内核函数，要求深入阅读相关的源码。

#### 2. Linux系统调用的基本概念

实质是指调用内核函数，于内核态中运行，Linux中的用户通过执行一条访管指令“int $0x80”来调用系统调用，该指令会产生一个访管中断，从而让系统暂停当前的进程执行，而转去执行系统调用处理程序。通过用户态传入的系统调用号从系统调用表中找到相应的服务例程的入口并执行，完成后返回。  
（1）系统调用号与系统调用表：Linux内核中设置了一张系统调用表，用于关联系统调用号及其相对应的服务例程入口地址，定义在**./arch/x86/entry/syscalls/syscall_64.tbl**文件中，每个系统调用占一个表项，一旦分配好就不可以有任何变更。  
（2）系统调用服务例程:每个系统调用都对应一个内核服务例程来实现系统调用的功能，其命名的格式都是以**"sys_**开头。其代码通常放在**./kernel/sys.c**中，服务例程的原型声明则是放在**./include/linux/syscall.h**中。如sys_open,通常格式是asmlinkage long sys_open(int flag......)。其中的amslinkage是一个必需的限定词，用于通知编译器从堆栈中提取函数的参数，而不是从寄存器中。  
在sys.c中编程时，格式是`SYSCALL_DEFINE5(mysetnice, pid_t, pid, int, flag, int, nicevalue, void __user *, prio, void __user *, nice)` **N=5**代表参数的个数。  
（3）系统调用参数传递：在X86中，Linux通过6个寄存器来传入参数，其中一个eax是传递系统调用号，后面的5个传递参数。  
（4）系统调用参数验证

## 三、开始实验

#### 1. 切换到root权限下，防止权限不够，导致出错。

```yaml
$ sudo passwd root
[sudo] password for leslie: 
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
$ su root
Password:
```

(1)首先，你安装Linux系统时，它会让你设置一个你的用户名和用户密码，在这里我设置的用户名是leslie，即绿色字体leslie@tp50的前半部分，后半部分tp50是我的主机名。  
sudo放在命令首，意思是当前指令以管理员权限运行。  
(2)passwd是一条命令，用来修改用户密码，参数root是超级用户名，拥有系统最高的权限。passwd root的意思是修改超级用户的密码，在创建Ubuntu时，默认超级用户是没有密码的（也可能是一个随机数之类的我不记得了），用这条命令重新设定一个密码。  
(3)su是一条命令，用来切换当前用户，在第一章你会认识到，Linux是一个多用户多任务的操作系统。参数root是用户名，指示切换到的用户，在这里su root意为切换到root用户，当参数缺省时，默认切换到超级用户。  
(4)你会发现，它在提示输入密码时，虽然键盘已经输入了密码，但是终端没有任何响应，不要担心，这正是Unix和Linux的特点，为了确保安全，在输入密码时不显示输入的内容，在输入密码后，直接按下回车就好了。

#### 2. 分配系统调用号，修改系统调用表

**（1）查看系统调用表，并修改**

```bash
gedit /usr/src/linux-4.19.25/arch/x86/entry/syscalls/syscall_64.tbl
```

你只需要将**linux-4.19.25**换成你自己下载好的版本即可。  
你会看见这个格式  
![](media/1504548-20190427101023085-200040378.png)  
应用二进制接口分为三种：64、x32和common，即三种不同的调用约定，这里不需考虑太多，三种任意选择一种即可，按照上述格式编写新的系统调用表表项如下：

```undefined
335	64	first_compile		__x64_sys_first_compile
```

![](media/1504548-20190427101509031-932849794.png)  
**（2）声明系统调用服务例程**  
查看系统调用头文件

```bash
gedit  /usr/src/linux-4.19.25/include/linux/syscalls.h
```

同样将linux-4.19.25换成你自己的版本即可。  
![](media/1504548-20190427102430767-1431291284.png)  
**（3）实现自己的系统调用服务例程**  
首先进入解压后的文件目录，就是开始解压放入的目录，如下图：

```shell
$cd /usr/src/linux-4.19.34(换成自己的版本即可)/kernel
vim sys.c
```

![](media/1504548-20190427103036649-1684243322.png)  
函数说明：  
这一步与上一步的关系，就是C语言中头文件与实现文件的关系，上一步我们对函数进行了声明，这里给函数一个具体的实现。

首先要明确，我们要实现一个什么样的功能，根据内容要求可知，这个系统调用需要具备对指定进程的nice值的修改及读取的功能，同时返回进程最新的nice值及优先级prio。

把功能分拆成一个一个小块，我们需要做到的有以下几点：

根据进程号pid找到相应的进程控制块PCB（因为进程控制块中记录了用于描述进程情况及控制进程运行所需要的全部信息，nice值和优先级正是其中的一部分）；  
根据PCB读取它的nice值和优先级prio；  
根据PCB对相应进程的nice值进行修改；  
将得到的nice值和优先级prio进行返回。

```csharp

SYSCALL_DEFINE5(first_compile, pid_t, pid, int, flag, int, nicevalue, void __user *, prio, void __user *, nice)
{
        int cur_prio, cur_nice;
        struct pid *ppid;
        struct task_struct *pcb;

        ppid = find_get_pid(pid);

        pcb = pid_task(ppid, PIDTYPE_PID);

        if (flag == 1)
        {
                set_user_nice(pcb, nicevalue);
        }
        else if (flag != 0)
        {
                return EFAULT;
        }

        cur_prio = task_prio(pcb);
        cur_nice = task_nice(pcb);

        copy_to_user(prio, &cur_prio, sizeof(cur_prio));
        copy_to_user(nice, &cur_nice, sizeof(cur_nice));

        return 0;
}
```

**（4）开始编译内核**  
首先，用下面这条命令查漏补缺，很有用处，用来它，我编译是一次通过的，没有遇见什么其他麻烦。

```mipsasm
sudo apt-get install libncurses5-dev make openssl libssl-dev bison flex
```

然后定位到，源码在的目录，也就是解压后放的目录。  
![](media/1504548-20190427105224097-1839783277.png)  
然后运行命令

```go
make menuconfig
```

![](media/1504548-20190427105346343-1318889937.png)  
然后会出现以下界面，根据下面的图片所指的按钮来，通过左右键来确定光标停在选的按钮上，enter键是确定键  
![](media/1504548-20190427105453443-1315339025.png)  
![](media/1504548-20190427105809226-509098033.png)  
![](media/1504548-20190427105832108-130457583.png)  
![](media/1504548-20190427105849099-2067444916.png)  
**准备工作做好后，开始编译，耗时最长**

```go
sudo make -j4 2> error.log
```

-j4表示使用四线程进行编译，这个过程大概持续一个小时，后面的重定向将错误信息输出到了error.log这个文件里面，方便我们之后进行错误排查，不至于一两个小时坐在电脑面前盯着信息输出生怕出现一个错误而自己错过了，之后修改只能靠两眼排查，相信我，那不是一种好的体验。  
开始等待吧，结束后就可以安装内核了。  
**（5）安装内核**  
此时还是在你原来的目录路径下  
安装模块：

```go
sudo make modules_install
```

使用这一行命令进行模块的安装，模块的安装持续时间大概在十几分钟左右，视你分配的资源多寡这个时间会适当地增加或减少。  
结束后  
安装内核：

```go
sudo make install
```

使用这一行命令进行内核的安装，内核的安装持续时间大概是几分钟，视你分配的资源多寡这个时间会适当地增加或减少。

这一步完成且没有任何错误后，恭喜你，你已经完成了绝大多数的工作了，剩下的都是一些简单且容易调试的内容，重启你的电脑/虚拟机。  
这个时候可以查看你的**/lib/module**目录下有无安装好的内核了。  
![](media/1504548-20190427112217977-133037903.png)  
**（6）重启系统**  
查看内核版本的命令，如下，你可以看自己现在的版本是否是新安装的内核

```bash
uname -a
```

![](media/1504548-20190427115545853-902372724.png)

之后可能会弹出这个界面，选择你自己刚刚编译好的内核即可  
![](media/1504548-20190427112453982-12448049.png)  
**（7）测试**  
自己选择一个文件夹，存放自己的测试代码,创建**.c**文件

```x86asm
vim test.c
```

![](media/1504548-20190427113305414-1712321920.png)

```cpp
#include <unistd.h>
#include <sys/syscall.h>
#include <stdio.h>
#define _SYSCALL_MYSETNICE_ 335
#define EFALUT 14

int main()
{
    int pid, flag, nicevalue;
    int prev_prio, prev_nice, cur_prio, cur_nice;
    int result;

    printf("Please input variable(pid, flag, nicevalue): ");
    scanf("%d%d%d", &pid, &flag, &nicevalue);

    result = syscall(_SYSCALL_MYSETNICE_, pid, 0, nicevalue, &prev_prio,
                     &prev_nice);
    if (result == EFALUT)
    {
        printf("ERROR!");
        return 1;
    }

    if (flag == 1)
    {
        syscall(_SYSCALL_MYSETNICE_, pid, 1, nicevalue, &cur_prio, &cur_nice);
        printf("Original priority is: [%d], original nice is [%d]\n", prev_prio,
               prev_nice);
        printf("Current priority is : [%d], current nice is [%d]\n", cur_prio,
               cur_nice);
    }
    else if (flag == 0)
    {
        printf("Current priority is : [%d], current nice is [%d]\n", prev_prio,
               prev_nice);
    }

    return 0;
}
```

之后使用gcc进行编译，根据要求输入对应的值。  
结束了。到这一步，你就熟悉了过程了，对于编译和添加内核有个基本的了解了。

## 最后附上需要用到的内核源码截图。

[Linux源码地址](https://elixir.bootlin.com)  
第一张图和第二张图是一个**函数set_user_nice()**  
![](media/1504548-20190427113808427-2125077605.png)  
![](media/1504548-20190427113848297-1646811745.png)

**task_on_rq_quened()**

![](media/1504548-20190427114005397-699535426.png)

---

![](media/1504548-20190427114211257-1187533098.png)

---

**task_current()**  
![](media/1504548-20190427114250622-1399936544.png)

![](media/1504548-20190427114312673-1104330022.png)

**effective_prio()**  
![](media/1504548-20190427114348113-199075895.png)

---

![](media/1504548-20190427114406382-983589608.png)

---

![](media/1504548-20190427114417683-1430763421.png)

---

![](media/1504548-20190427114429593-365920191.png)

---

![](media/1504548-20190427114441658-1376822283.png)

---

![](media/1504548-20190427114455202-1726088377.png)

---

![](media/1504548-20190427114504758-425979671.png)

作者：[睿晞](https://www.cnblogs.com/tsruixi/)

出处：[https://www.cnblogs.com/tsruixi/](https://www.cnblogs.com/tsruixi/)

身处这个阶段的时候，一定要好好珍惜，这是我们唯一能做的，求学，钻研，为人，处事，交友……无一不是如此。

劝君莫惜金缕衣，劝君惜取少年时。花开堪折直须折，莫待无花空折枝。

曾有一个业界大牛说过这样一段话，送给大家： 　　“华人在计算机视觉领域的研究水平越来越高，这是非常振奋人心的事。我们中国错过了工业革命，错过了电气革命，信息革命也只是跟随状态。但人工智能的革命，我们跟世界上的领先国家是并肩往前跑的。能身处这个时代浪潮之中，做一番伟大的事业，经常激动的夜不能寐。”

本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文连接，否则保留追究法律责任的权利.

分类: [操作系统](https://www.cnblogs.com/tsruixi/category/1407678.html)

好文要顶 关注我 收藏该文 ![](media/icon_weibo_24.png) ![](media/wechat.png)

[![](media/20200224170230.png.jpg)](https://home.cnblogs.com/u/tsruixi/)

[睿晞](https://home.cnblogs.com/u/tsruixi/)  
[粉丝 - 15](https://home.cnblogs.com/u/tsruixi/followers/) [关注 - 17](https://home.cnblogs.com/u/tsruixi/followees/)  

+加关注

16

[«](https://www.cnblogs.com/tsruixi/p/10747266.html) 上一篇： [第六章6_2](https://www.cnblogs.com/tsruixi/p/10747266.html "发布于 2019-04-21 21:56")  
[»](https://www.cnblogs.com/tsruixi/p/10779175.html) 下一篇： [C语言中宏的相关知识](https://www.cnblogs.com/tsruixi/p/10779175.html "发布于 2019-04-27 16:29")

posted @ 2019-04-26 23:33  [睿晞](https://www.cnblogs.com/tsruixi/)  阅读(20003)  评论(5)  [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=10777242)  收藏  举报

刷新评论[刷新页面](https://www.cnblogs.com/tsruixi/p/10777242.html#)[返回顶部](https://www.cnblogs.com/tsruixi/p/10777242.html#top)

登录后才能查看或发表评论，立即 登录 或者 [逛逛](https://www.cnblogs.com/) 博客园首页

**编辑推荐：**  
· [现代图片性能优化及体验优化指南 - 懒加载及异步图像解码方案](https://www.cnblogs.com/coco1s/p/17162742.html)  
· [记一次 .NET 某家装 ERP 系统 内存暴涨分析](https://www.cnblogs.com/huangxincheng/p/17159384.html)  
· [［ASP.NET Core］标记帮助器——替换元素名称](https://www.cnblogs.com/tcjiaan/p/17157309.html)  
· [现代图片性能优化及体验优化指南 - 缩放精细化展示](https://www.cnblogs.com/coco1s/p/17146704.html)  
· [一个诡异的 Pulsar InterruptedException 异常](https://www.cnblogs.com/crossoverJie/p/17146856.html)  

**阅读排行：**  
· [2023年，消失的金三银四](https://www.cnblogs.com/peida/p/17162881.html)  
· [C#神器"BlockingCollection"类实现C#神仙操作](https://www.cnblogs.com/baibaomen-org/p/17162795.html)  
· [《HelloGitHub》第 83 期](https://www.cnblogs.com/xueweihan/p/17162624.html)  
· [解读C#编程中最容易忽略7种编写习惯！](https://www.cnblogs.com/xiongze520/p/17164309.html)  
· [我的十年编程路 序](https://www.cnblogs.com/wenhanxiao/p/17163583.html)  

### 公告

[](https://github.com/liuyj24)

昵称： [睿晞](https://home.cnblogs.com/u/tsruixi/)  
园龄： [4年4个月](https://home.cnblogs.com/u/tsruixi/ "入园时间：2018-10-07")  
粉丝： [15](https://home.cnblogs.com/u/tsruixi/followers/)  
关注： [17](https://home.cnblogs.com/u/tsruixi/followees/)

+加关注

### 积分与排名

-   积分 - 128947
-   排名 - 9877

### [随笔分类](https://www.cnblogs.com/tsruixi/post-categories)

-   [C/C++/Algorithm(38)](https://www.cnblogs.com/tsruixi/category/1434290.html)
-   [Deep Learning(9)](https://www.cnblogs.com/tsruixi/category/1438004.html)
-   [Hadoop(8)](https://www.cnblogs.com/tsruixi/category/1618331.html)
-   [Java基础(23)](https://www.cnblogs.com/tsruixi/category/1579143.html)
-   [Linux(5)](https://www.cnblogs.com/tsruixi/category/1434315.html)
-   [Machine Learning(3)](https://www.cnblogs.com/tsruixi/category/1427500.html)
-   [PATA(126)](https://www.cnblogs.com/tsruixi/category/1509903.html)
-   [PATB(18)](https://www.cnblogs.com/tsruixi/category/1502709.html)
-   [Python(15)](https://www.cnblogs.com/tsruixi/category/1458093.html)
-   [PyTorch(3)](https://www.cnblogs.com/tsruixi/category/1427494.html)
-   更多

### 随笔档案

-   [2021年2月(1)](https://www.cnblogs.com/tsruixi/archive/2021/02.html)
-   [2021年1月(11)](https://www.cnblogs.com/tsruixi/archive/2021/01.html)
-   [2020年7月(10)](https://www.cnblogs.com/tsruixi/archive/2020/07.html)
-   [2020年6月(48)](https://www.cnblogs.com/tsruixi/archive/2020/06.html)
-   [2020年5月(13)](https://www.cnblogs.com/tsruixi/archive/2020/05.html)
-   [2020年4月(4)](https://www.cnblogs.com/tsruixi/archive/2020/04.html)
-   [2020年3月(19)](https://www.cnblogs.com/tsruixi/archive/2020/03.html)
-   [2020年2月(36)](https://www.cnblogs.com/tsruixi/archive/2020/02.html)
-   [2020年1月(4)](https://www.cnblogs.com/tsruixi/archive/2020/01.html)
-   [2019年12月(12)](https://www.cnblogs.com/tsruixi/archive/2019/12.html)
-   更多

### [阅读排行榜](https://www.cnblogs.com/tsruixi/most-viewed)

-   [1. Lab1：Linux内核编译及添加系统调用（详细版）(20003)](https://www.cnblogs.com/tsruixi/p/10777242.html)
-   [2. ORA-01950：对表空间“”XXXX”无权限，解决办法(18486)](https://www.cnblogs.com/tsruixi/p/10815861.html)
-   [3. Python中的next()\iter()函数详解(16263)](https://www.cnblogs.com/tsruixi/p/10824636.html)
-   [4. Python之文件读写(csv文件，CSV库，Pandas库)(14444)](https://www.cnblogs.com/tsruixi/p/11395160.html)
-   [5. ISE 14.7安装教程最新版（Win10安装）(12146)](https://www.cnblogs.com/tsruixi/p/10650880.html)

### [推荐排行榜](https://www.cnblogs.com/tsruixi/most-liked)

-   [1. Lab1：Linux内核编译及添加系统调用（详细版）(16)](https://www.cnblogs.com/tsruixi/p/10777242.html)
-   [2. Python之文件读写(csv文件，CSV库，Pandas库)(6)](https://www.cnblogs.com/tsruixi/p/11395160.html)
-   [3. Python中的next()\iter()函数详解(5)](https://www.cnblogs.com/tsruixi/p/10824636.html)
-   [4. VSCode编写C/C++语言,配置文件和注意事项(3)](https://www.cnblogs.com/tsruixi/p/11566833.html)
-   [5. C++字符串转化为int类型的利器stoi函数详解(2)](https://www.cnblogs.com/tsruixi/p/12944470.html)

Copyright © 2023 睿晞  
Powered by .NET 7.0 on Kubernetes
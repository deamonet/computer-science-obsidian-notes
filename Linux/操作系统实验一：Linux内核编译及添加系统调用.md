[GojeonPa](https://wangyu0516.github.io/)

# 操作系统实验一：Linux内核编译及添加系统调用

10 May 2020

前段时间一直在折腾操作系统实验一的作业，前前后后我花了两个多星期在这作业上，踩了不少坑。不过最后好歹是搞成功了，也算是有点收获，刚好在这里记录一下。

作业内容要求如下：

> ##### (1).添加一个系统调用，实现对指定进程的 nice 值的修改或读取功能，并返回进程最新的 nice 值及优先级 prio。  
> 建议调用原型为：  
> int mysetnice(pid_t pid, int flag, int nicevalue, void __user * prio, void __user * nice);  
> 参数含义：  
>     pid：进程 ID。  
>     flag：若值为 0，表示读取 nice 值；若值为 1，表示修改 nice 值。  
>     nicevalue：为指定进程设置的新 nice 值。  
>     prio、nice：指向进程当前优先级 prio 及 nice 值。  
> 返回值：系统调用成功时返回 0，失败时返回错误码 EFAULT。
> 
> ##### (2).写一个简单的应用程序测试 (1) 中添加的系统调用。
> 
> ##### (3).若程序中调用了 Linux 的内核函数，要求深入阅读相关函数源码。

首先要搞清楚这几个概念：

##### (1).prio：  
prio值表示进程的优先级，进程prio值越小则优先级越高。

##### (2).nice：  
nice值表示进程优先级的修正数值，范围是从-20到19。正值表示调整到低优先级，负值表示调整到高优先级，值为零则表示不会调整该进程的优先级。

##### (3).pid_t：

```c
typedef __kernel_pid_t  pid_t;
typedef int __kernel_pid_t;
```

##### 可以看出pid_t其实就是int类型，使用pid_t是为了可移植性好一些，因为在不同的平台上可能会有：

```c
typedef int pid_t;
```

##### 或：

```c
typedef long pid_t;
```

##### (4).void __user*：  
(void __user*)arg中的arg是一个用户空间的地址，而用户空间和内核空间之间不能简单地使用指针进行传递，故linux内核提供了多个函数和宏用于内核空间和用户空间传递数据，例如copy_from_user，copy_to_user，get_user，put_user等。

大概了解完后就是正式开始了。

### 一.下载和解压缩内核源代码文件

​ 首先在[Linux官网](https://www.kernel.org/)下载内核源代码，在这里可以找到所有的内核版本。我下载的是4.19.120版本的，个人感觉下载的内核版本最好要高于虚拟机内核版本，之前下载4.4.218版本，莫名其妙碰到好多坑，一换4.19.120立马就好了。

​ 由于我虚拟机下载比较慢，所以我是先在Windows上下载好后通过虚拟机共享文件夹传到虚拟机中(传过来的共享文件会在/mnt/hgfs目录下)。

进入虚拟机后，将传过来的压缩文件复制到/usr/src目录下，分两步解压缩：

##### （1）xz -d linux-4.19.120.tar.xz

##### （2）tar –xvf linux-4.19.120.tar

注意：由于编译过程中会生成很多临时文件，所以要确保有足够的空闲空间，最好起码能有 20GB，我在建立虚拟机时预留了 40GB 磁盘空间(后来又扩展到60G…)。

### 二.清除残留的.config和.o文件

每次开始完全重新编译之前，需要删除所有的编译生成文件(.o文件)、内核配置文件(.config文件)和各种备份文件。后续如果编译过程中出现错误，再次开始完全重新编译之前也需要如此清理。进入 linux-4.19.120 目录， 执行以下命令：

##### #make mrproper

这里可能会提醒安装 ncurses 包，在 Ubuntu 中 ncurses 库的名字是 libncurses5-dev，所以安装命令是：

##### #apt-get install libncurses5-dev

安装完缺少的包之后再次执行命令：

##### #make mrproper

  
  

### 三.添加系统调用

#### 1. 分配系统调用号，修改系统调用表

查看系统调用表(arch/x86/entry/syscalls/syscall_64.tbl)，每个系统调用在表中占一表项，其格式为： <系统调用号> <系统调用名> <服务例程入口地址>

选择一个未使用的系统调用号进行分配，比如当前系统使用到 334 号，则新添加的系统调用可使用 335 号。确定调用号后，在 syscall_64.tbl 文件中为新调用添加一条记录，如图所示：

![系统调用表](media/系统调用表.png)

#### 2.申明系统调用服务例程原型

查看Linux 系统调用服务例程的原型声明(include/linux/syscalls.h)，并在文件末尾添加相应原型声明，如图所示：

![系统调用服务例程原型](media/系统调用服务例程原型.png)

#### 3.实现系统调用服务例程

为新调用mysetnice编写服务例程sys_mysetnice，添加在 sys.c 文件(kernel/sys.c)中，代码如下：

```c
SYSCALL_DEFINE5(mysetnice,  pid_t, pid, int, flag, int, nicevalue, void __user*, prio, void __user*, nice)
{
	struct pid *id;    	    //进程标识符结构体
	struct task_struct *pcb;    //进程结构体

	int new_nice, new_prio;

	id = find_get_pid(pid);     //根据进程ID号获得进程标识符
	pcb = pid_task(id, PIDTYPE_PID);  
	//根据进程标识符获得指定进程，PIDTYPE_PID为进程类型的PID，除此之外还有线程，会话等类型的PID
	
	new_nice = task_nice(pcb);    //获得该进程的nice值
	new_prio = task_prio(pcb);    //获得该进程的prio值

	if(flag == 0)      //flag为0的情况
	{
		copy_to_user(nice, &new_nice, sizeof(new_nice));
		copy_to_user(prio, &new_prio, sizeof(new_prio));
		//将数据从内核空间拷贝到用户空间中	
		return 0;
	}
	else if(flag == 1)       //flag为1的情况
	{
		set_user_nice(pcb, nicevalue);
		//设置该进程的nice值为nicevalue
		
		new_nice = task_nice(pcb);
		new_prio = task_prio(pcb);
 		//由于进程的nice值被修改了，故需要重新获取进程的nice值和prio值
		
		copy_to_user(nice, &new_nice, sizeof(new_nice));
		copy_to_user(prio, &new_prio, sizeof(new_prio));
		return 0;	
	}
	return EFAULT;
}
```

  
  

### 四.配置和编译内核

执行命令：

##### #make menuconfig

运行该命令过程中，可能会出现错误信息：fatal error: curses.h: No such file or directory。

执行命令安装套件 ncurses devel：

##### #apt-get install libncurses5-dev

在之后出现的对话框中选择 save 保存配置信息，文件名采用默认的.config，再选择 exit 退出。

编译内核执行命令(可使用 make -j4 (4核CPU)或make -j2(2核CPU)来加快编译速度)：

##### #make -j4

内核配置完成后，执行 make 命令开始编译内核，如果编译成功，则生成 Linux 启动映像文件 bzImage(位于./arch/x86_64/boot/bzImage中)。

编译过程中，可能会出现一些错误，通常都是因为缺少某个库，一般根据相应的错误提示，安装相应的包即可，然后重新编译：

##### (1).缺少 openssl：

##### #apt-get install libssl-dev

##### (2).缺少 bison：

##### #apt-get install bison

##### (3).缺少 flex :

##### #apt-get install flex

内核编译一般都需要挺长时间(我花了将近三个小时)，所以需要有耐心，可以在编译期间可以去做一些其他的事。

  
  

### 五.编译模块和安装内核

依次执行以下命令：

编译模块：

##### #make modules

安装模块：

##### #make modules_install

编译和安装模块也花了不少时间。

再安装内核：

##### #make install

配置 grub 引导程序(该命令会自动修改 grub)：

##### #update-grub2

  
  

### 六.重启系统

重启后系统就会自动启动新的内核(不过似乎是要安装的内核版本高于虚拟机的内核版本才会自动启动新的内核)，如果重启后内核还是之前的内核，可以采取以下操作：

执行命令：

##### #vim /etc/default/grub

将GRUB_TIMEOUT_STYLE = HIDDEN注释，并将GRUB_TIMEOUT的值(也就是启动选择界面会出现多少秒)设置为一个较大的值，这里改为了10。

![配置grub](media/配置grub.png)

然后重启时就可以在启动界面选择需要启动的内核，启动完成后进入终端查看内核版本：

##### #uname -a

![内核版本](media/内核版本.jpg)

  
  

### 七.编写程序测试系统调用

编写的程序代码如下：

```c
#include<stdio.h>
#include<unistd.h>
#include<sys/syscall.h>
#define __NR_MYSETNICE 335

int main()
{
	int flag1, flag2, nicevalue1, nicevalue2;
	int nice, prio;
	pid_t id;

	id = getpid();
	
	printf("分别输入第一次的flag值和nicevalue值：");
	scanf("%d %d", &flag1, &nicevalue1);
	printf("分别输入第二次的flag值和nicevalue值：");
	scanf("%d %d", &flag2, &nicevalue2);
	printf("分别输入第三次的flag值和nicevalue值：");
	scanf("%d %d", &flag3, &nicevalue3);


	syscall(__NR_MYSETNICE, id, flag1, nicevalue1, &prio, &nice);
	printf("第一次: prio:%d, nice:%d\n", prio, nice);

	syscall(__NR_MYSETNICE, id, flag2, nicevalue2, &prio, &nice);
	printf("第二次: prio:%d, nice:%d\n", prio, nice);

	syscall(__NR_MYSETNICE, id, flag3, nicevalue3, &prio, &nice);
	printf("第三次: prio:%d, nice:%d\n", prio, nice);
	
	return 0;
}
```

用户态程序输出如下：

![测试程序运行结果](media/测试程序运行结果.png)

用户空间的进程初始化时默认是prio=20，nice=0，并且prio和nice有对应关系：prio = nice + 20。

到了这一步，差不多算是大功告成了，不过仍然还有一些地方没太搞懂，以后有时间再看看(下次一定)。

[Github](https://github.com/wangyu0516/)